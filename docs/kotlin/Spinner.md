---
layout: default
title: Spinner
parent: Kotlin
---

1. TOC
{:toc .text-delta} 

## Spinner : 여러 개의 목록 중에 하나를 선택할 수 있다.
##### Adapter는 데이터와 화면에 나타날 Spinner를 연결하며, 보여질 아이템의 레이아웃을 갖고 있다.
{: .no_toc }
<br/>

##### 1. design에서 Spinner를 추가한다.
{: .no_toc }
![sync](../images/spinner/spinner-locate.PNG)

##### 2. ArrayAdapter 클래스의 객체를 만든다. 인자는 (context, item layout, data)이다.
{: .no_toc }
```kotlin
    var data = mutableListOf("- please select -", "one", "two", "three")
    var adapter = ArrayAdapter<String>(this, android.R.layout.simple_list_item_1, data)
    // android.R.layout.simple_list_item_1은 기본으로 제공되는 item layout이다. 
```
##### 3. spinner에 ArrayAdapter 객체를 연결한다. 
{: .no_toc }
```kotlin
    private lateinit var binding: ActivityMainBinding // onCreate 밖
    ...
    binding = ActivityMainBinding.inflate(layoutInflater) // onCreate 안
    ...
    var spinner = binding.spinner
    spinner.adapter = adapter
```
##### 화면 출력
{: .no_toc }
![sync](../images/spinner/spinner-result1.PNG)
##### 4. spinner의 onItemSelectedListener method로 선택된 item에 대해 반응한다.
{: .no_toc }
```kotlin
    spinner.onItemSelectedListener = object : AdapterView.OnItemSelectedListener{
        override fun onNothingSelected(parent: AdapterView<*>?) {
            // 아무것도 선택 안될 시
        }
        override fun onItemSelected(parent: AdapterView<*>?, view: View?, position: Int, id: Long){
            // 무언가 선택되었을 시
        }
    }
```
##### 5. onItemSelected method의 position parameter 값으로 어떤 item이 선택되었는지 판단할 수 있다.
{: .no_toc }
```kotlin
    override fun onItemSelected(parent: AdapterView<*>?, view: View?, position: Int, id: Long){
        Log.d("spinnerResult", "${data.get(position)}")
    }
```
![sync](../images/spinner/spinner-result2.PNG)
```
    spinnerResult: - please select -
    spinnerResult: two
```

