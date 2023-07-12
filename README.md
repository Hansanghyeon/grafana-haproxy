<img width="1392" alt="스크린샷 2023-07-12 오전 1 17 47" src="https://github.com/Hansanghyeon/grafana-haproxy/assets/42893446/6814b169-3f19-4c99-ab35-ee359ea4ec23">

[HAProxy | Grafana Labs](https://grafana.com/grafana/dashboards/2428-haproxy/) 해당 대시보드를 haproxy 2.8에 호환하게 변경

## Haproxy + Prometheus + Grafana

- [Observability | Metrics | Prometheus | HAProxy Enterprise 2.7r1](https://www.haproxy.com/documentation/hapee/latest/observability/metrics/prometheus/)
- [Prometheus + Grafana + Docker Compose 설치 | devkuma](https://www.devkuma.com/docs/prometheus/docker-compose-install/)
- [New Docker Install with persistent storage, Permission problem - Grafana / Configuration - Grafana Labs Community Forums](https://community.grafana.com/t/new-docker-install-with-persistent-storage-permission-problem/10896/17)
- [A look at HAProxy native Prometheus metrics - Julien Pivotto](https://roidelapluie.be/blog/2019/11/27/haproxy-prometheus/)

> Replaced by better metrics:
>
> haproxy_backend_up: replaced by haproxy_backend_status  
> haproxy_server_up: replaced by haproxy_frontend_status with more values (0=STOP, 1=UP, 2=FULL, 2=MAINT, 3=DRAIN, 4=NOLB)
