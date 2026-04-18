---
title: "05. 로드 밸런싱 (ALB, 리스너, 타깃 그룹)"
weight: 5
---
> **작성일:** 2026-04-18 | **수정일:** 2026-04-18
```json
{
  "BasicALB951EEDC7": {
   "Type": "AWS::ElasticLoadBalancingV2::LoadBalancer",
   "Properties": {
    "LoadBalancerAttributes": [
     {
      "Key": "deletion_protection.enabled",
      "Value": "false"
     }
    ],
    "Name": "retail-store-ecs-ui",
    "Scheme": "internet-facing",
    "SecurityGroups": [
     {
      "Fn::GetAtt": [
       "BasicALBSecurityGroupF9D92961",
       "GroupId"
      ]
     }
    ],
    "Subnets": [
     {
      "Ref": "BasicRetailStoreVPCPublicSubnet1SubnetF807A1C8"
     },
     {
      "Ref": "BasicRetailStoreVPCPublicSubnet2Subnet3227177D"
     },
     {
      "Ref": "BasicRetailStoreVPCPublicSubnet3Subnet3BE6B890"
     }
    ],
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
    ],
    "Type": "application"
   },
   "DependsOn": [
    "BasicRetailStoreVPCPublicSubnet1DefaultRouteA07B80E5",
    "BasicRetailStoreVPCPublicSubnet1RouteTableAssociation109AA702",
    "BasicRetailStoreVPCPublicSubnet2DefaultRoute687061C9",
    "BasicRetailStoreVPCPublicSubnet2RouteTableAssociation661C9073",
    "BasicRetailStoreVPCPublicSubnet3DefaultRoute17E6A8A5",
    "BasicRetailStoreVPCPublicSubnet3RouteTableAssociationDA4CEA25"
   ]
  },
  "BasicALBTestListener9BC91E69": {
   "Type": "AWS::ElasticLoadBalancingV2::Listener",
   "Properties": {
    "DefaultActions": [
     {
      "ForwardConfig": {
       "TargetGroupStickinessConfig": {
        "DurationSeconds": 5,
        "Enabled": true
       },
       "TargetGroups": [
        {
         "TargetGroupArn": {
          "Ref": "BasicGreenTargetGroup843DBE73"
         },
         "Weight": 0
        },
        {
         "TargetGroupArn": {
          "Ref": "BasicBlueTargetGroup17B9FA9D"
         },
         "Weight": 100
        }
       ]
      },
      "Type": "forward"
     }
    ],
    "LoadBalancerArn": {
     "Ref": "BasicALB951EEDC7"
    },
    "Port": 8080,
    "Protocol": "HTTP"
   }
  },
  "BasicALBProdListenerE1FBE213": {
   "Type": "AWS::ElasticLoadBalancingV2::Listener",
   "Properties": {
    "DefaultActions": [
     {
      "ForwardConfig": {
       "TargetGroupStickinessConfig": {
        "DurationSeconds": 5,
        "Enabled": true
       },
       "TargetGroups": [
        {
         "TargetGroupArn": {
          "Ref": "BasicGreenTargetGroup843DBE73"
         },
         "Weight": 0
        },
        {
         "TargetGroupArn": {
          "Ref": "BasicBlueTargetGroup17B9FA9D"
         },
         "Weight": 100
        }
       ]
      },
      "Type": "forward"
     }
    ],
    "LoadBalancerArn": {
     "Ref": "BasicALB951EEDC7"
    },
    "Port": 80,
    "Protocol": "HTTP"
   }
  },
  "BasicALBManagedInstanceListener1F658A55": {
   "Type": "AWS::ElasticLoadBalancingV2::Listener",
   "Properties": {
    "DefaultActions": [
     {
      "ForwardConfig": {
       "TargetGroupStickinessConfig": {
        "DurationSeconds": 5,
        "Enabled": true
       },
       "TargetGroups": [
        {
         "TargetGroupArn": {
          "Ref": "ManagedManagedInstanceTargetGroup02D05B48"
         },
         "Weight": 1
        }
       ]
      },
      "Type": "forward"
     }
    ],
    "LoadBalancerArn": {
     "Ref": "BasicALB951EEDC7"
    },
    "Port": 8090,
    "Protocol": "HTTP"
   }
  },
  "BasicBlueTargetGroup17B9FA9D": {
   "Type": "AWS::ElasticLoadBalancingV2::TargetGroup",
   "Properties": {
    "HealthCheckIntervalSeconds": 10,
    "HealthCheckPath": "/actuator/health",
    "HealthCheckTimeoutSeconds": 5,
    "HealthyThresholdCount": 2,
    "Name": "BlueTargetGroup",
    "Port": 8080,
    "Protocol": "HTTP",
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
    ],
    "TargetGroupAttributes": [
     {
      "Key": "deregistration_delay.timeout_seconds",
      "Value": "0"
     },
     {
      "Key": "stickiness.enabled",
      "Value": "true"
     },
     {
      "Key": "stickiness.type",
      "Value": "lb_cookie"
     },
     {
      "Key": "stickiness.lb_cookie.duration_seconds",
      "Value": "300"
     }
    ],
    "TargetType": "ip",
	    "UnhealthyThresholdCount": 3,
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
  "BasicGreenTargetGroup843DBE73": {
   "Type": "AWS::ElasticLoadBalancingV2::TargetGroup",
   "Properties": {
    "HealthCheckIntervalSeconds": 10,
    "HealthCheckPath": "/actuator/health",
    "HealthCheckTimeoutSeconds": 5,
    "HealthyThresholdCount": 2,
    "Name": "GreenTargetGroup",
    "Port": 8080,
    "Protocol": "HTTP",
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
    ],
    "TargetGroupAttributes": [
     {
      "Key": "deregistration_delay.timeout_seconds",
      "Value": "0"
     },
     {
      "Key": "stickiness.enabled",
      "Value": "false"
     }
    ],
    "TargetType": "ip",
    "UnhealthyThresholdCount": 2,
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  }
}
```

---
## 1. `"BasicALB951EEDC7"`

```json
  "BasicALB951EEDC7": {
   "Type": "AWS::ElasticLoadBalancingV2::LoadBalancer",
   "Properties": {
    "LoadBalancerAttributes": [
     {
      "Key": "deletion_protection.enabled",
      "Value": "false"
     }
    ],
    "Name": "retail-store-ecs-ui",
    "Scheme": "internet-facing",
    "SecurityGroups": [
     {
      "Fn::GetAtt": [
       "BasicALBSecurityGroupF9D92961",
       "GroupId"
      ]
     }
    ],
    "Subnets": [
     {
      "Ref": "BasicRetailStoreVPCPublicSubnet1SubnetF807A1C8"
     },
     {
      "Ref": "BasicRetailStoreVPCPublicSubnet2Subnet3227177D"
     },
     {
      "Ref": "BasicRetailStoreVPCPublicSubnet3Subnet3BE6B890"
     }
    ],
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
    ],
    "Type": "application"
   },
   "DependsOn": [
    "BasicRetailStoreVPCPublicSubnet1DefaultRouteA07B80E5",
    "BasicRetailStoreVPCPublicSubnet1RouteTableAssociation109AA702",
    "BasicRetailStoreVPCPublicSubnet2DefaultRoute687061C9",
    "BasicRetailStoreVPCPublicSubnet2RouteTableAssociation661C9073",
    "BasicRetailStoreVPCPublicSubnet3DefaultRoute17E6A8A5",
    "BasicRetailStoreVPCPublicSubnet3RouteTableAssociationDA4CEA25"
   ]
  },
```

---

ALB(Application Load Balancer)를 생성합니다.

---

`"Type": "AWS::ElasticLoadBalancingV2::LoadBalancer"`에서 `"ElasticLoadBalancingV2"`는 AWS의 로드 밸런서 서비스 중 ALB, NLB, GWLB와 같은 v2 계열을 의미합니다.

---

```json
    "LoadBalancerAttributes": [
     {
      "Key": "deletion_protection.enabled",
      "Value": "false"
     }
```

`"LoadBalancerAttributes"`는 로드 밸런서의 속성을 설정하는 항목입니다. `"Key": "deletion_protection.enabled"`는 로드 밸런서의 삭제 보호 기능을 의미하며, `"Value": "false"`이므로 이 로드 밸런서는 삭제 보호가 비활성화된 상태로 생성됩니다. 즉 필요할 경우 별도의 보호 해제 없이 삭제할 수 있습니다.

실습 환경이기 때문에 이 옵션은 `"false"`로 설정되었지만, 실무에서는 일반적으로 `"true"`로 설정됩니다. ALB는 애플리케이션으로 들어오는 요청의 진입점 역할을 하므로, 실수로 삭제되면 사용자 요청을 정상적으로 처리할 수 없게 됩니다. 따라서 운영 환경에서는 삭제 보호를 `"true"`로 설정하는 것이 일반적입니다.

---

`"Scheme": "internet-facing"`은 이 로드 밸런서를 인터넷에 공개되는 형태로 만들겠다는 의미입니다. 즉 이 옵션이 설정되면 로드 밸런서 노드는 Public IP를 가지며, 외부 인터넷에서 접근이 가능하게 됩니다. 이 옵션은 ALB와 NLB에만 적용되며, GLB(Gateway Load Balancer)에는 사용할 수 없습니다.

---

```json
    "SecurityGroups": [
     {
      "Fn::GetAtt": [
       "BasicALBSecurityGroupF9D92961",
       "GroupId"
      ]
     }
    ],
```

전에 만든 보안 그룹을 이 로드 밸런서에 연결합니다. `"Fn::GetAtt"`는 앞서 생성한 보안 그룹 리소스에서 `GroupId` 값을 가져오기 위해 사용됩니다. 즉 보안 그룹 자체를 넘기는 것이 아니라, 그 보안 그룹의 ID를 참조해서 연결하는 것입니다.

참고로 이 보안 그룹은 `"VpcId"`가 지정되어 있으므로, 여기서는 `"Ref"`를 사용해도 동일하게 `"GroupId"`를 가져올 수 있습니다. 여기서 `"VpcId"`는 반환되는 값이 아니라, `"Ref"`가 무엇을 반환할지 결정하는 조건입니다. `"AWS::EC2::SecurityGroup"` 리소스는 `"VpcId"`가 지정된 경우 `"Ref"`가 `"GroupId"`를 반환합니다. 또한 `"GroupId"`는 CloudFormation 템플릿에 직접 작성되는 값이 아니라, AWS가 보안 그룹을 생성할 때 자동으로 부여하는 식별자입니다.

---

```json
    "Subnets": [
     {
      "Ref": "BasicRetailStoreVPCPublicSubnet1SubnetF807A1C8"
     },
     {
      "Ref": "BasicRetailStoreVPCPublicSubnet2Subnet3227177D"
     },
     {
      "Ref": "BasicRetailStoreVPCPublicSubnet3Subnet3BE6B890"
     }
    ],
```

로드 밸런서가 3개의 퍼블릭 서브넷을 참조합니다.

로드 밸런서는 여러 AZ에 걸쳐 배치됩니다. 정확히는 활성화된 각 AZ에 로드 밸런서 노드가 생성되며, 각 노드는 해당 서브넷에서 자신의 ENI를 통해 네트워크 통신을 수행합니다. 사용자는 DNS를 통해 이 노드들 중 하나로 요청을 보내고, 요청을 받은 노드가 이를 수신해 처리합니다. 이후 로드 밸런서는 리스너(Listener)와 규칙(Rule)에 따라 적절한 타깃을 선택해 요청을 전달합니다. 또한 기본적으로 cross-zone load balancing이 적용되므로, 요청은 같은 AZ 또는 다른 AZ의 타깃 그룹에 등록된 타깃으로 전달될 수 있습니다.

---

`"Type": "application"`은 이 로드밸런서가 ALB(Application Load Balancer)라는 것을 알려줍니다.

---

## 2. `"BasicALBTestListener9BC91E69"`

```json
  "BasicALBTestListener9BC91E69": {
   "Type": "AWS::ElasticLoadBalancingV2::Listener",
   "Properties": {
    "DefaultActions": [
     {
      "ForwardConfig": {
       "TargetGroupStickinessConfig": {
        "DurationSeconds": 5,
        "Enabled": true
       },
       "TargetGroups": [
        {
         "TargetGroupArn": {
          "Ref": "BasicGreenTargetGroup843DBE73"
         },
         "Weight": 0
        },
        {
         "TargetGroupArn": {
          "Ref": "BasicBlueTargetGroup17B9FA9D"
         },
         "Weight": 100
        }
       ]
      },
      "Type": "forward"
     }
    ],
    "LoadBalancerArn": {
     "Ref": "BasicALB951EEDC7"
    },
    "Port": 8080,
    "Protocol": "HTTP"
   }
  },
```

로드 밸런서의 테스트 리스너를 생성합니다. 리스너는 지정된 프로토콜과 포트로 들어오는 클라이언트 요청을 수신합니다. 그 후 등록된 규칙(Rule)을 순서대로 평가하여 적합한 타깃 그룹으로 트래픽을 전달합니다.

리스너(Listener)와 규칙(Rule) 예시입니다.

```
ALB
└── 리스너 (HTTP:80)
    ├── 규칙 1: /api/* → Target Group A
    ├── 규칙 2: /images/* → Target Group B
    └── 기본 규칙 (Default): → Target Group C
```

---

`"DefaultActions"`는 이 리스너에 들어온 요청을 기본적으로 어떻게 처리할지를 정의하는 설정입니다.

---

```json
      "ForwardConfig": {
       "TargetGroupStickinessConfig": {
        "DurationSeconds": 5,
        "Enabled": true
       },
       "TargetGroups": [
        {
         "TargetGroupArn": {
          "Ref": "BasicGreenTargetGroup843DBE73"
         },
         "Weight": 0
        },
        {
         "TargetGroupArn": {
          "Ref": "BasicBlueTargetGroup17B9FA9D"
         },
         "Weight": 100
        }
       ]
      },
```

`"ForwardConfig"`는 `forward` 동작을 어떻게 할지 정하는 설정입니다. `forward` 동작은 리스너가 받은 요청을 타깃 그룹으로 전달하는 동작입니다. 

`"TargetGroupStickinessConfig"`는 이 리스너로 들어온 같은 클라이언트의 요청, 즉 동일한 프로토콜과 동일한 포트로 들어온 요청을 일정 시간 동안 같은 타깃 그룹으로 유지하도록 하는 설정입니다. `"DurationSeconds"`는 그 유지 시간을 의미하고, `"Enabled": true`는 이 설정이 활성화되어 있음을 뜻합니다.

---

```json
       "TargetGroups": [
        {
         "TargetGroupArn": {
          "Ref": "BasicGreenTargetGroup843DBE73"
         },
         "Weight": 0
        },
        {
         "TargetGroupArn": {
          "Ref": "BasicBlueTargetGroup17B9FA9D"
         },
         "Weight": 100
        }
       ]
```

`"TargetGroups"`는 리스너가 요청을 어느 타깃 그룹으로 전달할지 정하는 설정입니다.

`"TargetGroupArn"`은 이 리스너가 요청을 전달할 타깃 그룹의 ARN입니다. ARN은 Amazon Resource Name의 약자로, AWS에서 리소스를 고유하게 식별하는 전체 이름입니다. 예를 들면 `"arn:aws:elasticloadbalancing:ap-northeast-2:123456789012:targetgroup/BlueTargetGroup/abc123"` 같은 값입니다.

여기서는 `"BasicGreenTargetGroup843DBE73"`와 `"BasicBlueTargetGroup17B9FA9D"`가 두 타깃 그룹을 참조합니다.

`"Weight"`는 해당 타깃 그룹으로 요청을 얼마나 보낼지를 나타내는 값입니다. 여기서는 `"BasicGreenTargetGroup843DBE73"`의 `"Weight"`가 `0`이고 `"BasicBlueTargetGroup17B9FA9D"`의 `"Weight"`가 `100`이기 때문에, 모든 트래픽이 `"BasicBlueTargetGroup17B9FA9D"`로 전달됩니다.

이 타깃 그룹은 추후 블루/그린 배포를 위한 것으로 보입니다. 블루/그린 배포는 기존 버전과 새 버전을 각각 다른 타깃 그룹에 두고, 새 버전이 정상적으로 동작하는지 확인한 뒤 트래픽을 전환하는 방식입니다. 보통 기존 운영 환경은 Blue 타깃 그룹에 두고, 업데이트된 컨테이너는 Green 타깃 그룹에 배치합니다. 이후 Green 쪽이 정상 작동하는 것이 확인되면 요청을 Green 쪽으로 전환하고, 문제가 생기면 다시 Blue 쪽으로 요청을 돌릴 수 있습니다. 롤링 업데이트 방식과는 다르게, 트래픽을 새 컨테이너 쪽으로 서서히 넘기는 것이 아니라 한 번에 전환하는 방식입니다.

---

`"Type": "forward"`는 이 리스너가 받은 요청을 타깃 그룹으로 전달하도록 설정되어 있다는 뜻입니다. 리스너는 일반적으로 받은 요청을 타깃 그룹으로 전달하지만, `"redirect"`를 사용하면 다른 URL이나 포트로 리다이렉트할 수 있고, `"fixed-response"`를 사용하면 정해진 응답을 바로 반환할 수도 있습니다. 

예를 들면, `80`번 포트로 들어온 `HTTP` 트래픽을 `HTTPS`인 `443`번 포트로 리다이렉트해서 추후 일어날 통신을 `HTTPS`로 하게 만들거나, `naver.com`으로 온 요청을 `www.naver.com`으로 보내는 등의 리다이렉트가 필요할 수 있습니다.  반면 `"fixed-response"`는 `404 Not Found` 같은 응답을 타깃 그룹으로 전달하지 않고 ALB가 직접 반환할 때 사용합니다.

---

```json
    "LoadBalancerArn": {
     "Ref": "BasicALB951EEDC7"
    },
```

로드 밸런서에 이 리스너를 연결합니다.

---

```json
    "Port": 8080,
    "Protocol": "HTTP"
   }
```

이 리스너는 ALB의 `8080`번 포트로 들어오는 `HTTP` 요청을 받으며, 해당 요청에 대해 `"DefaultActions"`에 정의된 동작을 수행합니다. 

---

## 3. `"BasicALBProdListenerE1FBE213"`

```json
  "BasicALBProdListenerE1FBE213": {
   "Type": "AWS::ElasticLoadBalancingV2::Listener",
   "Properties": {
    "DefaultActions": [
     {
      "ForwardConfig": {
       "TargetGroupStickinessConfig": {
        "DurationSeconds": 5,
        "Enabled": true
       },
       "TargetGroups": [
        {
         "TargetGroupArn": {
          "Ref": "BasicGreenTargetGroup843DBE73"
         },
         "Weight": 0
        },
        {
         "TargetGroupArn": {
          "Ref": "BasicBlueTargetGroup17B9FA9D"
         },
         "Weight": 100
        }
       ]
      },
      "Type": "forward"
     }
    ],
    "LoadBalancerArn": {
     "Ref": "BasicALB951EEDC7"
    },
    "Port": 80,
    "Protocol": "HTTP"
   }
  },
```

---

이 리스너는 앞서 살펴본 리스너와 거의 동일한 설정을 가지고 있습니다.

---

```json
    "Port": 80,
    "Protocol": "HTTP"
   }
```

이 리스너는 ALB의 `80`번 포트로 들어오는 `HTTP` 요청을 받으며, 해당 요청을 `"DefaultActions"`에 정의된 설정에 따라 타깃 그룹으로 전달합니다.

---

## 4. `"BasicALBManagedInstanceListener1F658A55"`

```json
  "BasicALBManagedInstanceListener1F658A55": {
   "Type": "AWS::ElasticLoadBalancingV2::Listener",
   "Properties": {
    "DefaultActions": [
     {
      "ForwardConfig": {
       "TargetGroupStickinessConfig": {
        "DurationSeconds": 5,
        "Enabled": true
       },
       "TargetGroups": [
        {
         "TargetGroupArn": {
          "Ref": "ManagedManagedInstanceTargetGroup02D05B48"
         },
         "Weight": 1
        }
       ]
      },
      "Type": "forward"
     }
    ],
    "LoadBalancerArn": {
     "Ref": "BasicALB951EEDC7"
    },
    "Port": 8090,
    "Protocol": "HTTP"
   }
  },
```

---

이 리스너 역시 앞서 살펴본 리스너들과 거의 동일한 설정을 가지고 있습니다.

---

```json
       "TargetGroups": [
        {
         "TargetGroupArn": {
          "Ref": "ManagedManagedInstanceTargetGroup02D05B48"
         },
         "Weight": 1
        }
       ]
```

이 리스너에는 하나의 타깃 그룹만 연결되어 있으며, `"ManagedManagedInstanceTargetGroup02D05B48"`을 참조하고 있습니다.

---

```json
    "Port": 8090,
    "Protocol": "HTTP"
   }
```

이 리스너는 ALB의 `8090`번 포트로 들어오는 `HTTP` 요청을 받아, `"DefaultActions"`에 정의된 설정에 따라 해당 타깃 그룹으로 전달합니다.

---

## 5. `"BasicBlueTargetGroup17B9FA9D"`

```json
  "BasicBlueTargetGroup17B9FA9D": {
   "Type": "AWS::ElasticLoadBalancingV2::TargetGroup",
   "Properties": {
    "HealthCheckIntervalSeconds": 10,
    "HealthCheckPath": "/actuator/health",
    "HealthCheckTimeoutSeconds": 5,
    "HealthyThresholdCount": 2,
    "Name": "BlueTargetGroup",
    "Port": 8080,
    "Protocol": "HTTP",
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
    ],
    "TargetGroupAttributes": [
     {
      "Key": "deregistration_delay.timeout_seconds",
      "Value": "0"
     },
     {
      "Key": "stickiness.enabled",
      "Value": "true"
     },
     {
      "Key": "stickiness.type",
      "Value": "lb_cookie"
     },
     {
      "Key": "stickiness.lb_cookie.duration_seconds",
      "Value": "300"
     }
    ],
    "TargetType": "ip",
    "UnhealthyThresholdCount": 3,
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
```

로드 밸런서의 타깃 그룹을 생성합니다.

---

`"HealthCheckIntervalSeconds": 10`은 로드 밸런서가 타깃 그룹에 등록된 각 타깃에 대해 `10`초마다 헬스 체크(Health Check)를 수행한다는 뜻입니다.

---

헬스 체크란 로드 밸런서가 트래픽을 전달하는 타깃 그룹의 타깃들이 정상적으로 작동하는지 확인하는 작업입니다. 이를 위해 로드 밸런서는 각 타깃에 헬스 체크 요청을 주기적으로 보내고, 그 응답을 바탕으로 정상 여부를 판단합니다.

---

`"HealthCheckPath": "/actuator/health"`는 로드 밸런서가 헬스 체크 요청을 보낼 URL 경로입니다. 이 타깃 그룹은 타깃에 `8080`번 포트로 `HTTP` 헬스 체크 요청을 보내도록 설정되어 있으므로, 실제 헬스 체크 요청은 `http://타깃IP:8080/actuator/health` 형태로 전송된다고 볼 수 있습니다.

---

`"HealthCheckTimeoutSeconds": 5`는 로드 밸런서가 헬스 체크 요청을 보낸 뒤 최대 5초까지 기다린다는 뜻입니다. 만약 5초 이내에 로드 밸런서가 응답을 받지 못한다면, 이 헬스 체크 요청은 실패한 것으로 간주합니다. 

---

`"HealthyThresholdCount": 2`는 헬스 체크에 2번 연속 성공해야 해당 타깃을 정상 상태(Healthy)로 판단하겠다는 뜻입니다. 한 번 헬스 체크에 성공했다고 해서 해당 타깃을 바로 정상 타깃으로 판단하기는 이르기 때문에, 연속적으로 헬스 체크에 성공해야 해당 타깃을 정상 타깃으로 판단합니다.

---

```json
    "Name": "BlueTargetGroup",
    "Port": 8080,
    "Protocol": "HTTP",
```

이 타깃 그룹(`"BlueTargetGroup"`)에 등록된 타깃들은 `8080`번 포트에서 `HTTP` 요청을 받습니다.

---

```json
    "TargetGroupAttributes": [
     {
      "Key": "deregistration_delay.timeout_seconds",
      "Value": "0"
     },
     {
      "Key": "stickiness.enabled",
      "Value": "true"
     },
     {
      "Key": "stickiness.type",
      "Value": "lb_cookie"
     },
     {
      "Key": "stickiness.lb_cookie.duration_seconds",
      "Value": "300"
     }
    ],
```

`"TargetGroupAttributes"`는 타깃 그룹의 세부 동작 속성을 정의합니다.

`"deregistration_delay.timeout_seconds"`는 타깃이 타깃 그룹에서 제외될 때, 해당 타깃에 이미 들어와 있던 요청을 끝낼 시간을 설정하는 옵션입니다. 타깃이 자신에게 온 요청을 처리하는 도중에 타깃 그룹에서 제외되면, 기존 요청이 아직 처리 중일 수 있습니다. 이 속성은 그때 기존 요청을 얼마나 더 처리하게 둘지를 설정합니다. 여기서는 `"Value": "0"`이므로, 대기 시간 없이 타깃을 즉시 제외합니다.

`"stickiness.enabled"`는 스티키(Sticky) 세션을 사용할지 여부를 정하는 옵션입니다. 스티키 세션은 한 사용자가 처음 연결된 타깃으로 일정 시간 동안 계속 요청을 보내도록 하는 기능입니다. 로드 밸런서는 원래 요청이 들어올 때마다 여러 타깃에 트래픽을 고르게 분산하려고 하지만, 이 설정을 사용하면 한 사용자의 여러 요청을 같은 타깃으로 연속해서 전달할 수 있습니다. 여기서는 값이 `"true"`이므로 스티키 세션이 활성화됩니다.

`"stickiness.type"`은 스티키 세션의 방식을 결정하는 옵션입니다. `"lb_cookie"`는 Load Balancer Cookie의 줄임말로, 로드 밸런서가 발급한 쿠키를 이용해 스티키 세션을 유지하겠다는 뜻입니다. 로드 밸런서 쿠키는 로드 밸런서가 사용자의 브라우저에 발급하는 쿠키로, 사용자와 특정 타깃의 연결 관계를 일정 시간 동안 기억하는 역할을 합니다. 이를 통해 사용자의 브라우저는 일정 시간 동안 같은 타깃으로 계속 요청을 보낼 수 있습니다.

`"stickiness.lb_cookie.duration_seconds"`는 로드 밸런서 쿠키 기반 스티키 세션을 몇 초 동안 유지할지 정하는 옵션입니다. 값이 300이기 때문에 300초 뒤에 스티키 세션이 만료됩니다.

---

`"TargetType": "ip"`는 로드 밸런서가 인스턴스의 ID가 아니라 타깃으로 등록된 리소스의 IP 주소를 기준으로 타깃을 식별하고 요청을 전달한다는 뜻입니다. `"TargetType"`의 종류는, `"ip"`, `"instance"`, `"lambda"`, `"alb"` 등이 있습니다.

ECS에서 `awsvpc` 네트워크 모드를 사용하는 경우에는, 로드 밸런서가 바라봐야 하는 대상이 EC2 인스턴스가 아니라 각 Task의 ENI/IP가 되기 때문에 `"TargetType": "ip"`를 사용해야 합니다. `awsvpc`는 ECS Task마다 VPC 네트워크를 직접 붙여주는 네트워크 모드입니다. 대부분 이 모드가 사용됩니다.

`"UnhealthyThresholdCount": 3`는 헬스 체크에 연속으로 3번 실패해야 해당 타깃을 비정상 상태(Unhealthy)로 간주하겠다는 의미입니다.

---
## 6. `"BasicGreenTargetGroup843DBE73"`

```json
  "BasicGreenTargetGroup843DBE73": {
   "Type": "AWS::ElasticLoadBalancingV2::TargetGroup",
   "Properties": {
    "HealthCheckIntervalSeconds": 10,
    "HealthCheckPath": "/actuator/health",
    "HealthCheckTimeoutSeconds": 5,
    "HealthyThresholdCount": 2,
    "Name": "GreenTargetGroup",
    "Port": 8080,
    "Protocol": "HTTP",
    "Tags": [
     {
      "Key": "CreationDate",
      "Value": "2026-03-31"
     },
     {
      "Key": "Version",
      "Value": "1.0.4"
     },
     {
      "Key": "WorkshopName",
      "Value": "ECSImmersionDay"
     }
    ],
    "TargetGroupAttributes": [
     {
      "Key": "deregistration_delay.timeout_seconds",
      "Value": "0"
     },
     {
      "Key": "stickiness.enabled",
      "Value": "false"
     }
    ],
    "TargetType": "ip",
    "UnhealthyThresholdCount": 2,
    "VpcId": {
     "Ref": "BasicRetailStoreVPC0FA21CD5"
    }
   }
  },
```

이전 타깃 그룹과 비슷한 설정을 가지고 있습니다.

---

```json
    "TargetGroupAttributes": [
     {
      "Key": "deregistration_delay.timeout_seconds",
      "Value": "0"
     },
     {
      "Key": "stickiness.enabled",
      "Value": "false"
     }
    ],
```

`"stickiness.enabled"`가 `"false"`로 설정되어 있으므로 스티키 세션이 활성화되지 않습니다. `GreenTargetGroup`은 새 애플리케이션 버전으로 테스트와 검증이 필요하기 때문에, 이 옵션을 통해 트래픽이 여러 타깃에 고르게 분산되도록 했다고 추측할 수 있습니다.
