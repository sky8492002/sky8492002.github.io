---
layout: default
title: when
parent: Kotlin
nav_order: 3
---

## when

#### 인자값에 따라 실행되는 코드가 결정된다. 
```kotlin
    var goto = 1
    when (goto) {
        1 -> {
            Log.d("when", "goto = 1")
        }
        2 -> {
            Log.d("when", "goto = 2")
        }
         else -> {
            Log.d("when", "goto가 앞의 모든 조건에 맞지 않음")
        }
    } 
```
##### goto가 1이므로 1 -> 뒤의 함수{}가 실행된다.
```
when: goto = 1
```
<br/>

##### 변수 in a..b 는 a <= 변수 <= b를 의미한다. 

```
    when (goto){
        in 1..10 ->{
            Log.d("when", "1 <= goto <= 10")
        }
        else ->{
            Log.d("when", "1 > goto or  10 < goto")
        }
    }
```

```
when: 1 <= goto <= 10
```