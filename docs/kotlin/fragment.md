---
layout: default
title: Fragment
parent: Kotlin
---

1. TOC
{:toc .text-delta} 

## Fragment
##### 하나의 Activity로 서로 다른 레이아웃을 구성할 수 있도록 설계
{: .no_toc }

## Fragment의 생명 주기(Life cycle)
{: .no_toc }
```
    생성 주기 method
    onAttach: Activity에 fragment가 추가되고  commit 되는 순간 호출된다. context가 인자로 주어진다.
    onCreate: fragment 생성 시 호출되며, 자원(변수 또는 resource)를 초기화한다.
    onCreateView: fragment의 view를 inflater를 사용하여 구성(초기화)한다.
    onActivityCreated: Activity의 onCreate()가 반환될 때 호출된다. 
    onStart: fragment가 새로 add되거나 화면에 사라졌다 나타날 경우 onCreateView 대신 onStart가 호출된다.
    onResume: onStart와 용도는 같으나 onPause 상태에서 화면에 나타날 경우 onStart를 건너뛰고 호출된다.
    
    소멸 주기 method
    onPause: fragment가 화면에서 다른 Activity 또는 fragment에 밀릴 때 호출된다.
    onStop: 밀린 fragment가 화면에서 완전히 보이지 않을 때 호출된다.
    onDestroyView: onCreateView()에서 생성한 view가 fragment에서 분리되었을 때 호출되어 view가 소멸된다.
    onDestroy: fragment에 연결된 모든 자원을 해제한다.
    onDetach: Activity에서 연결 해제된다.
```

## Fragment를 Activity source code에 추가하기
##### 1. new -> Fragemnt -> Fragment(Blank)로 Fragment code file을 생성한다.
{: .no_toc }
##### 2. Activity에 fragment를 
{: .no_toc }


## Fragment를 Layout에 추가하기 