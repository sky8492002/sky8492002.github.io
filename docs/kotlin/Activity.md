---
layout: default
title: Activity
parent: Kotlin
---

1. TOC
{:toc .text-delta} 

## Activity에서 다른 Activity로 값 주고받기
##### target Activity가 명시된 Intent를 생성한다. 실행할 Activity::class.java 형식을 사용한다.
{: .no_toc }
```kotlin
    val intent = Intent(this, SubActivity::class.java)
```
##### 생성한 Intent를 startActivity method에 담아 호출한다.
{: .no_toc }
```kotlin
    startActivity(intent)
```
<br/>

##### target Activity는 Activity Manager에 의해 전달된 Intent의 데이터를 사용할 수 있다.
{: .no_toc }
##### putExtra method로 key-value 형태의 데이터를 보낼 수 있다.
{: .no_toc }
```kotlin
    // Main Activity
    val intent = Intent(this, SubActivity::class.java)
    intent.putExtra("key", "value")
    startActivity(intent)
```
##### get( Type )Extra method로 Main Activity가 보낸 Intent로부터 key에 해당하는 value를 받을 수 있다.
{: .no_toc }
```kotlin
    // Sub Activity (target Activity)
    var value = intent.getStringExtra("key") // intent는 Activity의 기본 속성으로 따로 정의할 필요가 없다.
    Log.d("received by intent", "${value}")
```
```
    received by intent: value
```

<br/> 

##### target Activity를 호출한 Activity는 taget Activity가 종료할 때 보낸 Intent를 전달받을 수 있다.
{: .no_toc }
##### intent 객체 설정(실행할 Activity가 명시될 필요가 없다) 후 setResult로 상태값과 intent를 보낸다.
{: .no_toc }
```kotlin
    // Sub Activity (target Activity)
    val intent = Intent()
    intent.putExtra("returnKey", "returnValue")
    setResult(Activity.RESULT_OK, intent) // 실패 판단 시 Activity.RESULT_CANCELED를 대신 보낼 수 있다.
    finish()
```
##### onActivityResult로 target Activity가 보낸 intent를 처리할 수 있다.
{: .no_toc }
##### 단, target Activity를 호출할 때 호출한 주체를 파악하기 위해 startActivityForResult method를 사용했어야 한다.
{: .no_toc }
```kotlin
    // Main Activity
    override fun onCreate(savedInstanceState: Bundle?) {
        ...
        var requestCode = 99
        startActivityForResult(intent, requestCode) // 돌려받을 때 requestCode로 호출한 주체(버튼 등)을 파악한다.
    }
    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        // resultCode : Activity.RESULT_OK
        if (resultCode == Activity.RESULT_OK){
            when (requestCode){
                99-> {
                    var returnValue = data?.getStringExtra("returnKey") 
                    // data?는 data에 NULL값이 올 수 있음을 의미한다.
                }
            }
        }
    }
```