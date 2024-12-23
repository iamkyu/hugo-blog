---
layout: post
title: "책에서 찾은 도메인 서비스의 역할과 책임"
#description: ""
date: 2024-12-22
tags: []
comments: true
share: true
---

# 책에서 찾은 도메인 서비스의 역할과 책임

최근 팀 내에서 ‘도메인 서비스’의 책임과 역할에 관해 논의할 기회가 있었다.
그 과정에서 팀원들 간에 생각의 차이가 꽤 있다는 점을 느꼈다.
이 차이를 이해하고자 ‘도메인 서비스’의 개념과 목적을 다시 살펴보기로 했다.

특히, 이 개념을 처음 소개한(아마도) 에릭 에반스의 *도메인 주도 설계* (이대엽 역, 위키북스, 2011)와 함께,  
- 반 버논의 *도메인 주도 설계 구현* (윤창석, 황예진 역, 에이콘, 2016),  
- 블라드 코노노프의 *도메인 주도 설계 첫걸음* (김민석, 오창윤 역, 위키북스, 2022)  

등 주요 도서를 참고하여 개념을 종합해 보았다.
이 글은 개인적인 의견보다는 위 세 권의 책에서 ‘서비스’와 ‘도메인 서비스’ 개념에 관한 주요 인용구를 모아 정리한 글이다.

## 먼저 서비스란?

> 설계가 매우 명확하고 실용적이더라도 개념적으로 어떠한 객체에도 속하지 않는 연산이 포함 될 때가 있다. 이러한 문제를 억지로 해결하려 하기보다는 문제 자체의 면면에 따라 SERVICE를 모델에 명시적으로 포함할 수 있다. - 에릭 에반스, *도메인 주도 설계*, 이대엽 역, 위키북스, 2011.

에릭 에반스의 도메인 주도 설계에서 서비스는 다음과 같은 특성을 가진다고 한다.
- 다른 객체와의 관계를 강조
- 활동으로 이름을 지음(Entity 가 주로 동사나 명사로 이름을 부여하는 것과 달리)
- 서비스를 정의하는 기준은 순전히 사용하는 측에 무엇을 제공하느냐에 있음

또한 응용, 도메인, 인프라스트럭처 등 여러 계층에서 서비스라는 이름의 책임을 가지는 객체가 존재할 수 있다고 한다.

## 그럼 도메인 서비스란?

> 도메인의 개념 가운데 객체로는 모델에 어울리지 않는 것이 있다. 필요한 도메인 기능을 ENTITY나 VALUE에서 억지로 맡게 하면 모델에 기반을 둔 객체의 정의가 왜곡되거나, 또는 무의미하고 인위적으로 만들어진 객체가 추가될 것이다. (중략)
> 도메인의 중대한 프로세스나 변환 과정이 ENTITY나 VALUE OBJECT의 고유한 책임이 아니라면 연산을 SERVICE로 선언되는 독립 인터페이스로 모델에 추가하라. 모델의 언어라는 측면에서 인터페이스를 정의하고 연산의 이름을 UBIQUITOUS LANGUAGE의 일부가 되게끔 구성하라. SERVICE는 상태를 갖지 않게 만들어라.
> -에릭 에반스, *도메인 주도 설계*, 이대엽 역, 위키북스, 2011.

> 도메인 고유의 작업을 수행하는 무상태의 오퍼레이션 
> -반 버논, *도메인 주도 설계 구현,* 윤창석, 황예진 역, 에이콘, 2016.

> 비즈니스 로직을 구현한 상태가 없는 객체(stateless object)다. 대부분의 경우 이런 로직은 어떤 계산이나 분석을 수행하기 위한 다양한 시스템 구성요소의 호출을 조율 한다. 
> -블라드 코노노프, *도메인 주도 설계 첫걸음,* 김민석, 오창윤 역, 위키북스, 2022.

---

반 버논의 도메인주도설계구현에서 도메인 서비스는 아래와 같은 상황에 사용할 수 있다고 한다.
- 중요한 비즈니스를 수행할 때
- 어떤 컴포지션에서 다른 컴포지션으로 도메인 객체를 변형할 때
- 하나 이상의 도메인 객체에서 필요로 하는 입력 값을 계산할 때

> 도메인 모델은 일반적으로 비즈니스의 특정 측면에 집중된 소단위 행동을 처리하기 때문에, 도메인의 서비스는 **도메인 모델과 비슷한 원칙을 고수하는 경향**이 있다. 다수의 도메인 객체를 하나의 원자적 오퍼레이션으로 처리하므로 **복잡성이 약간 확대**될 가능성이 있다.
> -반 버논, *도메인 주도 설계 구현,* 윤창석, 황예진 역, 에이콘, 2016.

더불어 도메인 서비스 도출에 대해 아래와 같은 주의를 하기도 한다.
> 서비스로 도메인 개념을 모델링하는 데 너무 의존하지 말라. 상황이 적절할 때만 사용해야 한다. 신중을 기해서 사용하지 않으면 서비스를 모델링 문제를 해결하는 묘책이라 여길 수도 있다. 서비스를 지나치게 사용하면 애너믹 도메인 모델이 만들어지는 부정적인 결과를 초래할 수 있는데, 대부분의 도메인 로직이 엔터티와 값 객체 전체로 흩어지지 못하고 서비스에만 몰리게 된다.
> -반 버논, *도메인 주도 설계 구현,* 윤창석, 황예진 역, 에이콘, 2016.

에릭 에반스는 잘 만들어진 도메인 서비스는 아래와 같은 특징을 가진다고 한다.
1. 연산이 원래부터 ENTITY 나 VALUE OBJECT의 일부를 구성하는 것이 아니라 도메인 개념과 관련돼 있다
2. 인터페이스가 도메인 모델의 외적 요소의 측면에서 정의된다
3. 연산이 상태를 갖지 않는다

## 예제
반버논의 책에서 나온 예제로
- 설계 초기 단계 BacklogItem은 Product 의 일부로 구성된 애그리게잇으로 모델링 됨.
- 요구사항과 설계 전략의 변화로 두 모델은 분리 됨.
- 이에 따라 Product 모델의 `businessPriorityTotals()` 은  backlogItems을 전달마다 연산을 수행하게 변경 됨.
```java
public class Product {
    public static BusinessPriorityTotals businessPriorityTotals(
        Set<BacklogItem> backlogItems
    ) {
        // do something
    }
}
```

애그리게잇이 분리된 상황에서 Product가 `businessPriorityTotals()`라는 행위를 정적 메서드로 가지는게 올바른 해결책이었을까? 어쩐지 불편한 모델링이다.
이런 상황에서 도메인 서비스라는 모델링 도구를 통해 문제를 해결할 수 있다고 한다.

도메인서비스를 구현한 책의 예제는 아래와 같은 것들이 있다.
- [ResponseTimeFrameCalculationService](https://github.com/wikibook/lddd/blob/1693101887375709b95b6a3ffe518cef4b0e7cb4/listings/06-18.cs)
- [AuthorizationService](https://github.com/VaughnVernon/IDDD_Samples/blob/master/iddd_identityaccess/src/main/java/com/saasovation/identityaccess/domain/model/access/AuthorizationService.java)

## 책 구매하기
- [도메인 주도 설계, 에릭 에반스 저, 이대엽 역, 위키북스, 2011.](https://search.shopping.naver.com/book/catalog/32464065589?query=%EB%8F%84%EB%A9%94%EC%9D%B8%20%EC%A3%BC%EB%8F%84%20%EC%84%A4%EA%B3%84&NaPm=ct%3Dm4zbh1w8%7Cci%3D46aafe0eb258e205e92b515aa9ac40e5858be5e9%7Ctr%3Dboksl%7Csn%3D95694%7Chk%3Dfbb8d0fafd1d050b83e109cb9f20d70e4006f6ec)
- [도메인 주도 설계 구현, 반 버논 저, 윤창석, 황예진 역, 에이콘, 2016.](https://search.shopping.naver.com/book/catalog/32458977251?query=%5B%EB%8F%84%EB%A9%94%EC%9D%B8%20%EC%A3%BC%EB%8F%84%20%EC%84%A4%EA%B3%84%20%EA%B5%AC%ED%98%84%2C&NaPm=ct%3Dm4zbhv7s%7Cci%3D017ac9c67a399dc50fafc78e62898a674e23e5de%7Ctr%3Dboksl%7Csn%3D95694%7Chk%3Dfc63650f0ec227b845d4f2d81bdf9c79de747ad1)
- [도메인 주도 설계 첫걸음, 블라드 코노노프 저, 김민석, 오창윤 역, 위키북스, 2022.](https://search.shopping.naver.com/book/catalog/32557654619?query=%EB%8F%84%EB%A9%94%EC%9D%B8%20%EC%A3%BC%EB%8F%84%20%EC%84%A4%EA%B3%84%20%EC%B2%AB%EA%B1%B8%EC%9D%8C&NaPm=ct%3Dm4zbi58w%7Cci%3D5e64fe3e355cf2551d6b151282f421ab2f435825%7Ctr%3Dboksl%7Csn%3D95694%7Chk%3D0607559aa8b51f6dfb81cdfeb56681ae7535203d)