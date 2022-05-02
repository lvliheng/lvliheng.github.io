---
title: Navigation 按模块分包
---

想按模块分一下包

>nav_graph.xml

```java
<navigation
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/nav_graph"
    app:startDestination="@+id/home">

    <include app:graph="@navigation/home"/>
    <include app:graph="@navigation/list"/>
    <include app:graph="@navigation/form"/>

</navigation>
```

>home.xml

```java
<navigation
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/home"
    app:startDestination="@+id/titleScreen">

    <fragment
        android:id="@+id/titleScreen"
        android:name="com.example.android.navigationadvancedsample.homescreen.Title"
        android:label="@string/title_home"
        tools:layout="@layout/fragment_title">
        <action
            android:id="@+id/action_title_to_about"
            app:destination="@id/aboutScreen"/>
    </fragment>
</navigation>
```

其他两个也一样

但是，如果‘home’和‘list’的页面有交叉的就不行了
只能跳转到对应inclue的xml的入口
。。。
