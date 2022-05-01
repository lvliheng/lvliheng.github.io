---
RecyclerView 重排序后列表位置问题
---

RecyclerView自带的动画效果真的很不错
不过，在做“商品列表按照价格排序”这种需求的时候
遇到了一个问题
重拍序后列表自动滚动了到底部
就需要这样让RecyclerView保持在顶部位置

```Java
listAdapter.submitList(yourNewList) { 
    yourRecyclerView.scrollToPosition(0) 
}
```

下面是另一个高赞的回答，还没有验证

```Java
adapter.registerAdapterDataObserver(object : RecyclerView.AdapterDataObserver() {
    override fun onItemRangeInserted(positionStart: Int, itemCount: Int) {
        (recycler_view.layoutManager as LinearLayoutManager).scrollToPositionWithOffset(positionStart, 0)
    }
})
```

另外，在使用Paging的时候
新的问题来了
submitData()方法就没有commitCallBack了
就不能像上面的那种方式解决问题了

