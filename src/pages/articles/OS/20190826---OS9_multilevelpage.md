---
title: 운영체제9 - 페이지 테이블의 보완
date: "2019-08-02"
layout: post
draft: false
path: "/posts/OS/multi"
category: "OS"
tags:
  - "OS"
description: "페이지 테이블의 메모리 차지 해결하기"
---

> 운영체제를 공부하기 위해 *운영체제 아주 쉬운 세 가지 이야기* 라는 책을 읽고 공부한 내용을 정리합니다.  
> 잘못되거나 부족한 부분이 있으면 언제든 댓글로 가르침 부탁드립니다.


### 목차
1. 더 큰 페이지
2. 하이브리드 접근 방법 : 페이징과 세그먼터
  
## 1. 더 큰 페이지
페이지 테이블의 2가지 문제점 중 이전 포스팅에서 접근성 문제에 대해 얘기했다. 두 번째로 페이지 테이블 자체의 크기가 너무 크다는 문제점이 있다. 이를 단순하게 해결하는 방법은 페이지 크기를 증가시켜서 PTE 개수를 줄이는 방법이 있다. 하지만 이는 부작용도 수반한다. 페이지 내부의 낭비 공간이 증가하는 *내부 단편화* 문제가 발생한다. 프로세스가 페이지를 할당받지만 그 페이지의 일부만 사용하게 되고 이는 금방 메모리 고갈로 이어진다.
   
  
## 2. 하이브리드 접근 방법 : 페이징과 세그멘트
페이징과 세그멘테이션을 결합하여 페이지 테이블의 크기를 감소시키는 방법이 있다. 세그멘트마다 따로 페이지 테이블을 두는 것이다. 베이스, 바운드 레지스터 또한 마찬가지로 3개로 나뉜다. 각 바운드 레지스터 값은 세그멘트의 최대 유효 페이지의 개수를 나타내고, 유효 범위를 넘어가는 곳에 대한 메모리 접근은 예외를 발생시켜, 선형 페이지 테이블에 비해 메모리 사용을 개선시킬 수 있다.  

하지만 여전히 문제점은 존재한다. 1) 세그멘테이션은 일반적인 드문드문 사용되는 주소 공간을 지원할 만큼 충분히 유연하지 않다.(크기가 큰, 드문드문 사용되는 힙이 하나의 세그멘트로 배정되었을 때 이 힙에 접근하려면 힙 전체가 메모리에 존재해야 한다.) 2) 하이브리드 기법은 외부 단편화를 유발한다. 페이지 테이블은 다양한 크기를 갖기 때문이다.
   
  
## 3. 멀티 레벨 페이지 테이블
### 3.1 멀티 레벨 페이지 테이블이란?
선형 페이지 테이블을 트리 구조로 변형하여 크기를 줄이는 방법이다. 페이지 테이블을 페이지 크기 단위로 나눈 다음, 페이지 테이블의 유효하지 않은 항목이 있다면 해당 페이지는 할당하지 않는다. 또한 *페이지 디렉터리*라는 자료구조를 이용하여 페이지 테이블 각 페이지 할당 여부와 위치를 파악한다.  
  
* 페이지 디렉터리 (Page Directory) : 페이지 테이블을 구성하는 페이지의 존재 여부와 위치 정보를 가진 자료구조이다. 페이지 디렉터리는 페이지 디렉터리 항목(Page Directory Entries)으로 구성되며 각 항목은 유효 비트와 페이지 프레임 번호(PFN)을 갖고 있다.


### 3.2 장점
1. 크기 감소  
사용되는 페이지만큼만 페이지 테이블이 형성되므로 페이지 테이블 크기를 감소시킬 수 있다.  
2. 공간 할당의 유연성  
선형 페이지 테이블은 연속된 메모리 공간에 할당해야 하기에 할당이 쉽지 않지만, 이 경우 페이지 디렉터리를 통해 페이지 위치를 파악할 수 있으므로 공간 할당이 매우 유연하다.  

### 3.3 단점
1. 메모리 로드 추가 발생  
선행 페이지 테이블의 경우, 주소 변환과정에서 메모리 로드가 1회 발생한다. 하지만 이 경우, 페이지 디렉터리와 PTE 각 1회씩 2회의 메모리 로드가 발생한다. (물론, TLB 히트 성능은 동일하다)  
2. 복잡도 증가  
단순 선형 구조에서 복잡도가 더 증가한다.



