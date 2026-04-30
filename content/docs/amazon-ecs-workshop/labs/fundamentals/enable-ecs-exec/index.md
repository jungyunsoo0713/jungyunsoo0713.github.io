---
title: "Enable ECS Exec"
weight: 5
draft: true
---
> **작성일:** 2026-04-30 | **수정일:** 2026-04-30

이번 섹션에서는 ECS Exec 기능을 활성화하여, 실행 중인 컨테이너 내부에서 명령어를 실행하거나 셸에 접속하는 방법을 배웁니다. ECS Exec을 사용하면 EC2 인스턴스나 Fargate에서 실행 중인 ECS 태스크의 컨테이너에 직접 접근해 문제를 확인할 수 있습니다. SSH를 통해 호스트에 직접 접속할 필요가 없으므로, 이 방식은 간편하고 유용합니다.

ECS Exec을 활성화하는 과정은 총 5단계로 구성됩니다.

## 1. Check IAM role for the user

```bash
aws iam get-role-policy \
    --role-name $(aws sts get-caller-identity --query 'Arn' | cut -d'/' -f2) \
    --policy-name AmazonECSExecCommand
```

위 명령어는 현재 명령을 실행한 권한 주체를 확인(`--role-name $(aws sts get-caller-identity --query 'Arn' | cut -d'/' -f2)`)하고, 그 주체가 속한 IAM Role에 ECS Exec 권한 정책이 있는지 확인합니다. 

ARN 값은 `arn:aws:sts::123456789012:assumed-role/EcsWorkshopIdeRoleIdeRole/i-xxxx` 이런 형태를 가집니다. 이는 현재 AWS CLI 명령이 `EcsWorkshopIdeRoleIdeRole`이라는 IAM Role을 임시로 사용해 실행되고 있다는 의미입니다. 뒤의 `i-xxxx`는 해당 Role을 사용 중인 세션 이름으로, 실습 환경에서는 보통 EC2 인스턴스 ID가 들어갑니다.

요약하자면, 위 명령어는 현재 명령을 실행하는 권한 주체의 IAM Role에 ECS Exec 권한이 있는지 확인합니다. 이 권한이 있어야 ECS Exec을 사용할 수 있습니다.

위 명령어의 실행 결과입니다.

```bash
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

명령어를 실행하는 주체의 IAM Role에 ECS Exec 권한이 존재함을 확인할 수 있습니다.

>`EcsWorkshopIdeRoleIdeRole`은 실습 IDE 환경에 연결된 IAM Role입니다. 이 Role은 실습 터미널에서 AWS CLI 명령어를 실행할 때 필요한 권한을 제공합니다.

---

## 2. Check the IAM role for the ECS Task Role

```bash
aws iam get-role-policy \
    --role-name  retailStoreEcsTaskRole \
    --policy-name $(aws iam list-role-policies --role-name retailStoreEcsTaskRole --query 'PolicyNames[0]' --output text)
```

위 명령어는 `retailStoreEcsTaskRole`에 붙어 있는 첫 번째 인라인 정책 이름을 자동으로 찾고, 그 정책의 상세 권한 내용을 조회합니다. `retailStoreEcsTaskRole`에 ECS Exec에 필요한 권한이 들어 있는지 확인하기 위해서 이 명령어를 사용합니다. 

ECS Exec은 SSM 통신을 위해 Task IAM Role 권한이 필요합니다. 초기 CloudFormation 스택 생성 과정에서 `retailStoreEcsTaskRole`이 이미 생성되어 있으며, UI 서비스의 Task Definition에서 이 Role을 `taskRoleArn`으로 사용하고 있습니다. 

위 명령어의 실행 결과입니다.

```bash
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

명령어의 실행 결과를 보면 `retailStoreEcsTaskRole`에 `ssmmessages` 관련 권한이 포함되어 있습니다. ECS Exec은 컨테이너와 통신하기 위해 SSM 메시지 채널을 사용하므로, `ssmmessages:CreateControlChannel`, `ssmmessages:CreateDataChannel`, `ssmmessages:OpenControlChannel`, `ssmmessages:OpenDataChannel` 권한이 필요합니다. 따라서 이 Role은 ECS Exec을 사용할 수 있는 기본 권한을 가지고 있습니다.

---

## 3. Set up the environment

ECS Exec을 사용하려면 AWS CLI와 AWS CLI용 Session Manager Plugin이 필요합니다. 이 워크숍에서 제공하는 IDE 환경에는 두 도구가 이미 설치되어 있으므로 별도로 설치할 필요가 없습니다. 다만 개인 로컬 환경이나 실무 환경에서 ECS Exec을 사용하려면 AWS 공식 문서를 참고해 AWS CLI와 Session Manager Plugin을 직접 설치해야 합니다.

AWS CLI와 Session Manager Plugin는 ECS Exec을 실행하는 곳에 설치되어 있어야 합니다.
워크숍 IDE에서 실행하면 IDE 환경에 설치되어 있으면 되고, 로컬 PC에서 실행하면 로컬 PC에 설치해야 합니다.

## 4. Enable Amazon ECS Exec on the task

```bash
aws ecs update-service \
	    --cluster retail-store-ecs-cluster \
	    --service ui \
	    --task-definition retail-store-ecs-ui \
	    --enable-execute-command \
	    --force-new-deployment
```

위 명령어를 실행해 ECS Exec을 활성화합니다. `--enable-execute-command` 옵션은 ECS Exec 기능을 활성화합니다. `--force-new-deployment` 옵션은 새 배포를 강제로 시작합니다. ECS Exec 설정은 이미 실행 중인 기존 Task에 바로 적용되는 것이 아니라 새로 시작되는 Task에 적용됩니다. 따라서 `--force-new-deployment` 옵션을 사용해 기존 Task를 새 Task로 교체해야 합니다.

위 명령어의 실행 결과입니다.

```bash
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

`--enable-execute-command` 옵션으로 인해 ECS Exec이 활성화 되었습니다.

```bash
"enableExecuteCommand": true
```

`--force-new-deployment` 옵션으로 인해 새 배포가 시작되었습니다.

```bash
"rolloutState": "IN_PROGRESS",
"rolloutStateReason": "ECS deployment ecs-svc/6216346761948112104 in progress."
```

아래 명령어를 실행하여, ECS Service가 안정화될 때까지 기다립니다.

```bash
echo_g "Waiting for service to stabilize..."

aws ecs wait services-stable --cluster retail-store-ecs-cluster --services ui
```

아래 명령어는 `ui` 서비스에서 실행 중인 Task들 중 ECS Exec이 활성화된 Task를 찾고, 그중 첫 번째 Task ARN을 `ECS_EXEC_TASK_ARN` 환경 변수에 저장합니다.

```bash
ECS_EXEC_TASK_ARN=$(aws ecs list-tasks --cluster retail-store-ecs-cluster \
    --service-name ui --query 'taskArns[]' --output text | \
    xargs -n1 aws ecs describe-tasks --cluster retail-store-ecs-cluster --tasks | \
    jq -r '.tasks[] | select(.enableExecuteCommand == true) | .taskArn' | \
    head -n 1)
```

아래 명령어로 `ECS_EXEC_TASK_ARN` 환경 변수를 출력하면

```bash
echo_c $ECS_EXEC_TASK_ARN
```

`arn:aws:ecs:ap-northeast-3:802104112480:task/retail-store-ecs-cluster/613e4c31983a4dd29258e0c39ac058ae` 이 값을 얻을 수 있습니다.

## 5. Connect to the ECS Task

```bash
if [ -z "${ECS_EXEC_TASK_ARN}" ]; then echo "ECS_EXEC_TASK_ARN is not correctly configured!"; else
aws ecs execute-command --cluster retail-store-ecs-cluster \
    --task $ECS_EXEC_TASK_ARN \
    --container application \
    --interactive \
    --command "/bin/bash"
fi
```

위 명령어는 먼저 `ECS_EXEC_TASK_ARN` 변수에 접속할 Task ARN이 들어 있는지 확인합니다. 값이 비어 있으면 오류 메시지를 출력하고, 값이 있으면 `aws ecs execute-command` 명령어를 실행해 `application` 컨테이너 안에서 `/bin/bash` 셸을 대화형으로 실행합니다. 이를 통해 SSH 없이 실행 중인 ECS 컨테이너 내부에 접속할 수 있습니다.

위 명령어의 실행 결과입니다.

```bash

The Session Manager plugin was installed successfully. Use the AWS CLI to start a session.


Starting session with SessionId: ecs-execute-command-h58qgzkd8fpy9j6gkxa7rrkkii
bash-5.2# 
```

`bash-5.2#` 프롬프트가 표시되면 ECS Exec을 통해 컨테이너 내부 셸에 접속한 상태입니다. 이는 컨테이너 내부에서 Bash 5.2 버전의 셸이 열렸고, 현재 사용자가 일반적으로 root 사용자(`#`)임을 의미합니다. 이제 일반적인 Linux 명령어를 실행해 컨테이너 내부 파일, 프로세스, 환경 변수, 애플리케이션 상태 등을 확인할 수 있습니다.

```bash

The Session Manager plugin was installed successfully. Use the AWS CLI to start a session.


Starting session with SessionId: ecs-execute-command-h58qgzkd8fpy9j6gkxa7rrkkii
bash-5.2# cat /etc/os-release
NAME="Amazon Linux"
VERSION="2023"
ID="amzn"
ID_LIKE="fedora"
VERSION_ID="2023"
PLATFORM_ID="platform:al2023"
PRETTY_NAME="Amazon Linux 2023.8.20250721"
ANSI_COLOR="0;33"
CPE_NAME="cpe:2.3:o:amazon:amazon_linux:2023"
HOME_URL="https://aws.amazon.com/linux/amazon-linux-2023/"
DOCUMENTATION_URL="https://docs.aws.amazon.com/linux/"
SUPPORT_URL="https://aws.amazon.com/premiumsupport/"
BUG_REPORT_URL="https://github.com/amazonlinux/amazon-linux-2023"
VENDOR_NAME="AWS"
VENDOR_URL="https://aws.amazon.com/"
SUPPORT_END="2029-06-30"
bash-5.2# whoami
root
bash-5.2# df -h
Filesystem      Size  Used Avail Use% Mounted on
overlay          21G  1.6G   18G   8% /
tmpfs            64M     0   64M   0% /dev
shm             1.9G     0  1.9G   0% /dev/shm
tmpfs           1.9G     0  1.9G   0% /sys/fs/cgroup
/dev/root       1.1G  1.1G     0 100% /dev/init
/dev/nvme1n1     21G  1.6G   18G   8% /etc/hosts
/dev/nvme0n1p8  2.9G   15M  2.9G   1% /managed-agents/execute-command/certs
tmpfs           1.9G     0  1.9G   0% /proc/acpi
tmpfs           1.9G     0  1.9G   0% /sys/firmware
tmpfs           1.9G     0  1.9G   0% /proc/scsi
bash-5.2# 
```

---

이번 모듈에서는 다음 내용을 배웠습니다.

- 컨테이너 애플리케이션의 기반이 되는 ECS 클러스터를 생성하고 설정했습니다.
- CPU, 메모리, 컨테이너 설정을 포함한 Task Definition을 작성했습니다.
- ECS Service를 생성해 원하는 개수의 Task가 계속 실행되도록 구성했습니다.
- Task Definition과 ECS Service를 업데이트해 애플리케이션 설정을 변경했습니다.
- ECS Exec을 활성화해 실행 중인 컨테이너 내부에 접속하고 문제를 확인하는 방법을 배웠습니다.

