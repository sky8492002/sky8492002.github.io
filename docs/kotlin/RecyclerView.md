---
layout: default
title: RecyclerView
parent: Kotlin
---

1. TOC
{:toc .text-delta} 

## RecyclerView
##### 1. design에서 RecyclerView를 추가한다. (id : recyclerView로 설정했다)
{: .no_toc }
![recyclerView-locate](../images/recyclerView/recyclerView-locate.PNG)
<br/>

##### 2. 화면에 보여질 item layout을 생성한다. ( item_recycler.xml )
{: .no_toc }
![recyclerView-locate](../images/recyclerView/item-layout.PNG)
<br/>

##### 3. item layout에 들어갈 값을 담을 data class를 생성한다.
{: .no_toc }
```kotlin
    // ItemData.kt
    data class ItemData (var itemId : Int, var itemTitle: String, var itemDetail: String?)
```
<br/>

##### 4. MainActivity에서 itemData class 타입의 List를 만들어 return하는 method( fun loadData )를 선언한다.
{: .no_toc }
```kotlin
    // MainActivity.kt
    override fun onCreate(savedInstanceState: Bundle?) {
        ...
        var detailMap = mutableMapOf<Int, String>()
        for (index in 0 until 100){
            var realTime = System.currentTimeMillis()
            var sdf = SimpleDateFormat("yyyy-MM-dd HH:mm:ss")
            var formattedTime = sdf.format(realTime)
            detailMap.put(index, "no : ${index}, time : ${formattedTime}")
        }
        fun loadData(): MutableList<ItemData>{
            val list:MutableList<ItemData> = mutableListOf()
            for (index in 0 until 100){
                val itemId = index
                val itemTitle = "item ${index}"
                val itemDetail = detailMap.get(index)
                var itemData = ItemData(itemId, itemTitle, itemDetail)
                list.add(itemData)
            }
            return list
        }
    }
```
<br/>

##### 5. adapter를 정의하기 위해 CustomAdapter class를 만들고 시스템에 있는 RecyclerView class로부터 상속받는다.
{: .no_toc }
```kotlin
    // CustomAdapter.kt
    class CustomAdapter : RecyclerView.Adapter<>() {
    }
```
<br/>

##### 6. RecyclerView.Adapter는 ViewHolder 클래스를 Generic(타입 파라미터로 명시)으로 지정해야 한다.
{: .no_toc }
```kotlin
    class CustomAdapter : RecyclerView.Adapter<CustomHolder>() {
    }
    class CustomHolder: RecyclerView.ViewHolder{
    }
```
<br/>

##### 7. RecyclerView.ViewHolder는 1개의 생성자가 필요하다. Adapter에서 넘겨주는 item layout을 전달한다.
{: .no_toc }
```kotlin
    class CustomHolder(itemView: View): RecyclerView.ViewHolder(itemView){
    } // 넘겨받는 item layout의 타입: View
```
<br/>

##### 8. RecyclerView.Adapter는 정상적으로 동작하기 위해 다음 3가지의 method를 구현해야 한다.
{: .no_toc }
```kotlin
    class CustomAdapter : RecyclerView.Adapter<CustomHolder>() {
        override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): CustomHolder {
            // item layout을 생성하고 CustomHolder를 return한다.
        }
        override fun getItemCount(): Int {
            // 사용할 item data의 총 개수를 return한다.
        }
        override fun onBindViewHolder(holder: CustomHolder, position: Int) {
            // 스크롤 시 화면에 item data와 layout을 연결한다.
        }
    }
```
<br/>

##### 8.1 item data를 담을 List를 선언한다. getItemCount는 이 List의 size를 return할 것이다.
{: .no_toc }
```kotlin
    class CustomAdapter : RecyclerView.Adapter<CustomHolder>() {
        var list = mutableListOf<ItemData>()
        ...
        override fun getItemCount(): Int {
            return list.size
        }
    }
```
<br/>

##### 8.2 onCreateViewHolder는 화면에 보일 item layout의 개수만큼 호출된다.
{: .no_toc }
##### 호출되면 item layout을 View 형태로 변환하고 CustomHolder에 담아 return한다.
{: .no_toc }
```kotlin
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): CustomHolder {
        val view = LayoutInflater.from(parent.context).inflate(R.layout.item_recycler, parent, false)
        // inflate(resource, root, attachToRoot)
        // resource: View로 변환할 layout 파일명
        // root: 변환될 View의 parent
        // attachToRoot: true, false로 return값 결정
        // -> attachToRoot가 true인 경우 root의 자식으로 추가되어 root return
        // -> attachToRoot가 false인 경우 xml 내 최상위 View return
        return CustomHolder(view)
    }
```
<br/>

##### 8.3 onBindViewHolder는 스크롤 시 호출되어 화면에 item data와 layout을 연결한다.
{: .no_toc }
##### 호출되면 앞에서 선언한 list에서 position을 위치로 하는 item data를 가져온다.
{: .no_toc }
```kotlin
    class CustomAdapter : RecyclerView.Adapter<CustomHolder>() {
        var list = mutableListOf<ItemData>()
        ...
        override fun onBindViewHolder(customHolder: CustomHolder, position: Int) {
            val itemData = list.get(position)
            customHolder.setItem(itemData)
            // CustomHolder에 itemData를 연결할 setItem method가 9에서 만들어진다.
        }
    }
```
<br/>

##### 9. CustomHolder에서 View 타입의 item layout에 item data를 연결하는 setItem method를 만든다.
{: .no_toc }
##### view binding으로 itemView의 각 요소에 접근한다.
{: .no_toc }
```kotlin
    class CustomHolder(itemView: View): RecyclerView.ViewHolder(itemView){
        private lateinit var itemViewByBind: ItemRecyclerBinding
        fun setItem(itemData: ItemData){
            itemViewByBind = ItemRecyclerBinding.bind(itemView)
            itemViewByBind.itemId.text = "${itemData.itemId}"
            itemViewByBind.itemTitle.text= "${itemData.itemTitle}"
            itemViewByBind.itemDetail.text = "${itemData.itemDetail}"
        }
    }
```
<br/>

##### 10. MainActivity로 돌아와 화면에 출력할 item data를 담은 list를 생성한 adapter의 list에 넣는다.
{: .no_toc }
```kotlin
    // MainActivity
    override fun onCreate(savedInstanceState: Bundle?) {
        ...
        fun loadData(): MutableList<ItemData>{
            ...
        }
        val list: MutableList<ItemData> = loadData()
        var adapter = CustomAdapter()
        adapter.list = list
    }
```
<br/>

##### 11. activity_main의 Recyclerview에 adapter를 연결하고, LinearLayout 형태로 설정한다.
{: .no_toc }
```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        ...
        // recyclerView: activity_main에서 설정한 RecyclerView의 id
        binding.recyclerView.adapter = adapter
        binding.recyclerView.layoutManager = LinearLayoutManager(this)
    }
}
```
<br/>

##### 결과
{: .no_toc }
![result](../images/recyclerView/result.PNG){: width="30%" height="30%"}

<br/>

##### 추가: CustomHolder에서 init 추가 후 setOnClickListener를 사용할 수 있다.
{: .no_toc }
```kotlin
    class CustomHolder(itemView: View): RecyclerView.ViewHolder(itemView){
        private lateinit var itemViewByBind: ItemRecyclerBinding
        fun setItem(itemData: ItemData){
            itemViewByBind = ItemRecyclerBinding.bind(itemView)
            itemViewByBind.itemId.text = "${itemData.itemId}"
            itemViewByBind.itemTitle.text= "${itemData.itemTitle}"
            itemViewByBind.itemDetail.text = "${itemData.itemDetail}"
        }
        init{
            itemView.setOnClickListener{
                itemViewByBind.itemDetail.text = "Clicked"
            }
        }
    }  
```
<br/>

##### 코드: MainActivity.kt
```kotlin
    class MainActivity : AppCompatActivity() {
        private lateinit var binding: ActivityMainBinding

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            binding = ActivityMainBinding.inflate(layoutInflater)
            val view = binding.root
            setContentView(view)

            var detailMap = mutableMapOf<Int, String>()
            for (index in 0 until 100) {
                var realTime = System.currentTimeMillis()
                var sdf = SimpleDateFormat("yyyy-MM-dd HH:mm:ss")
                var formattedTime = sdf.format(realTime)
                detailMap.put(index, "no : ${index}, time : ${formattedTime}")
            }
            fun loadData(): MutableList<ItemData> {
                val list: MutableList<ItemData> = mutableListOf()
                for (index in 0 until 100) {
                    val itemId = index
                    val itemTitle = "item ${index}"
                    val itemDetail = detailMap.get(index)
                    var itemData = ItemData(itemId, itemTitle, itemDetail)
                    list.add(itemData)
                }
                return list
            }
            val list: MutableList<ItemData> = loadData()
            var adapter = CustomAdapter()
            adapter.list = list
            binding.recyclerView.adapter = adapter
            binding.recyclerView.layoutManager = LinearLayoutManager(this)
        }
    }
```
##### 코드: CustomAdapter.kt
```kotlin
    class CustomAdapter : RecyclerView.Adapter<CustomHolder>() {
        var list = mutableListOf<ItemData>()
        override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): CustomHolder {
            val view = LayoutInflater.from(parent.context).inflate(R.layout.item_recycler, parent, false)
            return CustomHolder(view)
        }
        override fun getItemCount(): Int {
            return list.size
        }
        override fun onBindViewHolder(customHolder: CustomHolder, position: Int) {
            val itemData = list.get(position)
            customHolder.setItem(itemData)
        }
    }

    class CustomHolder(itemView: View): RecyclerView.ViewHolder(itemView){
        private lateinit var itemViewByBind: ItemRecyclerBinding
        fun setItem(itemData: ItemData){
            itemViewByBind = ItemRecyclerBinding.bind(itemView)
            itemViewByBind.itemId.text = "${itemData.itemId}"
            itemViewByBind.itemTitle.text= "${itemData.itemTitle}"
            itemViewByBind.itemDetail.text = "${itemData.itemDetail}"
        }
        init{
            itemView.setOnClickListener{
                itemViewByBind.itemDetail.text = "Clicked"
            }
        }
    }
```