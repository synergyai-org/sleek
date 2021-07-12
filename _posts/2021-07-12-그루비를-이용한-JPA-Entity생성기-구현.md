---
layout: post
title: Groovy를 이용한 JPA Entity & Repository generator 만들기
summary: IntelliJ의 IDEA에서 자동 JPA 생성기를 구현하기 위한 처절한 투쟁기입니다.
featured-img: groovy_logo
categories: Groovy JPA Generator
---

## JPA란?

해당 포스트에서는 JPA에 대해 간략한 내용만 정리하고 넘어가겠습니다.  
JPA는 자바진영에서 제공하는 ORM기반 데이터베이스 추상화 라이브러리입니다. 실제 사용할 때는 구현체인 Hibernate와 함께 사용하게 됩니다.

데이터베이스의 관계를 영속성(Persistance)으로 저장하고 쿼리에 따른 데이터를 캐시에 담아둠으로 최적화된 형태로 DB를 사용할 수 있도록 도와줍니다. 엔티티와 레포지토리를 이용한 편리한 확장 기능도 제공합니다.

## Entity & Repository

엔티티는 JPA를 사용하기 위해 데이터베이스의 테이블을 자바 클래스 오브젝트로 구현해두는 구현체를 의미합니다. 레포지토리는 디비와 직접 연결하여 사용할 수 있는 다양한 메소드를 제공해주게 됩니다. JPA를 사용하게 된다면 Entity와 Repository를 지속적으로 구현하고 정의해줘야 합니다.  

이런 작업을 최소화 하고 테이블의 변경에 따른 Entity의 재정의를 빠르고 간략하게 구현하기 위한 Genorator구현을 결심하게 되었습니다.

## IntelliJ IDEA의 기본 Generator

인텔리제이에서 기본적으로 제공하는 Entity생성기가 존재합니다. 하지만 해당 생성기는 기본 설정을 변경할 수 없으며 팀에서 설정한 세팅 저장도 불가했습니다. Entity를 재생성할때마다 모든 이름을 이전으로 다시 설정해야 하고 updatable, insertable등의 부가 설정을 위해서는 결국 Entity클래스를 일부 수정하는 작업을 거쳐야했기때문에 아쉬운점이 여럿있었습니다.

이런 고민이 결국 Generator를 직접만들어보자는 생각으로 이어졌습니다. 마침 Backend파트에 잠깐 짬이나는 타이밍이기도 했습니다.

## Script Extentions

인텔리제이에서는 스크립트를 이용한 확장프로그램을 구현할 수 있도록 제공합니다.  
[이미지 넣을것!!!!!!!!!!!!!!!!!!!!!]  
Database에서 위의 이미지처럼 따라들어가게 되면 특정 폴더내에 스크립트를 작성하고 사용할 수 있습니다.

Groovy와 kotlin둘중 하나를 선택하여 구현할 수 있습니다. Kotlin의 경우 kt파일을 매우 잘 생성해주지만 참고하여 Java파일을 생성하기에는 어려웠기때문에 Groovy를 선택했습니다.. (~~다 만들고 생각해보니 왜...Groovy로 했을까...~~)

기존에 존재하는 Groovy파일을 복제하여 Baseline으로 잡고 구현을 시작했습니다.


## 인텔리제이의 기본 제공 파라미터 파악하기
