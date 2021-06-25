---
layout: default
title: Project A
parent: Kotlin
---

1. TOC
{:toc .text-delta} 

## Project A 메모
##### 화면 전환 시 intent와 transaction 대신 jetpack의 navigation 기능을 사용한다.
{: .no_toc }
##### 하나의 Activity + 여러개의 Fragment 구성을 사용한다.
{: .no_toc }
##### 하나의 Activity의 layout에 navGraph를 사용하는 Fragment를 추가한다.
{: .no_toc }
```xml
    <fragment
            android:id="@+id/nav_host_fragment"
            android:name="androidx.navigation.fragment.NavHostFragment"
            android:layout_width="0dp"
            android:layout_height="0dp"
            app:defaultNavHost="true"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintHorizontal_bias="0.0"
            app:layout_constraintLeft_toLeftOf="parent"

            app:layout_constraintRight_toRightOf="parent"
            app:layout_constraintTop_toTopOf="parent"
            app:layout_constraintVertical_bias="1.0"
            app:navGraph="@navigation/nav_graph" />
```
##### view Binding으로 layout의 Fragment를 참조할 때 다음과 같은 방식을 사용한다.
{: .no_toc }
```kotlin
    // activity 안에서 참조할 시
    val navHostFragment = supportFragmentManager.findFragmentByTag("nav_host_fragment")
    // fragment 안에서 참조할 시
    val navHostFragment = childFragmentManager.findFragmentByTag("nav_host_fragment") 
```
##### NavController를 사용하여 화면을 전환한다.
{: .no_toc }
```kotlin
    class startFragment : Fragment() {
        lateinit var navController:NavController
        lateinit var binding: FragmentStartBinding
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
        }
        override fun onCreateView(
            inflater: LayoutInflater, container: ViewGroup?,
            savedInstanceState: Bundle?
        ): View? {
            binding = FragmentStartBinding.inflate(layoutInflater)
            val view = binding.root
            return view
        }
        // onViewCreated의 param view는 onCreateView에서 return한 view이다.
        override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
            super.onViewCreated(view, savedInstanceState)

            navController = Navigation.findNavController(view)
            Log.d("onClick", "notClicked")

            binding.button.setOnClickListener{
                Log.d("onClick", "clicked")
                navController.navigate(R.id.action_startFragment_to_secondFragment)
            }
        }
    }
```

##### EditText 내용 변경시 RecyclerView filter 기능 구현 - 전체를 갱신해야 오류가 나지 않는다.
{: .no_toc }
```kotlin
    //BetaViewStockFragment.kt
    class BetaViewStockFragment : Fragment() {
        ...
        fun checkEditText(adapter: AdapterBetaJongmokList, editText: EditText, listDto: List<JongmokDto>){
            editText.addTextChangedListener(object: TextWatcher{
                override fun afterTextChanged(s: Editable?) {
                    adapter.items =
                        listDto.filter { it.name.contains(editText.text.trim()) }
                }
                override fun beforeTextChanged(
                    s: CharSequence?,
                    start: Int,
                    count: Int,
                    after: Int
                ) {
                }
                override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) {
                }
            })  
        }
    }

    //AdapterBetaJonmokList.kt   
    class AdapterBetaJongmokList(val context: Context, val list: List<JongmokDto>): RecyclerView.Adapter<Holder>() {
        var items: List<JongmokDto> = list
            set(value){
                field = value
                notifyDataSetChanged() // 전체 갱신
            }
        ...
    }
```

##### rxAndroid - onNext, onError, onComplete
{: .no_toc }
##### AutoCompleteTextView - [leftEditText = AutoCompleteTextView]
{: .no_toc }
##### ItemTouchHelper
{: .no_toc }
##### OnDragListener - onDrag의 event.getAction() == DragEvent.ACTION_DROP는 자신을 포함한 view 안에서만 작동한다. 
{: .no_toc }

## BetaViewStockFragment.kt
```kotlin
    class BetaViewStockFragment : Fragment() {
        override fun onCreate...
        override fun onCreateView... -> FragmentBetaViewStockBinding
        override fun onViewCreated... -> RetrofitInstance, setRecyclerView, setRecyclerViewNotHelper, setItemsToGraph
        fun setItemsToGraph... -> RetrofitInstance, setChartData
        fun setRecyclerViewNotHelper... -> getDragListener
        fun setRecyclerView... -> getDragListener, ItemTouchhelper, setItemsToGraph
        fun checkEditText...
        fun setChartData...
        fun getDragListener... -> View.OnDragListener, setItemsToGraph
    }
    class LinearLayoutManagerWrapper: LinearLayoutManager{
        ...
    }
```
<br/>

## AdapterBetaJongmokList.kt
```kotlin
    class AdapterBetaJongmokList(val context: Context, val list: List<JongmokDto>): RecyclerView.Adapter<Holder>(){
        var items...
        fun addItem...
        fun removeItem...
        override fun onCreateViewHolder... -> ListJongmokBinding, Holder
        override fun getItemCount...
        override fun onBindViewHolder... -> Holder
    }
    class Holder(var jongmokViewByBind: ListJongmokBinding): RecyclerView.ViewHolder(jongmokViewByBind.root), View.OnLongClickListener{
        fun setItem... -> JongmokDto
        override fun onLongClick... -> View.DragShadowBuilder
    }
```


## BetaViewStockFragment.kt full code
```kotlin
    package com.qhedge.client.ui

    import...

    // TODO: Rename parameter arguments, choose names that match
    // the fragment initialization parameters, e.g. ARG_ITEM_NUMBER
    private const val ARG_PARAM1 = "param1"
    private const val ARG_PARAM2 = "param2"

    /**
    * A simple [Fragment] subclass.
    * Use the [BetaViewStockFragment.newInstance] factory method to
    * create an instance of this fragment.
    */
    class BetaViewStockFragment : Fragment() {
        // TODO: Rename and change types of parameters
        lateinit var binding: FragmentBetaViewStockBinding

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
        }

        override fun onCreateView(
            inflater: LayoutInflater, container: ViewGroup?,
            savedInstanceState: Bundle?
        ): View? {
            binding = FragmentBetaViewStockBinding.inflate(layoutInflater)
            val view = binding.root
            // Inflate the layout for this fragment
            return view
        }

        @SuppressLint("CheckResult")
        override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
            super.onViewCreated(view, savedInstanceState)
            RetrofitInstance(requireContext()).getJongmokList().subscribe({it2 ->

                var leftRecyclerView = binding.leftRecyclerView
                var rightRecyclerView = binding.rightRecyclerView

                var emptyList: List<JongmokDto> = emptyList()
                var leftAdapter = setRecyclerView(leftRecyclerView,emptyList)
                var rightAdapter = setRecyclerViewNotHelper(rightRecyclerView, it2)

                var leftEditText = binding.leftEditText
                var jongmokNameList = mutableListOf<String>()

                for(jongmok in it2){
                    jongmokNameList.add(jongmok.name)
                }
                val arrayAdapter = ArrayAdapter(requireContext(), android.R.layout.simple_list_item_1, jongmokNameList)
                leftEditText.setAdapter(arrayAdapter)

                var selectedItem: JongmokDto = JongmokDto()
                selectedItem.name = ""
                leftEditText.setOnItemClickListener{parent, view, position, id ->
                    var selectedPosition = jongmokNameList.indexOf(parent.getItemAtPosition(position))
                    selectedItem = it2[selectedPosition]

                    if (leftEditText.text.toString().equals(selectedItem.name.toString())){

                        var inThere = false
                        for (item in leftAdapter.items){
                            if (selectedItem.jongmokCode.equals(item.jongmokCode)){
                                inThere = true
                            }
                        }
                        if (inThere == false) {
                            leftAdapter.addItem(selectedItem.clone())
                            setItemsToGraph(leftAdapter)
                        }
                    }
                }

                var rightEditText = binding.rightEditText
                //checkEditText(leftAdapter, leftEditText, it2)
                checkEditText(rightAdapter, rightEditText, it2)

            })
        }

        @SuppressLint("CheckResult")
        fun setItemsToGraph(adapter: AdapterBetaJongmokList){
            var colorList = arrayListOf<String>("#FF0000", "#00FF00", "#0000FF", "#7FFFD4", "#8A2BF2")
            var listOfGraph = mutableListOf<List<StockDailyDto>>()
            var cnt = adapter.items.size

            for (item in adapter.items){
                RetrofitInstance(requireContext()).getStockDailyList(item).subscribe({
                    listOfGraph.add(it)
                }, {}, {
                    cnt --
                    //item.nameColor = colorList[cnt % 3]
                    if (cnt == 0) {
                        var itemNum = 0
                        for (item in adapter.items){
                            item.nameColor = colorList[itemNum % colorList.size]
                            itemNum ++
                        }
                        adapter.notifyDataSetChanged() // Adatper 데이터 갱신
                        setChartData(listOfGraph, adapter.items)
                    }
                })
            } // onNext, onError, onComplete - onNext에서 listOfGraph에 추가하고 onComplete에서 추가가 완료된
            if (adapter.items.size == 0){
                setChartData(listOfGraph, adapter.items)
            }// 하나 남은 상태에서 swipe 하여 제거할 때 대비
        }

        fun setRecyclerViewNotHelper(recyclerView: RecyclerView, listDto: List<JongmokDto>): AdapterBetaJongmokList{
            recyclerView.adapter = AdapterBetaJongmokList(requireContext(), listDto)
            var linearLayoutManagerWrapper = LinearLayoutManagerWrapper(requireContext())
            recyclerView.layoutManager = linearLayoutManagerWrapper
            recyclerView.setHasFixedSize(true)

            var dragListener = getDragListener()
            recyclerView.setOnDragListener(dragListener) // 자신에게 들어온 drag drop을 감지

            return recyclerView.adapter as AdapterBetaJongmokList
        }

        fun setRecyclerView(recyclerView: RecyclerView, listDto: List<JongmokDto>): AdapterBetaJongmokList{
            recyclerView.adapter = AdapterBetaJongmokList(requireContext(), listDto)
            var linearLayoutManagerWrapper = LinearLayoutManagerWrapper(requireContext())
            recyclerView.layoutManager = linearLayoutManagerWrapper
            recyclerView.setHasFixedSize(true)

            var dragListener = getDragListener()
            recyclerView.setOnDragListener(dragListener) // 자신에게 들어온 drag drop을 감지

    //        val customAdapterDataObserver = object: RecyclerView.AdapterDataObserver(){
    //            override fun onChanged(){
    //                setItemsToGraph(recyclerView.adapter as AdapterBetaJongmokList)
    //            }
    //        }
    //        (recyclerView.adapter as AdapterBetaJongmokList).registerAdapterDataObserver(customAdapterDataObserver)  // recyclerview의 데이터 변화 감지하여 새로운 그래프를 그리게 함

            ItemTouchHelper(object : ItemTouchHelper.SimpleCallback(0, ItemTouchHelper.LEFT or ItemTouchHelper.RIGHT){
                override fun onMove(
                    recyclerView: RecyclerView,
                    viewHolder: RecyclerView.ViewHolder,
                    target: RecyclerView.ViewHolder
                ): Boolean {
                    return true
                }
                override fun onSwiped(viewHolder: RecyclerView.ViewHolder, direction: Int) {
                    (recyclerView.adapter as AdapterBetaJongmokList).removeItem(viewHolder.adapterPosition)
                    setItemsToGraph(recyclerView.adapter as AdapterBetaJongmokList)
                } // item 양 옆으로 슬라이드 시 recyclerview에서 제거하고 새로운 그래프를 그리게 함
            }).apply{
                attachToRecyclerView(recyclerView) // 적용
            }

            return recyclerView.adapter as AdapterBetaJongmokList
        }

        fun checkEditText(adapter: AdapterBetaJongmokList, editText: EditText, listDto: List<JongmokDto>){
            editText.addTextChangedListener(object: TextWatcher{
                override fun afterTextChanged(s: Editable?) {
                    adapter.items =
                        (listDto.filter { it.name.contains(editText.text.trim())}).toMutableList()
                }
                override fun beforeTextChanged(
                    s: CharSequence?,
                    start: Int,
                    count: Int,
                    after: Int
                ) {
                }
                override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) {
                }
            })
        }

        fun setChartData(listOfGraph: MutableList<List<StockDailyDto>>, items: MutableList<JongmokDto>){
            var chartData = LineData()
            for(graph in listOfGraph){
                var entry = ArrayList<Entry>()
                var count = 0
                for (dailyData in graph){
                    entry.add(Entry(count.toFloat(), dailyData.close.toFloat()))
                    count +=1
                }

                var lineDataSet = LineDataSet(entry, graph[0].jongmokCode)
                for (item in items){
                    if (graph[0].jongmokCode.equals(item.jongmokCode))
                    {
                        lineDataSet.setColor(Color.parseColor((item.nameColor)))
                        lineDataSet.setCircleColor(Color.parseColor((item.nameColor)))
                        lineDataSet.setDrawCircles(false)
                    }
                }

                chartData.addDataSet(lineDataSet)
            }
            binding.linechart.setData(chartData)
            binding.linechart.invalidate()
        }

        fun getDragListener(): View.OnDragListener{ // item이 자신에게 drop되었는지 판단하여 행동
            return object: View.OnDragListener {
                override fun onDrag(v: View?, event: DragEvent?): Boolean { // v : drag를 인식하는 view
                    if (event != null) {
                        Log.d("event.result","${event.getAction()}")
                    }

                    if (event != null) {
                        when (event.getAction()){
                            DragEvent.ACTION_DROP->{ // item을 끌어다 놓을 시 본래 속해있던 view에서 이 view로 item이 이동하게 함
                                var positionTarget = -1
                                val viewSource: View = event.localState as View // drag하고 있는 view
                                var target:RecyclerView

                                if (v != null) {

                                    //target = v.rootView.rootView.findViewById(R.id.leftRecyclerView)
                                    target = v as RecyclerView // v는 drag하던 view의 drop을 인식하는 view (drop을 감지하는 view가 target이 되어야 함)

                                    if (viewSource != null){
                                        val source: RecyclerView = viewSource.parent as RecyclerView // item이 drag하기 전 있던 view

                                        var adapterSource : AdapterBetaJongmokList = source.adapter as AdapterBetaJongmokList
                                        var itemToMove: JongmokDto = JongmokDto()
                                        for (item in adapterSource.items){
                                            if (viewSource.text_code.text.equals(item.jongmokCode)){
                                                itemToMove = item.clone()
                                                break
                                            }
                                        }

                                        var adapterTarget : AdapterBetaJongmokList = target.adapter as AdapterBetaJongmokList // item이 drag된 view

                                        var inThere = false
                                        for (item in adapterTarget.items){
                                            if (itemToMove.jongmokCode.equals(item.jongmokCode)){
                                                inThere = true
                                            }
                                        }

                                        if (inThere == false) {
                                            adapterTarget.addItem(itemToMove)
                                            Log.d("onDrag", "${viewSource.parent.parent}")
                                            setItemsToGraph(adapterTarget)
                                        }

                                        return true
                                    }
                                }
                            }
                        }
                    }
                    return true
                }
            }
        }

    }

    class LinearLayoutManagerWrapper: LinearLayoutManager {
        constructor(context: Context) : super(context) {}
        constructor(context: Context, orientation: Int, reverseLayout: Boolean) : super(context, orientation, reverseLayout) {}
        constructor(context: Context, attrs: AttributeSet, defStyleAttr: Int, defStyleRes: Int) : super(context, attrs, defStyleAttr, defStyleRes) {}
        override fun supportsPredictiveItemAnimations(): Boolean { return false }
    }
```
<br/>

##  AdapterBetaJongmokList.kt full code
```kotlin
    package com.qhedge.client.adapter

    import ...

    class AdapterBetaJongmokList(val context: Context, val list: List<JongmokDto>): RecyclerView.Adapter<Holder>(){
        var items: MutableList<JongmokDto> = list.toMutableList()
            set(value){
                field = value
                notifyDataSetChanged()
            }

        fun addItem(item: JongmokDto){
            items.add(item)
            notifyDataSetChanged()
        }

        fun removeItem(position: Int){
            items.removeAt(position)
            notifyDataSetChanged()
        }

        override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): Holder {
            //val view = LayoutInflater.from(context).inflate(R.layout.list_jongmok, parent, false)
            val view = ListJongmokBinding.inflate(LayoutInflater.from(context),parent,false)
            return Holder(view)
        }

        override fun getItemCount(): Int {
            return items.size
        }

        override fun onBindViewHolder(holder: Holder, position: Int) {
            val jongmokData = items.get(position)
            holder.setItem(jongmokData)
        }

    }

    class Holder(var jongmokViewByBind: ListJongmokBinding): RecyclerView.ViewHolder(jongmokViewByBind.root), View.OnLongClickListener/*, View.OnDragListener*/{
        var bindResult = jongmokViewByBind
        init{
            bindResult.layoutInfo.visibility= View.GONE
            bindResult.root.setOnLongClickListener(this)
            //bindResult.root.setOnDragListener(this)
        }

        //private lateinit var jongmokViewByBind: ListJongmokBinding
        fun setItem(jongmokData: JongmokDto){
            //jongmokViewByBind = ListJongmokBinding.bind(jongmokView)
            bindResult.textName.text = "${jongmokData.name}"
            bindResult.textCode.text = "${jongmokData.jongmokCode}"
            bindResult.textStartDate.text = "${jongmokData.startDate}"
            bindResult.textStockCount.text = "${jongmokData.stockCnt}"
            bindResult.textName.setTextColor(Color.parseColor((jongmokData.nameColor)))
        }

        override fun onLongClick(v: View): Boolean {
            var shadowBuilder: View.DragShadowBuilder = View.DragShadowBuilder(v)
            v.startDragAndDrop(null, shadowBuilder, v, 0)
            Log.d("onLongClick", "${v}")
            return true
        } // 길게 누를 시 해당 view가 drag 상태로 바뀜

    }
```