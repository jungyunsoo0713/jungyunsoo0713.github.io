---
title: "Metrics and Access Logs"
weight: 3
---

```bash
export RETAIL_ALB=$(aws elbv2 describe-load-balancers --name retail-store-ecs-ui \
  --query 'LoadBalancers[0].DNSName' --output text)

hey -n 1000000 -c 1 -q 10 http://$RETAIL_ALB/home &
```

위 명령어는 `retail-store-ecs-ui` 로드 밸런서의 DNS 이름을 조회해 `RETAIL_ALB` 변수에 저장한 뒤, `hey` 도구를 사용해 UI 애플리케이션의 `/home` 경로로 지속적인 HTTP 요청을 보내는 명령어입니다.

`hey -n 1000000 -c 1 -q 10 http://$RETAIL_ALB/home &`는 `http://$RETAIL_ALB/home` 주소로 총 1,000,000번의 요청을 보내되, 동시 요청 수는 `1`개로 유지하고, 초당 최대 10개의 요청을 보내도록 설정합니다. 마지막의 `&`는 이 부하 발생 작업을 백그라운드에서 실행하겠다는 뜻입니다.

즉, UI 서비스에 일정한 트래픽을 만들어 Service Connect 로그나 접근 로그, 모니터링 데이터를 확인하기 위한 명령어입니다.

https://catalog.workshops.aws/ecs-immersion-day/en-US/50-lp-features/50-networking/ecs-service-connect/300-service-connect-metrics
