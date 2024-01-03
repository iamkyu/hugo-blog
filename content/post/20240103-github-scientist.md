---
layout: post
title: "Github Scientist 로 두 시스템 응답 비교하기"
#description: ""
date: 2024-01-03
tags: []
comments: true
share: true

---

# Github Scientist 로 두 시스템 응답 비교하기

같은 코드 베이스에서 상호작용하던 코드 일부를 물리적으로 분리 된 시스템, 새로운 프로그래밍 언어로 이식하는 과정에서 기존 코드와 신규 코드 동작의 일치 여부 검증이 필요했다.

조회성 유스케이스의 구현 코드들이었고 동작을 여러번 수행해도 결과가 달라지지 않았기에 병행 실행 후 두 결과를 비교하는 것이 안전하다 판단했다. 
두 시스템을 (1) 병행 실행 (2) 결과를 비교 (3) 결과를 보고 하는 일련의 과정의 보일러 플레이트 코드를 줄이기 위한 실용적인 방법을 고민하던 중 [“마이크로서비스 도입, 이렇게 한다(샘 뉴먼 저)”](https://search.shopping.naver.com/book/catalog/32485088006?cat_id=50010921&frm=PBOKPRO&query=%EB%A7%88%EC%9D%B4%ED%81%AC%EB%A1%9C%EC%84%9C%EB%B9%84%EC%8A%A4+%EB%8F%84%EC%9E%85%2C+%EC%9D%B4%EB%A0%87%EA%B2%8C+%ED%95%9C%EB%8B%A4&NaPm=ct%3Dlqx6b7co%7Cci%3D4a5f92a3a4069ea32790815ec27ec1f9b59b50f6%7Ctr%3Dboknx%7Csn%3D95694%7Chk%3Dbc572c04ec637074e2975abede999d09a3a0df47) 책에서 병행실행 패턴을 구현한 [Github Scientist](https://github.com/github/scientist) 라이브러리를 알게 됐다.

> A Ruby library for carefully refactoring critical paths.

루비 코드로 작성된 라이브러리 지만 이미 다양한 기여자가 PHP, 닷넷, 파이썬, 자바 등으로 이식한 버전이 있었고 내가 필요했던 자바 버전의 선택지는 두가지였다.

- https://github.com/rawls238/Scientist4J
- https://github.com/MisterSpex/misterspex-scientist

`rawls238/Scientist4J` 버전이 Micrometer 와 같은 매트릭 수집 도구와 연동도 잘 된 편이지만 이는 불 필요했기에 의존성이 최소화 된 `MisterSpex/misterspex-scientist` 를 채택했지만 두 버전 모두 기본적인 구조는 유사하다.
1. `Experiment` 라는 실험을 만들고
2. 1번에서 만든 실험 내에서 각 시스템의 호출을 수행. 결과를 `Observation` 에 담는다.
3. 두 `Observation` 를 담은 `Result` 를 만든다.

그리고 `Experiment` 에는 비어 있는 `publish(Result result)` 를 제공하는데 여기에 원하는 방식으로 `Result` 의 비교, 결과 수집을 구현 하면 된다.

비교 구현은 자바의 `equals` 구현을 검토했지만 [JDK-8031085](https://bugs.openjdk.org/browse/JDK-8031085) 과 같은 버그로 한 시스템에서만 보정을 수행하여 차이가 발생했다. 이 차이는 중요하지 않은 유스케이스였지만 이런 각각의 엣지케이스에 대응하는 `equals` 를 구현하는게 합당해보이지 않았고 기존 코드 변경을 최소화 하고 싶었기에 모든 비교 대상에 `equals` 구현도 부담스러웠다.

대안으로 구글 Gson 을 통해 객체를 Json Tree 로 변환 후 재귀적으로 순회하며 `Map` 에 담아 비교하는 방식을 택했다. Tree 순회 구현은 ChatGPT 의 도움을 받았는데 신선한 경험이었다.
이 방식의 장점 중 하나는 객체의 속성 중 차이가 있는 속성명 수집이 쉬웠고, 이를 통해 속성명에 따라 다른 방식으로 비교 할 수 있는 점이다. 예컨대 앞서 말한 JDK 버그의 경우 속성명이 `at` 으로 끝나는 값은 초 까지만 비교하는 것이다. 
비교 수행 후 값이 다른 속성에 한해 Kibana 에 수집하여 결과 차이를 쉽게 파악할 수 있기도 했다.

조회성 유스케이스에서 병행 실행 패턴은 적절하게 선택이었지만 변경을 유발하는 유스케이스에는 맞지 않았다. 다행히 복잡하지 않고 명령이 많은 시스템으로 전달되지 않았기에 회귀 테스트로도 많은 품이 들진 않았지만 좀 더 효율적인 방법을 고민하고 있다.