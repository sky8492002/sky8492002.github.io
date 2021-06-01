---
layout: default
title: if
parent: Kotlin
nav_order: 2
---

## if

bundle exec jekyll serve

#### 변수에 if문을 직접 대입할 수 있다.
```kotlin
    var a = 1
    var b = 2
    var bigger = if (a < b) {
        a = a - b
        b
    } else a

    Log.d("result", "$bigger, $a")
```
#### if문의 마지막 줄에 있는 b 값이 변수 bigger에 저장된다.
#### if문이 참이므로 a 값이 a-b = -1로 변경된다.
```
result: 2, -1
```