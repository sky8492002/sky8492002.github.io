---
layout: default
title: 반복문(for, while)
parent: Kotlin
---

1. TOC
{:toc .text-delta} 

## for 반복문

##### 괄호 안에 반복할 범위를 지정한다.
{: .no_toc }

<br/>

##### index in a..b : a~b까지 반복한다. a <= index <= b
{: .no_toc }

```kotlin
    for (index in 1..3){
        Log.d("for_in", "${index}")
    }
```

```
    for_in: 1
    for_in: 2
    for_in: 3
```
<br/>

##### index in a until b : a부터 b 직전까지 반복한다. a <= index < b
{: .no_toc }

```kotlin
    for (index in 1 until 3){
        Log.d("for_in_until", "${index}")
    }
```

```
    for_in_until: 1
    for_in_until: 2
```
<br/>

##### index in a..b step c : a~b까지 반복하되 한 번 반복할 때마다 index를 c만큼 증가시킨다.
{: .no_toc }

```kotlin
    for (index in 0..9 step 3){
        Log.d("for_in_step", "${index}")
    }
```

```
    for_in_step: 0
    for_in_step: 3
    for_in_step: 6
    for_in_step: 9
```

<br/>

##### index in a downTo b : ..와 반대로 a~b까지 index를 감소시키며 반복한다. a >= index >= b
{: .no_toc }

```kotlin
    for (index in 3 downTo 1){
        Log.d("for_in_downTo", "${index}")
    }
```
```
    for_in_downTo: 3
    for_in_downTo: 2
    for_in_downTo: 1
```

<br/>

##### element in Array (or Collection) : 배열 또는 컬렉션 내 개수만큼 반복하여 각 요소에 접근한다.
{: .no_toc }

```kotlin
    var array = arrayOf("s", "t", "r")
    for (element in array){
        Log.d("for_in_array", "${element}")
    }
    var set = mutableSetOf("unique_one", "unique_two")
    for (element2 in set){
        Log.d("for_in_Collection", "${element2}")
    }
```

```
    for_in_array: s
    for_in_array: t
    for_in_array: r
    for_in_Collection: unique_one
    for_in_Collection: unique_two
```

<br/>

## while 반복문

##### 특정 조건이 만족(괄호 안 결과가 false)할 때까지 반복한다.
{: .no_toc }

<br/>

##### do와 함께 사용 시 do 블록 안 코드를 한번 실행 후 while문을 해당 코드로 반복한다.
{: .no_toc }
```kotlin
    var value = 0
    do{
        Log.d("do_while", "${value}")
        value +=1
    }while(value <3)
```
##### 즉 do 한 번 실행 후 일반 while문과 동일하게 진행된다.
{: .no_toc }
```kotlin
    while(value <3){
        Log.d("do_while", "${value}")
        value +=1
    }
```

```
    do_while: 0
    do_while: 1
    do_while: 2 
```

<br/>

##### break, continue는 while문을 빠져나가거나 코드 뒷부분을 건너뛸 때 사용한다.
{: .no_toc }

```kotlin
    var value = 0
    while(value <100){
        if (value ==3){
            Log.d("break", "${value}")
            break
        }
        if (value ==2){
            Log.d("continue", "${value}")
            value +=1
            continue
        }
        Log.d("while", "${value}")
        value +=1
    }
```

```
    while: 0
    while: 1
    continue: 2
    break: 3
```