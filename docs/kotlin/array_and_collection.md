---
layout: default
title: 배열과 컬렉션
parent: Kotlin
---

1. TOC
{:toc .text-delta} 

## 배열(Array)

##### int, long, char, float, double 형식의 배열을 만들 수 있다.
{: .no_toc }

```kotlin
    var intArray = IntArray(10)
    var longArray = LongArray(10)
    var charArray = CharArray(10)
    var floatArray = FloatArray(10)
    var doubleArray = DoubleArray(10)
```
<br/>

##### string 형태 배열은 기본 배열의 공간을 빈 문자열로 변경하거나 직접 할당하여 사용한다.
{: .no_toc }
```kotlin
    var stringArray = Array(10, {item->""})]
    var stringArray = arrayOf("s", "t", "r", "i", "n", "g")
```
<br/>

##### 배열의 값은 index에 따라 두가지 방식으로 변경하고 불러올 수 있다.
{: .no_toc }
```kotlin
    intArray[index] = 1
    var value = intArray[index]

    intArray.set(index, 1)
    var value = intArray.get(index)
```
<br/>

## 컬렉션(Collection)
##### 접두어 mutable을 붙여 동적으로 사용한다. 붙이지 않을 시 선언 이후 변경 불가능하다.
{: .no_toc }

### List

##### 빈 list 생성 시 타입을 명시해야 한다.
{: .no_toc }

```kotlin
    var stringList = mutableListOf<String>()
```
<br/>

##### add, removeAt 함수로 값을 추가하고 삭제한다. set 함수로 특정 위치의 값을 변경할 수도 있다.
{: .no_toc }

```kotlin
    var list = mutableListOf("one", "two")
    list.add("three") // one, two, three
    list.removeAt(1)  // one, three
    list.set(1, "3") // one, 3
```
<br/>

### Set

##### list와 달리 중복을 허용하지 않는다. 이미 있는 값은 add해도 추가되지 않는다.
{: .no_toc }


```kotlin
    var set = mutableSetOf<String>()
    set.add("one")
    set.add("two")
    set.add("two")
    Log.d("Set", "${set}")
```
```
    Set: [one, two]
```

<br/>

##### 값이 unique하기 때문에 remove(값)으로 삭제하는 것이 가능하다.
{: .no_toc }

```kotlin
    set.remove("one")
    Log.d("Set", "${set}")
```
```
    Set: [two]
```

<br/>

### Map

##### key와 value의 쌍으로 입력된다. key 없이는 값에 접근할 수 없다.
{: .no_toc }

```kotlin
    var map = mutableMapOf<String, String>() // <key 타입, value 타입>
    map.put("key1", "value1")
    map.put("key2", "value2")
    Log.d("Map", "${map}")
```

```
    Map: {key1=value1, key2=value2}
```

<br/>

##### put 함수 사용 시 해당 key가 존재하지 않으면 추가, 존재하면 수정 작업을 수행한다.
{: .no_toc }

```kotlin
    map.put("new key", "new value") // 값 추가
    map.put("key1", "modified value") // 값 수정
    Log.d("Map", "${map}")
```

```
    Map: {key1=modified value, key2=value2, new key=new value}
```

<br/>

##### get, remove 함수로 값을 가져오거나 제거할 수 있다. key, value 쌍은 변하지 않는다.
{: .no_toc }

```kotlin
    map.remove("new key")
    var value = map.get("key1")
    Log.d("Map", "${map}")
    Log.d("Get", "${value}")
```

```
    Map: {key1=modified value, key2=value2}
    Get: modified value
```

