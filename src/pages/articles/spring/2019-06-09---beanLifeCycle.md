---
title: 스프링 Bean Life Cycle
date: "2019-06-09"
layout: post
draft: false
path: "/posts/lifecycle/"
category: "SPRING"
tags:
  - "SPRING"
description: "스프링 Bean Life Cycle에 대해 알아보자"
---

>최범균님의 *스프링 프로그래밍 입문* 이라는 책을 통해 공부한 부분을 블로그에 정리하고자 합니다.
>잘못되거나 부족한 부분이 있으면 언제든 댓글로 가르침 부탁드립니다.

### 목차
1. 스프링 빈 라이프 사이클
2. 빈 객체의 초기화와 소멸 : 스프링 인터페이스
3. 빈 객체의 초기화와 소멸 : @Bean 태그 속성


## 1. 스프링 빈 라이프 사이클
컨테이너가 초기화 될 때 빈 객체 생성, 의존 주입, 초기화가 진행되고 컨테이너 종료시 빈 객체도 소멸된다.


## 2. 빈 객체의 초기화와 소멸 : 스프링 인터페이스
빈 객체의 초기화 시점과 소멸 시점을 알고 싶다면 스프링 프레임워크에서 제공하는 인터페이스를 구현하여 제공되는 메소드를 이용할 수 있다.  
초기화 - `InitializingBean` 인터페이스의 `afterPropertiesSet()` 메소드
소멸 - `DisposibleBean` 인터페이스의 `destroy()` 메소드


## 3. 빈 객체의 초기화와 소멸 : @Bean 태그 속성
인터페이스를 구현할 수 있는 클래스라면 다행이지만 외부에서 제공받거나 하는 클래스의 경우 인터페이스를 구현할 수 없다.  
이때 컨테이너 설정에서 @Bean 태그의 속성으로 초기화, 소멸 시점 메소드를 실행시킬 수 있다.  
`@Bean(initMethod="초기화시 실행할 메소드명" destoryMethod="객체소멸시 실행할 메소드명")`  
단, 두 메소드는 파라미터가 없는 메소드여야 한다. 파라미터가 있으면 익셉션 발생