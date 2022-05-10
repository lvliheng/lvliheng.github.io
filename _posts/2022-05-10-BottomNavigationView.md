---
title: 自定义BottomNavigationView
---

```java
navView = binding.navView

menuView = navView.getChildAt(0) as BottomNavigationMenuView

val homeView = menuView.getChildAt(0)
val homeItemView = homeView as BottomNavigationItemView

 val homeCustomView = LayoutInflater.from(this).inflate(R.layout.item_tab_custom, menuView, false)
homeCustomView.findViewById<ImageView>(R.id.icon).setBackgroundResource(R.mipmap.icon_home_select)
homeCustomView.findViewById<TextView>(R.id.name).setTextColor(ContextCompat.getColor(this, R.color.white))
homeItemView.addView(homeCustomView)
```

跳转二级页面时隐藏

```java
navController.addOnDestinationChangedListener { _, destination, _ ->
    when (destination.id) {
        R.id.home_fragment -> showBottomNav()
        ...        
        else -> hideBottomNav()
    }
}
```
