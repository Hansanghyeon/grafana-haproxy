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

## Grafana 집계를 위해서 haproxy frontend 분리하기

현재 나의 인프라에서 gateway 역활에 haproxy가 로드밸런서 역활을하고있다.

같은 서버내부에서 proxy하는 것이기 때문에 고가용성을 얻지는 못하지만 내부에 VM이 다운되어서 로드밸런싱하는 것까지는 가능하다!

지금까지는 haproxy의 설정이

```
frontend http_in
	mode http
	bind *:80
	option forwardfor

	acl host_example hdr(host) -i example.com
	acl host_example hdr_sub(host) -i example.com # *.example.com 접속
	use_bakcend http_example if host_exmaple
	
	acl host_example2 hdr(host) -i example2.com
	acl host_example2 hdr_sub(host) -i example2.com # *.example2.com 접속
	use_bakcend http_example2 if host_exmaple2

backend http_example
	server localhost 127.0.0.1:80 check
	
backend http_example2
	server localhost 222.0.0.1:80 check
```

이런식으로 acl내부에서 여러개의 도메인을 분리하고 거기에 여러 backend를 사용하는 방식으로 구성했다.

이렇게하고 haproxy의 metric을 살펴보면 `proxy="http_in"`으로만 나온다.

grafana에서 각 서비스마다 대시보드를 만들고 통합 대시보드를 만들고 이런것을 생각했기 때문에 `proxy="example"` `proxy="example2"` 이런식으로 나와야하지 않을까? 고민을 했다.

### frontend로 분리를 했으니까 acl 조건으로까지 분리를 하지않고 도메인을 바인딩 하면되지 않을까?

wildcard 형식으로 도메인을 분리해서 backend를 선택하는 방식으로 해서 bind에는 wildcard 정규식으로 bind가 안되는 것을 알게되었다.

