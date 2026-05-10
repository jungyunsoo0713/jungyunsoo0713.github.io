---
title: "Enable ECS Exec"
weight: 5
---
> **작성일:** 2026-05-10 | **수정일:** 2026-05-10

이번 섹션에서는 터미널을 통해 ECS 서비스의 태스크에서 실행 중인 컨테이너에 접속하여 셸 명령어를 실행할 수 있는 ECS Exec 기능을 활성화합니다. 셸 명령어를 실행해서 컨테이너의 상태를 조회하거나 특정 명령을 할 수 있습니다. 

>간단히 말하자면, 컨테이너도 리눅스 환경에서 실행되기 때문에, 터미널로 셸 명령어를 실행하는 것으로 볼 수 있습니다. 다만 SSH로 접속하는 것이 아니라 ECS Exec 기능을 사용합니다.

참고로 ECS Exec 기능을 사용해 입력된 명령어는 CloudWatch Logs에 로그로 기록할 수 있습니다.

---

ECS Exec 기능을 활성화 하기 위해서는 다음 5가지 절차가 필요합니다.
1. 사용자의 IAM Role을 확인합니다 - 사용자가 사용하는 IAM Role의 ECS Exec 실행 권한이 있는지 확인합니다.
2. ECS Task Role의 IAM Role을 확인합니다 - ECS Task Role에 ECS Exec 기능을 위한 `ssmmessages` 채널 관련 권한이 있는지 확인합니다.

> 참고로 ECS Task Execution Role은 ECS Exec 기능과 직접적인 연관이 없습니다. ECS Task Execution Role은 ECS 태스크를 실행하기 위해서 ECR에서 컨테이너 이미지를 가져오고 CloudWatch Logs에 로그를 기록할 수 있는 IAM Role이고, ECS Task Role은 ECS 태스크가 실행 중에 사용하는 IAM Role입니다. 이 IAM Role에는 ECS Exec 기능에 필요한 `ssmmessages` 관련 IAM Policy도 포함되어 있어야 합니다.

3. 환경을 설정합니다 - AWS CLI와 Session Manager plugin for the AWS CLI을 설치합니다. 
4. 태스크에 Amazon ECS Exec 기능을 활성화합니다 - ECS 서비스의 태스크가 ECS Exec 기능을 사용할 수 있도록 활성화합니다
5. ECS 태스크에 접속합니다 - AWS CLI를 이용해서 ECS 태스크에 접속합니다.

---

## 1. 사용자의 IAM Role을 확인합니다

다음 명령어는 현재 AWS CLI를 실행 중인 IAM Role에 `AmazonECSExeCommand`라는 Inline Policy가 붙어있는지 확인합니다.

```bash
	aws iam get-role-policy \
    --role-name $(aws sts get-caller-identity --query 'Arn' | cut -d'/' -f2) \
    --policy-name AmazonECSExecCommand
```

`aws iam get-role-policy`는 특정 IAM Role에 붙어있는 Inline Policy의 내용을 조회하는 명령어입니다.

`--role-name`은 IAM Role의 이름을 지정하는 옵션입니다. `aws sts get-caller-identity`는 이 AWS CLI를 실행하고 있는 IAM 주체(IAM Principal, IAM User/Role)를 확인합니다. `--query 'Arn'`은 이 명령어의 출력에서 `Arn` 필드의 값만 뽑아냅니다.

`--policy-name`은 조회할 Inline Policy의 이름을 지정합니다. 

요약하자면, `--role-name`은 어떤 IAM Role을 볼지 정하고, `--policy-name`은 해당 IAM Role에서 조회할 Inline Policy 이름을 지정합니다.

이 명령어의 출력입니다.

```json
{
  "RoleName": "EcsWorkshopIdeRoleIdeRole",
  "PolicyName": "AmazonECSExecCommand",
  "PolicyDocument": {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Action": [
          "ecs:DescribeTasks",
          "ecs:ExecuteCommand"
        ],
        "Resource": [
          "arn:aws:ecs:ap-northeast-3:802104112480:cluster/*",
          "arn:aws:ecs:ap-northeast-3:802104112480:task/retail-store-ecs-cluster/*"
        ],
        "Effect": "Allow"
      }
    ]
  }
}
```

`"ecs:ExecuteCommand"`이 허용되어 있는 것을 확인할 수 있습니다.

>Inline Policy는 특정 IAM User/IAM Role/IAM Group에 직접 붙어있는 IAM Policy입니다. Managed IAM Policy는 IAM User/IAM Role/IAM Group에 붙였다 떼었다 할 수 있지만, Inline Policy는 특정 IAM User/IAM Role/IAM Group에 종속되어 있습니다. 이는 IAM Policy가 특정 IAM User/IAM Role/IAM Group에 종속되어 있어야만 하는 경우에 사용됩니다.

>Inline Policy가 필요한 예시를 들어보겠습니다. 만약 어떤 Managed IAM Policy가 회사의 기밀 데이터베이스에 접근할 수 있는 권한을 가지고 있다고 가정합시다. 이 Managed IAM Policy는 다른 IAM User/Role/Group에 붙을 수 있기 때문에 실수로 이 Managed IAM Policy가 붙어서는 안 되는 IAM User/Role/Group에 붙을 수 있습니다. 이 경우 의도되지 않은 주체가 기밀 데이터베이스에 접근할 수 있게 됩니다. 이러한 리스크를 줄이기 위해 IAM Policy가 특정 IAM User/Role/Group에 종속되어 있는 Inline Policy가 필요합니다.

---

## 2. ECS Task Role의 IAM Role을 확인합니다

다음 명령어는 `retailStoreEcsTaskRole` IAM Role의 

```bash
aws iam get-role-policy \
    --role-name  retailStoreEcsTaskRole \
    --policy-name $(aws iam list-role-policies --role-name retailStoreEcsTaskRole --query 'PolicyNames[0]' --output text)
```

`aws iam list-role-policies`는 특정 IAM Role에 붙어있는 Inline Policy의 이름들을 조회하는 명령어입니다.

`--query 'PolicyNames[0]'`는 이 이름들(이름 목록) 중 첫 번째 Inline Policy 이름을 조회하게 합니다.

이 명령어의 출력입니다.

```json
{
  "RoleName": "retailStoreEcsTaskRole",
  "PolicyName": "BasicRetailStoreEcsTaskRoleDefaultPolicy9759E157",
  "PolicyDocument": {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Action": [
          "ssmmessages:CreateControlChannel",
          "ssmmessages:CreateDataChannel",
          "ssmmessages:OpenControlChannel",
          "ssmmessages:OpenDataChannel"
        ],
        "Resource": "*",
        "Effect": "Allow"
      },
      {
        "Condition": {
          "Bool": {
            "elasticfilesystem:AccessedViaMountTarget": "true"
          }
        },
        "Action": [
          "elasticfilesystem:ClientMount",
          "elasticfilesystem:ClientRootAccess",
          "elasticfilesystem:ClientWrite"
        ],
        "Resource": "arn:aws:elasticfilesystem:ap-northeast-3:802104112480:file-system/fs-0429eb7819aeec1c2",
        "Effect": "Allow"
      }
    ]
  }
}
```

`"ssmmessages:CreateControlChannel"`, `"ssmmessages:CreateDataChannel"`, `"ssmmessages:OpenControlChannel"`, `"ssmmessages:OpenDataChannel"` 들이 허용되어 있는 것을 확인할 수 있습니다. 이 작업들이 허용되어야 ECS Exec 기능에 필요한 메시지 채널을 사용할 수 있습니다.

---

## 3. 환경을 설정합니다

ECS Exec 기능을 사용하려면 AWS CLI와 Session Manager plugin for the AWS CLI가 필요합니다. 랩 실습에서는 IDE에 이미 설치되어 있으므로 따로 설치할 필요는 없지만, 개인 실습 환경이나 실무에서는 이 둘이 먼저 설치되어 있어야 ECS Exec 기능을 사용할 수 있습니다.

---

## 4. ECS 서비스의 태스크가 ECS Exec 기능을 사용할 수 있도록 활성화합니다

다음 명령어를 실행하여 ECS 서비스를 업데이트 합니다. 

```bash
aws ecs update-service \
    --cluster retail-store-ecs-cluster \
    --service ui \
    --task-definition retail-store-ecs-ui \
    --enable-execute-command \
    --force-new-deployment
```

`--enable-execute-command`는 ECS 서비스 업데이트 시 ECS Exec 기능을 사용할 수 있도록 활성화합니다.

`--force-new-deployment`는 새 ECS 서비스의 배포를 강제로 시작합니다. ECS Exec 기능은 새 태스크가 배포되었을 때 활성화됩니다.

>이전에 애플리케이션 UI 업데이트 시, `aws ecs update-service` 명령어에 `--force-new-deployment`를 쓰지 않은 이유는, `--task-definition` 값이 바뀔 때 ECS가 자동으로 새 태스크를 배포하기 때문입니다. 즉, 새로운 Task Definition 리비전으로 ECS 서비스를 업데이트할 때 자동으로 ECS가 새 태스크를 배포합니다.

이 명령어의 출력입니다.

```json
{
  "service": {
    "serviceArn": "arn:aws:ecs:ap-northeast-3:802104112480:service/retail-store-ecs-cluster/ui",
    "serviceName": "ui",
    "clusterArn": "arn:aws:ecs:ap-northeast-3:802104112480:cluster/retail-store-ecs-cluster",
    "loadBalancers": [
      {
        "targetGroupArn": "arn:aws:elasticloadbalancing:ap-northeast-3:802104112480:targetgroup/BlueTargetGroup/d981484920c49db1",
        "containerName": "application",
        "containerPort": 8080
      }
    ],
    "serviceRegistries": [],
    "status": "ACTIVE",
    "desiredCount": 2,
    "runningCount": 2,
    "pendingCount": 0,
    "launchType": "FARGATE",
    "platformVersion": "LATEST",
    "platformFamily": "Linux",
    "taskDefinition": "arn:aws:ecs:ap-northeast-3:802104112480:task-definition/retail-store-ecs-ui:4",
    "deploymentConfiguration": {
      "deploymentCircuitBreaker": {
        "enable": false,
        "rollback": false
      },
      "maximumPercent": 200,
      "minimumHealthyPercent": 100,
      "strategy": "ROLLING",
      "bakeTimeInMinutes": 0
    },
    "deployments": [
      {
        "id": "ecs-svc/6216346761948112104",
        "status": "PRIMARY",
        "taskDefinition": "arn:aws:ecs:ap-northeast-3:802104112480:task-definition/retail-store-ecs-ui:4",
        "desiredCount": 0,
        "pendingCount": 0,
        "runningCount": 0,
        "failedTasks": 0,
        "createdAt": "2026-04-30T03:00:42.816000+00:00",
        "updatedAt": "2026-04-30T03:00:42.816000+00:00",
        "launchType": "FARGATE",
        "platformVersion": "1.4.0",
        "platformFamily": "Linux",
        "networkConfiguration": {
          "awsvpcConfiguration": {
            "subnets": [
              "subnet-070fc8b7200cdf7bb",
              "subnet-06d97f13d5fa2d612"
            ],
            "securityGroups": [
              "sg-0c092c5ec3f4e4c45"
            ],
            "assignPublicIp": "DISABLED"
          }
        },
        "rolloutState": "IN_PROGRESS",
        "rolloutStateReason": "ECS deployment ecs-svc/6216346761948112104 in progress."
      },
      {
        "id": "ecs-svc/5678755162203410295",
        "status": "ACTIVE",
        "taskDefinition": "arn:aws:ecs:ap-northeast-3:802104112480:task-definition/retail-store-ecs-ui:4",
        "desiredCount": 2,
        "pendingCount": 0,
        "runningCount": 2,
        "failedTasks": 0,
        "createdAt": "2026-04-30T02:05:17.056000+00:00",
        "updatedAt": "2026-04-30T02:07:46.073000+00:00",
        "launchType": "FARGATE",
        "platformVersion": "1.4.0",
        "platformFamily": "Linux",
        "networkConfiguration": {
          "awsvpcConfiguration": {
            "subnets": [
              "subnet-070fc8b7200cdf7bb",
              "subnet-06d97f13d5fa2d612"
            ],
            "securityGroups": [
              "sg-0c092c5ec3f4e4c45"
            ],
            "assignPublicIp": "DISABLED"
          }
        },
        "rolloutState": "COMPLETED",
        "rolloutStateReason": "ECS deployment ecs-svc/5678755162203410295 completed."
      }
    ],
    "roleArn": "arn:aws:iam::802104112480:role/aws-service-role/ecs.amazonaws.com/AWSServiceRoleForECS",
    "events": [
      {
        "id": "a2b132d4-64b4-43bf-a94e-1ea7bf51a56d",
        "createdAt": "2026-04-30T02:07:46.079000+00:00",
        "message": "(service ui) has reached a steady state."
      },
      {
        "id": "9b72259b-7694-4344-bb17-1dbda7d20b6e",
        "createdAt": "2026-04-30T02:07:46.078000+00:00",
        "message": "(service ui) (deployment ecs-svc/5678755162203410295) deployment completed."
      },
      {
        "id": "58707235-bb94-4be3-9698-9fe310213b33",
        "createdAt": "2026-04-30T02:06:44.015000+00:00",
        "message": "(service ui, taskSet ecs-svc/7142340952746948336) has begun draining connections on 2 tasks."
      },
      {
        "id": "ee7c7490-ad4f-41d7-903b-67339cf4fae8",
        "createdAt": "2026-04-30T02:06:44.011000+00:00",
        "message": "(service ui) deregistered 2 targets in (target-group arn:aws:elasticloadbalancing:ap-northeast-3:802104112480:targetgroup/BlueTargetGroup/d981484920c49db1)"
      },
      {
        "id": "4247e268-1ad6-4185-99f6-b28eee41fafa",
        "createdAt": "2026-04-30T02:06:42.457000+00:00",
        "message": "(service ui) has stopped 2 running tasks: (task 3e7a1395d08a42a89371db1f2755625e) (task 5e1c383c0e4a4d4a8beb0b98276134a2)."
      },
      {
        "id": "6f258c8c-e3d7-4d0b-812a-9c61a2f42561",
        "createdAt": "2026-04-30T02:06:01.418000+00:00",
        "message": "(service ui) registered 2 targets in (target-group arn:aws:elasticloadbalancing:ap-northeast-3:802104112480:targetgroup/BlueTargetGroup/d981484920c49db1)"
      },
      {
        "id": "446a4936-80ec-4234-b6cb-ba6166f00311",
        "createdAt": "2026-04-30T02:05:31.480000+00:00",
        "message": "(service ui) has started 2 tasks: (task 6a127288b5fa4ad7b70e2b340d705cc0) (task ef673d3a5c6d4401993bdbe5f77df574)."
      },
      {
        "id": "bffac50d-89a0-48c5-87f3-777a0200e7f8",
        "createdAt": "2026-04-30T01:01:10.917000+00:00",
        "message": "(service ui) has reached a steady state."
      },
      {
        "id": "f7248583-2d1a-480c-958b-fc99bc7cbde9",
        "createdAt": "2026-04-30T01:01:10.916000+00:00",
        "message": "(service ui) (deployment ecs-svc/7142340952746948336) deployment completed."
      },
      {
        "id": "4d5b30d0-2541-4771-9c59-308f8a5e9530",
        "createdAt": "2026-04-30T01:00:30.305000+00:00",
        "message": "(service ui) registered 1 targets in (target-group arn:aws:elasticloadbalancing:ap-northeast-3:802104112480:targetgroup/BlueTargetGroup/d981484920c49db1)"
      },
      {
        "id": "9933efbb-ce13-452f-bd87-e6821f877079",
        "createdAt": "2026-04-30T01:00:00.213000+00:00",
        "message": "(service ui) has started 2 tasks: (task 3e7a1395d08a42a89371db1f2755625e) (task 5e1c383c0e4a4d4a8beb0b98276134a2)."
      }
    ],
    "createdAt": "2026-04-30T00:59:43.081000+00:00",
    "currentServiceRevisions": [
      {
        "arn": "arn:aws:ecs:ap-northeast-3:802104112480:service-revision/retail-store-ecs-cluster/ui/6216346761948112104",
        "requestedTaskCount": 0,
        "runningTaskCount": 0,
        "pendingTaskCount": 0
      },
      {
        "arn": "arn:aws:ecs:ap-northeast-3:802104112480:service-revision/retail-store-ecs-cluster/ui/5678755162203410295",
        "requestedTaskCount": 2,
        "runningTaskCount": 2,
        "pendingTaskCount": 0
      }
    ],
    "placementConstraints": [],
    "placementStrategy": [],
    "networkConfiguration": {
      "awsvpcConfiguration": {
        "subnets": [
          "subnet-070fc8b7200cdf7bb",
          "subnet-06d97f13d5fa2d612"
        ],
        "securityGroups": [
          "sg-0c092c5ec3f4e4c45"
        ],
        "assignPublicIp": "DISABLED"
      }
    },
    "healthCheckGracePeriodSeconds": 30,
    "schedulingStrategy": "REPLICA",
    "deploymentController": {
      "type": "ECS"
    },
    "createdBy": "arn:aws:iam::802104112480:role/EcsWorkshopIdeRoleIdeRole",
    "enableECSManagedTags": false,
    "propagateTags": "NONE",
    "enableExecuteCommand": true,
    "availabilityZoneRebalancing": "ENABLED",
    "resourceManagementType": "CUSTOMER"
  }
}
```

---

위 ECS 서비스를 업데이트하는 명령어를 실행한 후 다음 명령어를 사용하여 ECS 서비스가 안정화될 때까지 기다립니다.

```bash
echo_g "Waiting for service to stabilize..."

aws ecs wait services-stable --cluster retail-store-ecs-cluster --services ui
```

---

다음 명령어를 실행하여 실행 중인 UI 서비스 태스크 중 ECS Exec 기능이 활성화된 태스크의 ARN을 `ECS_EXEC_TASK_ARN` 변수에 저장합니다.

```bash
ECS_EXEC_TASK_ARN=$(aws ecs list-tasks --cluster retail-store-ecs-cluster \
    --service-name ui --query 'taskArns[]' --output text | \
    xargs -n1 aws ecs describe-tasks --cluster retail-store-ecs-cluster --tasks | \
    jq -r '.tasks[] | select(.enableExecuteCommand == true) | .taskArn' | \
    head -n 1)
```

다음 명령어를 실행하여 `ECS_EXEC_TASK_ARN` 변수의 값을 출력합니다.

```bash
echo_c $ECS_EXEC_TASK_ARN
```

이 명령어의 출력입니다.

```bash
`arn:aws:ecs:ap-northeast-3:802104112480:task/retail-store-ecs-cluster/613e4c31983a4dd29258e0c39ac058ae`
```

---

## 5. ECS 태스크에 접속합니다

다음 명령어를 실행하여 ECS Exec기능으로 컨테이너에 접속합니다. 

```bash
if [ -z "${ECS_EXEC_TASK_ARN}" ]; then echo "ECS_EXEC_TASK_ARN is not correctly configured!"; else
aws ecs execute-command --cluster retail-store-ecs-cluster \
    --task $ECS_EXEC_TASK_ARN \
    --container application \
    --interactive \
    --command "/bin/bash"
fi
```

`ECS_EXEC_TASK_ARN` 변수값이 존재하면 `aws ecs execute-command` 명령어를 실행하여 ECS Exec기능을 사용해 `/bin/bash` 셸을 실행합니다.

>리마인드 하자면, ECS Exec 기능은 특정 컨테이너에서 명령어를 실행하게 해주는 기능입니다. GUI 대신에 CLI(AWS CLI)를 사용해서 컨테이너 내부에 명령어를 전달하여 컨테이너 내부를 조작한다고 생각하면 됩니다. `aws ecs execute-command \ ... \ --command "ls"` 같은 명령어를 실행할 수 있습니다. 하지만 이 명령어들을 지속적으로 실행하고 출력을 보기 위해서는 컨테이너의 셸을 실행하는 것이 간편합니다. 그렇기 때문에 `aws ecs execute-command \ ... \ --command "/bin/bash"`와 같은 명령어로 셸을 실행하여 대화형 터미널을 사용하는 것입니다.

이 명령어의 출력입니다.

```bash

The Session Manager plugin was installed successfully. Use the AWS CLI to start a session.


Starting session with SessionId: ecs-execute-command-h58qgzkd8fpy9j6gkxa7rrkkii
bash-5.2# 
```

다음과 같은 리눅스 명령어를 사용할 수 있습니다.

```bash
cat /etc/os-release

df -h
```

AWS CLI에서 직접 `aws ecs execute-command \ ... \ --command "cat /etc/os-release"` 명령어를 사용하여 컨테이너 내부에서 명령어를 실행할 수도 있지만, 컨테이너의 셸을 실행시키는 것이 더 간편합니다.
