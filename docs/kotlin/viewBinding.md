---
layout: default
title: View Binding
parent: Kotlin
---

1. TOC
{:toc .text-delta} 

## ViewBinding
##### findViewById를 쓰는 대신 XML의 view component에 접근하는 object를 반환받아 view에 접근하는 방식
{: .no_toc }
##### 1. Gradle Scripts / build gradle(Module)에 접근한다.
{: .no_toc }
![gradle](../images/viewBinding/gradle.PNG)
<br/>

##### 2. android 항목에 다음과 같이 작성한다. 
{: .no_toc }
![buildFeatures](../images/viewBinding/buildFeatures.PNG)
<br/>

##### 3. 상단의 sync now를 눌러 적용한다.
{: .no_toc }
![sync](../images/viewBinding/sync.PNG)
<br/>

##### 4. 적용할 Activity에서 layout xml에 대응하는 클래스의 instance를 생성한다.
{: .no_toc }
##### - activity_main.xml에 대응하는 클래스는 ActivityMainBinding으로 자동 설정된다.
{: .no_toc }
##### - lateinit 예약어는 binding이 onCreate가 호출된 후에 생성되게 한다. 
{: .no_toc }
```kotlin
    class MainActivity : AppCompatActivity() {
        private lateinit var binding: ActivityMainBinding
        override fun onCreate(savedInstanceState: Bundle?) {
            ...
        }
    }
```
<br/>

##### 5. onCreate method 안에서 inflate method를 호출한다. 생성된 객체(binding).root로 view를 참조할 수 있다.
{: .no_toc }
```kotlin
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        val view = binding.root
        setContentView(view) // 기존 방법 : setContentView(R.layout.activity_main)
    }
```
<br/>

##### 6. 생성된 객체인 binding에 도트 연산자(.)를 붙여 layout의 요소를 참조할 수 있다.
{: .no_toc }
```kotlin
    val intent = Intent(this, SubActivity::class.java)
    binding.btnStart.setOnClickListener{startActivity(intent)}
```
<br/>

##### 추가 정보: 특정 View를 명시할 수 있다면 inflate 대신 bind를 사용할 수 있다.
##### itemView는 xml의 layout을 View 형태로 변환한 상태이므로 bind를 사용할 수 있다.( recyclerView 9번 항목 참조 )
{: .no_toc }
```kotlin
```