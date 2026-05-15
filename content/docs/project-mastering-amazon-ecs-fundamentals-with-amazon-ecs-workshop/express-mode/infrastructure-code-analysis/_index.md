---
title: "Express Mode - Infrastructure Code Analysis"
weight: 1
---
> **작성일:** 2026-05-15 | **수정일:** 2026-05-15
```json
{
  "PermissionsBoundary2D0B48CC": {
   "Type": "AWS::IAM::ManagedPolicy",
   "Properties": {
    "Description": "",
    "ManagedPolicyName": "ecsWorkshopIdeRoleBoundary",
    "Path": "/",
    "PolicyDocument": {
     "Statement": [
      {
       "Action": [
        "acm-pca:DescribeCertificateAuthority",
        "acm-pca:GetCertificate",
        "acm-pca:GetCertificateAuthorityCertificate",
        "acm-pca:IssueCertificate",
        "acm-pca:ListCertificateAuthorities",
        "acm:DescribeCertificate",
        "acm:ImportCertificate",
        "acm:ListCertificates",
        "application-autoscaling:DescribeScalingPolicies",
        "application-autoscaling:PutScalingPolicy",
        "application-autoscaling:PutScheduledAction",
        "application-autoscaling:RegisterScalableTarget",
        "cloudwatch:*",
        "dynamodb:*",
        "ec2:AssociateAddress",
        "ec2:AuthorizeSecurityGroupIngress",
        "ec2:CreateTags",
        "ec2:CreateVpcEndpoint",
        "ec2:DeleteTags",
        "ec2:DeleteVpcEndpoints",
        "ec2:DescribeAddresses",
        "ec2:DescribeInstances",
        "ec2:DescribeManagedPrefixLists",
        "ec2:DescribeRouteTables",
        "ec2:DescribeSecurityGroupRules",
        "ec2:DescribeSecurityGroups",
        "ec2:DescribeSubnets",
        "ec2:DescribeTags",
        "ec2:DescribeVolumes",
        "ec2:DescribeVpcEndpoints",
        "ec2:DescribeVpcs",
        "ec2:RevokeSecurityGroupIngress",
        "ec2messages:AcknowledgeMessage",
        "ec2messages:DeleteMessage",
        "ec2messages:FailMessage",
        "ec2messages:GetEndpoint",
        "ec2messages:GetMessages",
        "ec2messages:SendReply",
        "ecr:BatchCheckLayerAvailability",
        "ecr:BatchGetImage",
        "ecr:DescribeImages",
        "ecr:DescribeRepositories",
        "ecr:GetAuthorizationToken",
        "ecr:GetDownloadUrlForLayer",
        "ecr:ListImages",
        "ecs:*",
        "ecs:PutClusterCapacityProviders",
        "elasticfilesystem:ClientMount",
        "elasticfilesystem:ClientRootAccess",
        "elasticfilesystem:ClientWrite",
        "elasticloadbalancing:*",
        "elasticloadbalancing:CreateTargetGroup",
        "elasticloadbalancing:DeleteTargetGroup",
        "elasticloadbalancing:ModifyListener",
        "guardduty:CreateDetector",
        "guardduty:GetDetector",
        "guardduty:ListDetectors",
        "guardduty:UpdateDetector",
        "iam:AddRoleToInstanceProfile",
        "iam:AttachRolePolicy",
        "iam:CreateInstanceProfile",
        "iam:CreatePolicy",
        "iam:CreateRole",
        "iam:CreateServiceLinkedRole",
        "iam:DeleteInstanceProfile",
        "iam:DeleteRolePolicy",
        "iam:DetachRolePolicy",
        "iam:GetInstanceProfile",
        "iam:GetRole",
        "iam:GetRolePolicy",
        "iam:ListAttachedRolePolicies",
        "iam:ListInstanceProfilesForRole",
        "iam:ListRolePolicies",
        "iam:ListRoles",
        "iam:PutRolePolicy",
        "iam:PutUserPermissionsBoundary",
        "iam:RemoveRoleFromInstanceProfile",
        "logs:*",
        "s3:DeleteObject",
        "s3:GetBucketLocation",
        "s3:GetBucketVersioning",
        "s3:GetObject",
        "s3:ListBucket",
        "s3:PutObject",
        "secretsmanager:CreateSecret",
        "secretsmanager:DeleteSecret",
        "secretsmanager:DescribeSecret",
        "secretsmanager:GetSecretValue",
        "secretsmanager:PutSecretValue",
        "secretsmanager:RotateSecret",
        "secretsmanager:TagResource",
        "secretsmanager:UpdateSecret",
        "secretsmanager:UpdateSecretVersionStage",
        "ssm:*",
        "ssmmessages:CreateControlChannel",
        "ssmmessages:CreateDataChannel",
        "ssmmessages:OpenControlChannel",
        "ssmmessages:OpenDataChannel",
        "sts:AssumeRole",
        "sts:GetCallerIdentity",
        "vpc-lattice-svcs:Invoke",
        "vpc-lattice:*",
        "xray:*"
       ],
       "Effect": "Allow",
       "Resource": "*"
      },
      {
       "Action": "iam:PassRole",
       "Effect": "Allow",
       "Resource": [
        "arn:aws:iam::*:role/*ECS*",
        "arn:aws:iam::*:role/*EcsTask*",
        "arn:aws:iam::*:role/*ecs*",
        "arn:aws:iam::*:role/*retail*"
       ]
      }
     ],
     "Version": "2012-10-17"
    }
   }
  }
}
```

이 리소스는 `ecsWorkshopIdeRoleBoundary`라는 IAM Managed Policy를 생성합니다.

이 정책은 이름상 Workshop IDE Role의 Permissions Boundary로 사용되며, IDE Role이 ECS Workshop 실습에 필요한 AWS 작업을 할 수 있는 최대 권한 범위를 정합니다.

ECS, EC2 네트워크, ALB, CloudWatch Logs, X-Ray, SSM, Secrets Manager, ECR, EFS, VPC Lattice, IAM 일부 작업 등이 허용되어 있습니다.

다만 `iam:PassRole`은 위험한 권한이라서 모든 Role에 대해 허용하지 않고, 이름에 `ECS`, `EcsTask`, `ecs`, `retail` 등이 들어간 Role에 대해서만 허용합니다. `iam:PassRole`은 사용자가 어떤 IAM Role을 직접 쓰는 권한이 아니라, AWS 서비스에게 그 Role을 넘겨줄 수 있는 권한입니다. 
