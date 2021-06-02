---
layout: default
title: 클래스 (class)
parent: Kotlin
---

1. TOC
{:toc .text-delta} 

## Class
##### 기본 구조: 클래스명을 설정한 클래스에 변수, 함수를 내장할 수 있다.
{: .no_toc }
```kotlin
    class ClassName{
        var value
        fun function(){
        }
    }
```
<br/>

### 생성자
##### 클래스라는 그룹으로 묶여 있는 코드를 실행하기 위해 함수 형태로 제공되는 생성자를 호출한다.
{: .no_toc }

<br/>

##### 프라이머리(Primary) 생성자:  class 키워드와 같은 위치에 constructor 키워드로 작성되며, 파라미터를 받을 수 있다.
{: .no_toc }
```kotlin
    class ClassName constructor(param_name : param_type){
    }
```
<br/>

##### 클래스의 생성자가 호출될 때 실행되는 init 블록은 생성자를 통해 넘어온 파라미터에 접근할 수 있다.
{: .no_toc }
```kotlin
    class ClassName constructor(param_name : param_type){
        init{
            var received = param_name
        }
    }
```
<br/>

##### 접근 제한자나 특정 옵션이 없다면 프라이머리 생성자의 constructor 키워드를 생략할 수 있다.
{: .no_toc }
```kotlin
    class ClassName(param_name : param_type){
        init{
            var received = param_name
        }
    }
```
<br/>

##### 세컨더리(Secondary) 생성자:  constructor 키워드를 함수처럼 작성할 수 있다.
{: .no_toc }
```kotlin
    class ClassName {
        constructor(param_name : param_type){
            var received = param_name
        }
    }
```

<br/>

### Class의 사용
##### 클래스의 생성자 호출 후 생성되는 Instance는 객체(Object)라고도 부르며, 변수에 담을 수 있다.
{: .no_toc }
```kotlin
    class Book(bookName : String){
    }
    var book = Book("dictionary")
```
<br/>

##### 클래스의 변수를 맴버 변수(또는 Property), 함수를 맴버 함수(또는 Method)라고 부른다.
{: .no_toc }
##### Property와 Method는 도트 연산자(.)로 호출할 수 있다. 
{: .no_toc }
```kotlin
    class Book(bookName : String){
        var bookTitle = "None"
        init{
            bookTitle = bookName
        }
        fun modifyTitle(): String{
            bookTitle = "Title : ${bookTitle}"
            return bookTitle
        }
    }
    var book = Book("dictionary")
    var title_by_property = book.bookTitle
    var title_by_method = book.modifyTitle()
    Log.d("result_property", "${title_by_property}")
    Log.d("result_method", "${title_by_method}")
```
```
    result_property: dictionary
    result_method: Title : dictionary
```