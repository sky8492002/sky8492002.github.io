---
layout: default
title: View Binding
parent: Kotlin
---

1. TOC
{:toc .text-delta} 

## Function

##### 기본 틀: 주어질 파라미터의 타입, 함수 안에서 쓰일 이름과 반환할 타입이 명시된다.
{: .no_toc }
```kotlin
    fun function(param_name: param_type): return type{
        return returnValue
    }
```
##### 반환값이 없을 경우 반환할 타입은 명시되지 않는다.
{: .no_toc }
```kotlin
    fun function(param_name: param_type){
    }
```
##### 파라미터를 받지 않는 경우 파라미터의 타입과 이름은 명시되지 않는다.
{: .no_toc }
```kotlin
    fun function(){
        return returnValue
    }
```
<br/>

##### return값은 변수에 담아 활용할 수 있다. 
{: .no_toc }
```kotlin
    fun doubler(num: Int): Int{
        return num * 2
    }
    var number = doubler(100)
    Log.d("number", "${number}")
```
```
    number: 200
```

<br/>

##### 파라미터값은 Val형이기 때문에 함수 내부에서 변경 불가능하다(Immutable).
{: .no_toc }
```kotlin
    fun doubler(num: Int): Int{
        num = -1
        return num * 2
    }
```
```
    Val cannot be reassigned
```

<br/>

##### 여러 개의 파라미터를 받아올 수 있으며, 받아오지 않을 경우를 대비한 기본값을 설정할 수 있다.
{: .no_toc }
```kotlin
    fun info(name: String, age: Int = 25){
        Log.d("info", "name: ${name}, age: ${age}")
    }
    info("mike")
```

```
    info: name: mike, age: 25
```