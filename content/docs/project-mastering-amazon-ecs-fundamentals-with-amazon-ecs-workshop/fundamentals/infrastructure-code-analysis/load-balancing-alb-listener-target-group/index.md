---
title: "05. 로드 밸런싱 (ALB, 리스너, 타깃 그룹)"
weight: 5
---
> **작성일:** 2026-05-03 | **수정일:** 2026-05-03
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

로드 밸런서를 생성하는 리소스입니다.

---

`"Type": "AWS::ElasticLoadBalancingV2::LoadBalancer"`에서 `ElasticLoadBalancingV2`는 ALB, NLB 같은 최신 세대 로드 밸런서들과 리스너등등을 포함하고 있는 CloudFormation 네임스페이스입니다.

---

`"LoadBalancerAttributes"`는 로드 밸런서의 여러 속성과 속성값을 `key-value` 페어로 정의합니다. 속성이 계속 추가될 수 있기 때문에 이런 구조를 가진 것으로 보입니다.

`"Key": "deletion_protection.enabled"` 속성은 이 로드 밸런서의 삭제 보호 여부를 의미합니다. 속성값이 `"Value": "false"`이기 때문에 이 로드 밸런서는 삭제로부터 보호되지 않습니다. 로드 밸런서가 실수로 삭제되면, ALB의 경우 애플리케이션이 인터넷으로부터 요청을 받을 수 없습니다. 때문에 이 속성을 설정해 의도하지 않은 삭제로부터 보호하기 위한 것으로 보입니다.

---

`"Scheme"`은 로드 밸런서를 인터넷에 공개할지, VPC 내부에서만 사용할지 정합니다. 값이 `"internet-facing"`이기 때문에 이 로드 밸런서는 인터넷에 공개되어 외부로부터 접근이 가능합니다. VPC 내부에서만 사용하는 경우는 값이 `"internal"`이 되야합니다.

---

`"SecurityGroups"`는 로드 밸런서에 적용될 보안 그룹을 명시합니다. 이 경우 보안 그룹의 Group ID가 필요합니다. 

---

`"Subnets"`는 이 로드 밸런서가 배치될 서브넷들을 명시합니다. 여기서는 3개의 퍼블릭 서브넷에 모두 배치됩니다.

로드 밸런서는 여러 노드를 가질 수 있고, 이 노드들은 지정된 각 서브넷에 배치됩니다. 각 노드에 있는 ENI를 통해 클라이언트로부터 요청을 받을 수 있습니다. 이 ENI에 연결된 퍼블릭 IP 주소는 모두 다릅니다. 따라서 클라이언트가 이 로드 밸런서의 DNS 호스트 이름을 조회하면, AWS가 관리하는 DNS가 여러 노드의 ENI에 연결된 퍼블릭 IP 주소들을 클라이언트에 반환합니다. 클라이언트는 이 IP 주소 중 하나로 로드 밸런서에 요청을 보내게 됩니다.

참고로 로드 밸런서 노드의 ENI에 매핑된 퍼블릭 IP 주소는 사용자가 직접 할당하거나 관리하는 것이 아니라, AWS가 자동으로 할당하고 관리합니다.

---

`"Type": "application"`은 이 로드 밸런서가 Application Load Balancer(ALB) 라는 것을 의미합니다.

---

`"DependsOn"`에 지정된 리소스들을 보면 앞서 말한 3개의 퍼블릭 서브넷에 존재하는 기본 라우트와 라우트 테이블 어소시에이션들입니다. 이 리소스들이 먼저 생성되어야 이 로드 밸런서가 생성될 수 있음을 의미합니다.

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

로드 밸런서의 리스너(Listener)를 생성하는 리소스입니다. 리스너는 특정 포트와 프로토콜로 들어오는 요청을 받습니다. 이 리스너는 블루/그린 배포를 위한 테스트 리스너입니다. 

---

`"DefaultActions"`는 이 리스너가 받은 트래픽에 대한 기본 동작을 설정하는 필드입니다.

---

`"ForwardConfig"`는 리스너가 받은 요청을 어떤 타깃 그룹들로, 어떻게 전달할지에 대한 설정입니다.

`"TargetGroupStickinessConfig"`는 동일한 클라이언트로부터 온 요청을 같은 타깃 그룹으로 얼마 동안 계속 전달할지를 설정합니다. `"Enabled": true`이기 때문에 이 설정이 활성화되어 있고, `"DurationSeconds": 5`이기 때문에 5초 동안 동일한 클라이언트의 요청이 같은 타깃 그룹으로 전달됩니다.

이 리스너는 블루/그린 배포를 위한 테스트 리스너이기 때문에, 동일한 클라이언트의 요청, 즉 블루/그린 배포 테스터가 보낸 테스트용 트래픽이 같은 타깃 그룹(블루 또는 그린)으로 계속 전달될 필요가 있습니다. 

---

`"TargetGroups"`은 타깃 그룹들을 지정합니다. 총 두 개의 타깃 그룹이 지정되어있습니다.


`"TargetGroupArn"`은 타깃 그룹의 ARN을 지정하는 필드입니다. `"Ref"`로 타깃 그룹의 Logical ID를 참조하여 해당 타깃 그룹의 ARN을 반환받아 지정합니다. 참고로 ARN은 Amazon Resource Name의 줄임말로, AWS에서 리소스를 식별하는 고유한 식별자입니다. 예시로 `arn:aws:ec2:us-east-1:123456789012:vpc/vpc-0e9801d129EXAMPLE`과 같은 구조를 가지고 있습니다.

`"Weight"`은 이 타깃 그룹으로 보낼 트래픽의 비율입니다. 

첫 번째 타깃 그룹은 기본 그린 타깃 그룹(`"BasicGreenTargetGroup843DBE73"`)이고, `"Weight"`의 값은 `0`으로 설정되어 있습니다. 따라서 이 타깃 그룹에는 로드 밸런서로부터 트래픽이 전달되지 않습니다.

두 번째 타깃 그룹은 기본 블루 타깃 그룹(`"BasicBlueTargetGroup17B9FA9D"`)이고, `"Weight"`의 값은 `100`으로 설정되어 있습니다. 따라서 이 타깃 그룹에는 이 리스너가 받은 모든 트래픽이 전달됩니다.

블루/그린 배포에서 테스트 리스너를 통해 받은 테스트 트래픽은 그린 타깃 그룹으로 전달됩니다. 만약 이 그린 타깃 그룹에 있는 태스크가 정상적으로 테스트 트래픽을 처리한다면, 프로덕션 리스너를 통해 받은 운영 트래픽도 그린 타깃 그룹으로 전달됩니다. 만약 태스크가 정상적으로 작동하지 않는다면, 프로덕션 리스너를 통해 받은 운영 트래픽은 기존 블루 타깃 그룹으로 계속 전달됩니다. 

여기서는 일단 리스너에서 받은 트래픽이 그린 타깃 그룹으로 전혀 전달되지 않고, 블루 타깃 그룹으로만 전달되게 설정되어 있습니다. 이는 초기 설정으로, 나중에 실습을 통해 이 값이 바뀔 수 있습니다.

---

`"Type": "forward"`는 이 리스너의 동작 방식이 트래픽은 타깃 그룹으로 전달함을 의미합니다.

필드 값 중에는 `"redirect"`, `"fixed-response"`, `"authenticate-oidc"`도 존재합니다.  
`"redirect"`는 클라이언트를 다른 URL로 리다이렉트하는 것이고, `"fixed-response"`는 정해진 응답을 반환합니다. `"authenticate-oidc"`는 OIDC 방식으로 클라이언트를 인증하고, 인증이 성공한 요청만 이후 타깃 그룹으로 전달되도록 합니다.

---

`"LoadBalancerArn"`은 이 리스너가 연결될 로드 밸런서를 지정합니다.

---

`"Port": 8080`과 `"Protocol": "HTTP"`는 이 리스너가 받는 요청이 `HTTP` 프로토콜을 사용하고, 향하는 포트가 `8080` 포트라는 것을 의미합니다.

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

로드 밸런서의 리스너(Listener)를 생성하는 리소스입니다.

이 리스너는 프로덕션 환경에서 사용되는 리스너입니다. 일반적으로 블루/그린 배포를 하는 애플리케이션의 프로덕션용 리스너에도 그린 타깃 그룹과 블루 타깃 그룹이 지정되어 있습니다. 이는 테스트 후에 운영 트래픽을 블루 타깃 그룹에서 그린 타깃 그룹으로 전환할 수 있어야 하기 때문입니다. 이 타깃 그룹들은 테스트 리스너에서 사용된 타깃 그룹들과 동일합니다.

이 리스너의 구조는 앞서 살펴본 블루/그린 배포를 위한 테스트 리스너(`"BasicALBTestListener9BC91E69"`)와 거의 동일합니다. 다만 이 리스너가 받는 요청이 향하는 포트는 `80` 포트입니다.

이는 이 리스너가 실제 프로덕션 환경에서 사용되는 리스너이기 때문에, 일반 `HTTP` 요청이 기본적으로 사용하는 `80` 포트로 들어오는 요청을 받는 것입니다.

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

로드 밸런서의 리스너(Listener)를 생성하는 리소스입니다.

이 리스너는 ECS Managed Instances에서 실행되는 웹 애플리케이션에 요청을 전달하기 위한 리스너입니다. ECS Managed Instances는 EC2 인스턴스 기반으로 실행하지만, 인스턴스 프로비저닝, 스케일링, 패치, 유지보수 같은 관리 부담을 AWS가 많이 대신해주는 방식입니다.

이 리스너의 구조는 앞서 살펴본 블루/그린 배포를 위한 테스트 리스너(`"BasicALBTestListener9BC91E69"`)와 동일한 부분이 많지만, 타깃 그룹이 두 개가 아닌 하나라는 점과 트래픽이 도달하는 포트가 `8090` 포트라는 점이 다릅니다.

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

타깃 그룹을 생성하는 리소스입니다. 앞서 살펴본 테스트 리스너와 프로덕션용 리스너에서 지정한 기본 블루 타깃 그룹(`"BasicBlueTargetGroup17B9FA9D"`)을 생성합니다.

---

`"HealthCheckIntervalSeconds"`는 로드 밸런서가 이 타깃 그룹에 헬스 체크(Health Check) 요청을 보내는 주기를 의미합니다. 값이 `10`이기 때문에 로드 밸런서가 헬스 체크 요청을 10초마다 보냅니다. 로드 밸런서는 타깃 그룹에 연결된 태스크들이 정상적으로 응답하는지 확인하기 위해 헬스 체크 요청을 주기적으로 자동 전송합니다.

---

`"HealthCheckPath": "/actuator/health"`는 로드 밸런서가 헬스 체크 요청을 보내는 엔드포인트 경로입니다. 타깃 그룹의 태스크 ENI에 할당된 프라이빗 IP 주소까지 합치면 `http://10.0.x.x:8080/actuator/health`와 같은 형식이 됩니다.

참고로 `"/actuator/health"` 헬스 체크 엔드포인트는 스프링 부트로 만들어진 애플리케이션이 가지고 있는 엔드포인트입니다. 따라서 이 헬스 체크 엔드포인트가 보인다면, 해당 애플리케이션은 스프링 부트로 만들어진 애플리케이션일 확률이 높습니다. 단, 스프링 부트에 Spring Boot Actuator가 활성화되어 있어야 이 엔드포인트가 활성화됩니다. Spring Boot Actuator가 비활성화되어 있으면 이 엔드포인트가 없거나 접근해도 응답하지 않을 수 있습니다.

---

`"HealthCheckTimeoutSeconds"`는 지정된 시간 동안 헬스 체크 요청에 대한 응답을 받지 못하면, 해당 헬스 체크를 실패한 것으로 간주하겠다는 의미입니다. 여기서는 값이 `5`이기 때문에, 5초 안에 헬스 체크 요청에 대한 응답을 받지 못하면 해당 헬스 체크는 실패한 것으로 간주합니다.

---

`"HealthyThresholdCount"`는 헬스 체크가 몇 번 연속으로 성공해야 해당 타깃을 정상 상태로 판단할지를 설정하는 필드입니다. 여기서는 값이 `2`이기 때문에, 헬스 체크에 2번 연속으로 성공해야 해당 타깃을 정상 상태로 판단합니다.

---

`"Port"`는 타깃 그룹에 등록된 타깃이 요청을 받을 포트를 의미합니다. 여기는 `8080`포트입니다. 

주의할 점으로, 리스너 리소스에 있는 포트 번호는, 클라이언트가 ALB로 보낸 요청의 목적지 포트를 말하는 것이고, 타깃 그룹 리소스에 있는 포트 번호는 타깃 그룹에 등록된 타깃이 요청을 받을 포트를 의미합니다. 

로드 밸런서는 리스너가 받은 트래픽의 일부 정보를 추가하거나 수정한 후, 새 트래픽을 만들어 타깃 그룹에 등록된 타깃으로 전달합니다. 이때 새 트래픽의 목적지 포트 번호는 타깃 그룹에 설정된 포트 번호를 따릅니다.

예를 들면, 클라이언트가 ALB에 `80`번 포트로 요청을 하면, ALB는 그 요청 트래픽의 일부 정보를 추가하거나 수정한 후, `8080`번 포트를 목적지로 하는 새 트래픽을 생성해 타깃 그룹의 타깃으로 전달합니다.

```
클라이언트 → ALB:80 (예: http://retail-store-ecs-ui-123456789.ap-northeast-3.elb.amazonaws.com:80/home)
ALB → UI Task:8080 (예: http://10.0.3.25:8080/home)
```

---

`"Protocol"`은 타깃 그룹에 등록된 타깃이 받을 트래픽의 프로토콜을 의미합니다. 여기서는 `HTTP`입니다.

---

`"TargetGroupAttributes"`는 타깃 그룹의 여러 속성과 속성값을 `key-value` 페어로 정의합니다. 속성이 계속 추가될 수 있기 때문에 이런 구조를 가진 것으로 보입니다.

`"Key": "deregistration_delay.timeout_seconds"` 속성은 이 타깃 그룹에 등록된 타깃이 업데이트 등의 이유로 속한 타깃 그룹에서 제외되었을 때, 해당 타깃의 기존 연결이나 처리 중이던 요청을 마무리할 시간을 의미합니다. 속성값이 `"Value": "0"`이기 때문에, 이 타깃 그룹의 타깃은 속한 타깃 그룹에서 제외되자마자 기존 연결이나 처리 중이던 요청을 마무리할 시간이 주어지지 않습니다.

`"Key": "stickiness.enabled"` 속성은 동일한 클라이언트의 요청을 동일한 타깃 그룹 안의 같은 타깃으로 계속 전달하는 기능을 활성화할지 여부를 의미합니다. 속성값이 `"Value": "true"`이기 때문에, 이 기능은 활성화되어 있습니다. 같은 타깃 그룹에 있는 태스크들은 고가용성을 위해 여러 AZ에 분산 배치되는 것이 일반적입니다. 이 기능은 동일한 클라이언트의 요청이 여러 AZ의 타깃으로 매번 분산되기보다, 처음 선택된 같은 타깃으로 계속 전달되게 합니다.

위 기능을 사용하는 이유는 같은 클라이언트의 요청이 여러 타깃으로 분산되면서 나타날 수 있는 문제나 비효율을 줄이기 위함입니다. 만약 여러 태스크에 요청이 분산된다면 각 태스크는 해당 클라이언트의 세션 상태나 캐시를 저장하거나 다시 만들어야 할 수 있습니다. 하나의 태스크에만 요청이 지속적으로 온다면 다른 태스크들은 해당 사용자의 세션 상태나 캐시를 저장할 필요가 줄어듭니다.

`"Key": "stickiness.type"` 속성은 `stickiness` 기능을 무엇을 기준으로 실행할 것이냐를 정합니다. 속성값 `"Value": "lb_cookie"`는 로드 밸런서가 클라이언트에 발급한 쿠키를 의미합니다. 로드 밸런서는 클라이언트에게 쿠키를 발급할 수 있습니다. 이 쿠키는 트래픽 등을 제어하는 데 사용됩니다.

`"Key": "stickiness.lb_cookie.duration_seconds"` 속성은 `stickiness` 기능이 얼마 동안 지속될지를 정합니다. 속성값이 `"Value": "300"`이기 때문에 동일한 클라이언트의 요청은 300초 동안 같은 타깃으로 전달됩니다.

---

`"TargetType"`은 타깃 그룹에 등록된 타깃을 어떤 기준으로 식별할지 정하는 필드입니다. 이 필드의 값이 `"ip"`이기 때문에 이 타깃은 IP 주소를 기준으로 등록되고 식별됩니다. ECS의 `awsvpc` 네트워크 모드에서는 Task의 ENI가 프라이빗 IP를 가지기 때문에, `"TargetType"`의 값은 보통 `"ip"`를 가집니다.

---

`"UnhealthyThresholdCount"`는 헬스 체크가 몇 번 연속으로 실패해야 해당 타깃을 비정상 상태로 판단할지 설정하는 필드입니다. 여기서는 값이 `3`이기 때문에, 헬스 체크에 3번 연속으로 실패해야 해당 타깃을 비정상 상태로 판단합니다.

---

`"VpcId"` 필드는 이 타깃 그룹이 속할 VPC를 명시합니다.

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
```

타깃 그룹을 생성하는 리소스입니다. 앞서 살펴본 기본 블루 타깃 그룹(`"BasicBlueTargetGroup17B9FA9D"`)과 유사한 구조를 가지고 있습니다. 

차이점으로는, 이 타깃 그룹은 `stickiness` 기능이 비활성화되어 있습니다. `stickiness` 기능이 비활성화되어 있는 이유는, 이 타깃 그룹이 새 컨테이너를 테스트할 때 사용되는 그린 타깃 그룹이기 때문에, 트래픽을 분산하여 전달해서 타깃이 잘 작동하는지 보기 위함으로 추측할 수 있습니다.

`"UnhealthyThresholdCount"`가 `3`이 아닌 `2`인 이유는 이 그린 타깃 그룹에 연결되어 있는 컨테이너를 더 보수적으로 검증하기 위함인 것으로 추측할 수 있습니다. 더 적은 연속 실패 횟수로 컨테이너의 상태를 판단하기 때문입니다.
