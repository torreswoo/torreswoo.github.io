---
bg: "posts/java/jolokia/jolokia02.png"
layout: post
title:  "SpringBoot | Jolokia JVM 모니터링"
crawlertitle: "Spring & SpringBoot & jolokia & JMX & 모니터링"
summary: "SpringBoot Jolokia 모니터링"
date:   2018-01-15 00:00:00 +0700
categories: posts
tags: ['SpringBoot', 'jolokia']
author: torreswoo
---

### contents
- intro : background 
- SpringBoot 애플리케이션 모니터링 
- SpringBoot jolokia

---

## intro : background 
  SpringBoot기반에 서버를 개발하는 일이 많이 있다. SpringBoot는 설정의 편리함을 가지고 Java로도 충분히 빠른속도로 개발 할 수 있다는 것을 보여주었다. Ruby On Rails, Django, Node.js와 같은 Framework 모두 각각의 장단점으로 적절한 프로젝트 개발을 진행할 수 있다. 자바는 설정의 복잡함과 높은 러닝커브등의 이유로 빠른 개발이 힘들다고 생각했다. 

  IT회사에 다니면서 확실히 '스프링기반의 자바 서버' 개발을 하는 일이 많다. 유지보수와 안정성이 가장 큰 이유일 것이다. 예전에 XML설정이 주를 이룬 스프링 개발방식을 지나, 애노테이션, 특히나 SpringBoot가 나오면서 사용성이 훨씬 좋아졌다고 생각한다.

  나는 SpringBoot로 대용량 데이터처리 백엔드 서버와 API서버 등을 개발했다. 적절한 부분에 부트를 상용한것 같다. 부트로 개발을 할때마다 반복적으로 필요한 것들이 있었다. Configuration, Actuator등 이미 부트에서 제공해주는 것들이 많이 있다. 이것들을 다시한번 정리해놓을 필요가있다. 또한 API를 위한 Swagger, JPA와 QueryDSL설정, 로깅과 테스팅 설정은 기본적으로 세팅하고 개발을 시작한다.


---

## SpringBoot 애플리케이션 모니터링 
 이런 여러가지 중에서 JVM과 애플리케이션의 상태를 모니터링하는 것은 중요하지만 쉽지 않기 때문에 놓지는 경우가 많다. SpringBoot Actuator를 사용하면 어느정도 상태를 모니터링 할수가 있다. VisualVM같은 툴도있고, jstat와 같은 command를 사용할 수도 있다.
 
```
- SpringBoot Actuator | https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#production-ready
- SpringBoot Actuator Remote Shell (v2.0에서 deprecated)
- VisualVM & VisualGC | https://visualvm.github.io/
- Scouter | https://github.com/scouter-project/scouter
- JMX jolokia | https://jolokia.org/features/overview.html
- jstat & jmap & jstack - 실제 서버에서 command로 JVM & GC상태를 알 수 있다
``` 
![springboot actuator](/assets/images/posts/java/jolokia/actuator01.png)
![VisualVM](/assets/images/posts/java/jolokia/visualvm01.png)
 
 
---

## SpringBoot - jolokia
![jolokia](/assets/images/posts/java/jolokia/jolokia01.png)
  SpringBoot에서 에서는 이미 jolokia가 설정이 default로 enable이다. 그래서 jolokia API로 Memory, OS, Threading정보등을 확인할 수 있다. 가장 자주 확인 하는 값은 현재 heap메모리 사용량이다. 대용량 트래픽을 처리하는 경우 수GB로 애플리 케이션을 실행시키고 heap메모리가 어느정도 올라가고 GC가 일어나면서 다시 heap이 떨어지는지 확인할 필요가 있다. 이런 API사용해서 차트로 보여줄필요가 있다. setInterval로 1초마다 값을 가져와서 차트를 업데이트한다.
  
  이미 jolokia와 highchart를 이용해서 dashboard를 만든 사례가 있어 이것을 사용해서 프로젝트를 만들어 봤다. 라이브러리 버전 업그레이드와 코스수정 및 css를 추가했다 
  참고 : http://www.nurkiewicz.com/2011/03/jolokia-highcharts-jmx-for-human-beings.html
  
![jolokia](/assets/images/posts/java/jolokia/jolokia03.png)


```
# configuration & usage
01. application.properties : https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-jmx.html
02. client library(javascript) : jolokia.js라이브러리추가 : https://jolokia.org/client/javascript.html
03. html &chart : dashboard-chart.js JMX jolokia api를 mbean설정하고, 찍러서 값을 가지고옴,  chart에 뿌림, setInterval로 1초마다 업데이트          
04. highchart : https://www.highcharts.com/demo
05. jolokia protocol 사용법 : https://jolokia.org/reference/html/protocol.html

# jolokia API usage
GET  /jolokia/read/java.lang:type=Memory
GET  /jolokia/read/java.lang:type=OperatingSystem
GET  /jolokia/read/java.lang:type=Threading
```
  
  
---
참고
- https://github.com/torreswoo/template-SpringBoot
- http://www.nurkiewicz.com/2011/03/jolokia-highcharts-jmx-for-human-beings.html
- https://jolokia.org/download.html
- https://oddpoet.net/blog/2013/09/26/jolokia-jmx-http-bridge/
- https://github.com/JM-Lab/jm-spring-boot-init

