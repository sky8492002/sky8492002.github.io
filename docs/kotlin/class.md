---
layout: default
title: 클래스 (class)
parent: Kotlin
---

1. TOC
{:toc .text-delta} 

## Class
{: .no_toc }
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

## 생성자
##### 클래스라는 그룹으로 묶여 있는 코드를 실행하기 위해 함수 형태로 제공되는 생성자를 호출한다.
{: .no_toc }
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

## Class의 사용
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
            bookTitle = "Title is ${bookTitle}"
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
    result_method: Title is dictionary
```
<br/>

##### companion object 블록을 사용하면 Instance화 없이 Property와 Method를 호출할 수 있다.
{: .no_toc }
```kotlin
    class Book{
        companion object{
            var bookTitle = "tales"
            fun modifyTitle() : String{
                bookTitle = "changed tales"
                return bookTitle
            }
        }
    }
    var title_by_property = Book.bookTitle
    var title_by_method = Book.modifyTitle()
    Log.d("result_property", "${title_by_property}")
    Log.d("result_method", "${title_by_method}")
```
```
    result_property: tales
    result_method: changed tales
```

##### <주의> 클래스 함수 내부에 있는 local 클래스는 companion object를 사용할 수 없다.
{: .no_toc }
```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)
            class Book{
                companion object{
                }
            }
        }
    }
```
```kotlin
    class Book{
        fun function{
            class Book2 {
                companion object {
                }
            }
        }
    }
```
```
    Modifier 'companion' is not applicable inside 'local class'
```
##### 중첩 클래스(nested class)에는 사용 가능하다.
{: .no_toc }
```kotlin
    class Book{
        class Book2{
            companion object{
            }
        }
    }
```
<br/>

## 데이터 클래스(data class)
##### data class는 일반 class와 다르게 toString으로 주소 대신 값을 불러올 수 있다.
{: .no_toc }
```kotlin
    class normalClass(var param1: String, var param2: Int)
    var normal = normalClass("zero", 0)
    Log.d("normalClass", "${normal.toString()}")

    data class dataClass(var param1: String, var param2: Int)
    var data1 = dataClass("one", 1)
    var data2 = dataClass(param1 = "two", param2= 2)
    Log.d("dataClass", "${data1.toString()}, ${data2.toString()}")
```
```
    normalClass: com.example.project_1.MainActivity$onCreate$normalClass@b719b2e
    dataClass: dataClass(param1=one, param2=1), dataClass(param1=two, param2=2)
```

<br/>
## 클래스의 상속과 확장
##### 자식 클래스는 부모 클래스의 Property와 Method를 사용할 수 있다.
{: .no_toc }
##### 상속할 부모 클래스는 open Class 형태로 사용해야 자식이 상속할 수 있다.
{: .no_toc }
```kotlin
    open class parentClass{
        var bookTitle = "";
        fun modifyTitle(): String{
            bookTitle = "Title is ${bookTitle}"
            return bookTitle
        }
    }
```
##### 상속받을 자식 클래스는 자식 클래스: 부모 클래스 형태로 사용해야 자식이 상속할 수 있다.
{: .no_toc }
```kotlin
    class ChildClass: ParentClass() {
        //bookTitle = "childBook"  함수 밖에서 바로 사용할 수 없다.
        fun work(){
            bookTitle = "childBook"
            modifyTitle()
        }
    }
    var child = ChildClass()
    child.work()
    Log.d("childClass", "${child.bookTitle}")
```
```
    childClass: Title is childBook
```
<br/>

##### 자식 클래스의 parameter를 부모 클래스의 parameter로 전달할 수 있다.
{: .no_toc }
```kotlin
    open class ParentClass(parentParam: String){
        var returnParentParam = parentParam
        fun returnParam(): String{
            // return parentParam  파라미터를 바로 return 할 수 없다.
            return returnParentParam
        }
    }
    class ChildClass(childParam: String): ParentClass(childParam) {
    }
    var child = ChildClass("hi")
    Log.d("param", "${child.returnParam()}")
```
```
    param: hi
```
<br/>

##### 자식 클래스의 Method는 재정의(Override) 될 수 있다. 단, 재정의 할 부모 Method가 open fun 형태여야 한다.
{: .no_toc }
```kotlin
    open class ParentClass(){
        open fun print(): String{
            return "parent"
        }
    }
    class ChildClass(): ParentClass() {
        override fun print() : String{
            return "child"
        }
    }
    var child = ChildClass()
    Log.d("overrideResult", "${child.print()}")
```
```
    overrideResult: child
```
##### 마찬가지로 부모 Property가 open 형태라면 자식 클래스에서 재정의할 수 있다.
{: .no_toc }
```kotlin
    open class ParentClass(){
        open var name = "parent"
    }
    class ChildClass(): ParentClass() {
        override var name = "child"
    }
```
<br/>

##### Extension : 이미 존재하는 클래스에 도트 연산자(.)로 Method를 추가하여 확장할 수 있다.
{: .no_toc }
```kotlin
    class Standard() {
        fun one() {}
        fun two() {}
    }
    fun Standard.three(): String{
        return "new fun"
    }
    var standard = Standard()
    Log.d("extension", "${standard.three()}")
```
```
    Log.d("extension", "${standard.three()}")
```
<br/>
## 추상화, 인터페이스
##### 추상화(abstract)와 인터페이스(interface)는 본 클래스를 구현하기 전에 개념만 설계해 놓은 클래스이다.
{: .no_toc }
##### interface는 Method/Property가 미구현 상태로 선언된다. interface가 아닌 코드 속에는 포함될 수 없다.
{: .no_toc }
```kotlin
    interface base{
        abstract fun one()
        fun two() // 기본적으로 abstract 키워드가 생략되어 있다. 즉 one과 two 함수는 같은 형태이다.
    }
    class real: base{
        override fun one(){}
        override fun two(){}
    }
```
```kotlin
    fun function{ 
        interface base {
            fun one()
            fun two()
        }
    } 
    // 'base' is an interface so it cannot be local. Try to use anonymous object or abstract class instead
```
<br/>

##### abstract class는 Method/Property가 일부 구현 상태로 선언된다.
{: .no_toc }
```kotlin
    abstract class base{
        abstract fun one(): String // 구현할 method와 return 타입이 같아야 한다.
        fun two():String{
            return "base에서 구현됨"
        }
    }
    class real: base() {
        override fun one() : String{
            return "real에서 구현됨"
        } // 미구현된 method와 return 타입이 같아야 한다.
        // override fun two(): String {}  이미 구현된 method는 재정의가 불가능하다.
    }
```
