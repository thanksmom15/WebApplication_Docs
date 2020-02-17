# 개인과제
 ### 어쩌구저쩌구
 * 과제 끝나고 쓸꺼얌><

<br/>

# Prometheus 이해
 ### 구조
 https://github.com/prometheus/prometheus
 * ~~그림 및 설명 TOBEADDED~~

 ### Grafana까지 보여지는 과정
 * 레지스트리에 메트릭(카운터) 등록
    * 카운터 inc()
    * 5분마다 레지스트리의 메트릭을 pushGateway로 push
    * push 되어있는 메트릭을 프로메테우스 서버가 pull
    * 프로메테우스 서버에 grafana를 연동하여 시각화
        * 데이터소스를 추가하면서 프로메테우스 서버를 지정하여 추가

 ### 문법
  #### Metric
 * Counter
    * 값을 나타내는 메트릭
    * 증가만 할 수 있는 메트릭이라고 생각하면 됨
    * 값을 증가시키거나, 0으로 초기화해서 다시 시작하는 기능만 지원
    * ex) http total send bytes / http total request / running time / 오류 발생

 * Gauge
    * 값을 증가하거나 감소할 수 있음
    * ex) temperature / current cpu usage / current thread count

 * Histogram
    * 특정 기간동안 측정된 값을 표현
    * 모든 메트릭 데이터의 합계 제공
    * quantile을 계산할 수 있도록 지원
    * ex) TPS(Transaction per second) / 특정 기간동안의 집계나 API 콜 수 등을 보고싶을 때

 * Summary
    * 히스토그램과 비슷
    * φ-quantiles을 지원한다는 점이 차이점
    * 슬라이딩 시간단위의 윈도우를 계산할때 사용



<br/>

# Grafana 이해
 ### 사용 가이드
 https://support.cloudz.co.kr/support/solutions/articles/42000065881-cloud-z-%EB%AA%A8%EB%8B%88%ED%84%B0%EB%A7%81-%EC%82%AC%EC%9A%A9%EC%9E%90-%EA%B0%80%EC%9D%B4%EB%93%9C

 ### Query
 https://prometheus.io/docs/prometheus/latest/querying/basics/

