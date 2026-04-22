---
title: "07. 로깅 (CloudWatch)"
weight: 7
---
> **작성일:** 2026-04-19 | **수정일:** 2026-04-19
```json
{
  "BasicWorkshopLogGroup063FF597": {
   "Type": "AWS::Logs::LogGroup",
   "Properties": {
    "LogGroupName": "retail-store-ecs-tasks",
    "RetentionInDays": 1,
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Version",
      "Value": "1.0.2"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ]
   },
   "UpdateReplacePolicy": "Delete",
   "DeletionPolicy": "Delete"
  }
}
```

---
## 1. `"BasicWorkshopLogGroup063FF597"`

```json
  "BasicWorkshopLogGroup063FF597": {
   "Type": "AWS::Logs::LogGroup",
   "Properties": {
    "LogGroupName": "retail-store-ecs-tasks",
    "RetentionInDays": 1,
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-02-21"
     },
     {
      "Key": "Version",
      "Value": "1.0.2"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ]
   },
   "UpdateReplacePolicy": "Delete",
   "DeletionPolicy": "Delete"
  }
```

CloudWatch Logs의 로그 그룹을 생성합니다.

---

`"RetentionInDays": 1`은 이 로그 그룹에 저장된 로그를 1일 동안만 보관하겠다는 뜻입니다.

---

`"UpdateReplacePolicy": "Delete"`는 이 리소스를 관리하는 CloudFormation 스택이 업데이트되는 과정에서 해당 리소스가 교체되면, 기존 로그 그룹을 보관하지 않고 삭제한다는 의미입니다.

---

`"DeletionPolicy": "Delete"`는 CloudFormation 스택을 삭제할 때 이 로그 그룹도 같이 삭제하겠다는 뜻입니다.
