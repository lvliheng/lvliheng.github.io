---
title: 关于Android M 权限
---

  
  > Note: Beginning with Android 6.0 (API level 23), users can revoke permissions from any app at any time, even if the app targets a 
  > lower API level. You should test your app to verify that it behaves properly when it's missing a needed permission, regardless of 
  > what API level your app targets.
  
  从Android 6.0开始，系统可以管理应用的权限了。
  
  （用到某个权限的时候再去请求，随时关闭应用的某个权限，嗯，这样才更合理嘛！）
  
  具体流程：

  * 检查权限；
  * 请求权限；
  * 重写回调。
  
  官方文档的方法：

  {% highlight java %}
  // Here, thisActivity is the current activity
  if (ContextCompat.checkSelfPermission(thisActivity,
                  Manifest.permission.READ_CONTACTS)
          != PackageManager.PERMISSION_GRANTED) {
  
      // Should we show an explanation?
      if (ActivityCompat.shouldShowRequestPermissionRationale(thisActivity,
              Manifest.permission.READ_CONTACTS)) {
  
          // Show an expanation to the user *asynchronously* -- don't block
          // this thread waiting for the user's response! After the user
          // sees the explanation, try again to request the permission.
  
      } else {
  
          // No explanation needed, we can request the permission.
  
          ActivityCompat.requestPermissions(thisActivity,
                  new String[]{Manifest.permission.READ_CONTACTS},
                  MY_PERMISSIONS_REQUEST_READ_CONTACTS);
  
          // MY_PERMISSIONS_REQUEST_READ_CONTACTS is an
          // app-defined int constant. The callback method gets the
          // result of the request.
      }
  }
  {% endhighlight %}

  第一个条件：检查应用是否有读取通讯录的权限，如果返回的结果是“拒绝”进行下一步，
  
  {% highlight java %}
  if(ContextCompat.checkSelfPermission(thisActivity,
                  Manifest.permission.READ_CONTACTS)
          != PackageManager.PERMISSION_GRANTED){
      //用户拒绝了
  }
  {% endhighlight %}
  
  第二个条件：如果第一次用户拒绝了，再次检查时弹出的对话框多了一个选项“不再询问”。
  
  {% highlight java %}
  if (ActivityCompat.shouldShowRequestPermissionRationale(thisActivity,
              Manifest.permission.READ_CONTACTS)) {
      //勾选“不再询问”再次拒绝
      //这里可以打开系统设置页提示用户添加我们需要的权限
  } else {
      //用户在没有勾选“不再询问”的情况下拒绝了，则再次请求权限
      //最后一个参数是request code，一个页面需要请求多个权限时以便区分
      ActivityCompat.requestPermissions(thisActivity,
                    new String[]{Manifest.permission.READ_CONTACTS},
                    MY_PERMISSIONS_REQUEST_READ_CONTACTS);
  }
  {% endhighlight %}
  
  然后，处理请求结果。
  
  {% highlight java %}
  @Override
  public void onRequestPermissionsResult(int requestCode,
          String permissions[], int[] grantResults) {
      switch (requestCode) {
          case MY_PERMISSIONS_REQUEST_READ_CONTACTS: {
              // If request is cancelled, the result arrays are empty.
              if (grantResults.length > 0
                  && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
  
                  // permission was granted, yay! Do the
                  // contacts-related task you need to do.
  
              } else {
  
                  // permission denied, boo! Disable the
                  // functionality that depends on this permission.
              }
              return;
          }
  
          // other 'case' lines to check for other
          // permissions this app might request
      }
  }
  {% endhighlight %}
  
  最后，如果用户之前勾选了“不再提示”并拒绝了权限，现在又想使用某个需要权限的功能时，可以打开应用对应的系统设置页。
  
  {% highlight java %}
  private void goToSettings() {
      Intent myAppSettings = new Intent();
      myAppSettings.setAction(Settings.ACTION_APPLICATION_DETAILS_SETTINGS);
      myAppSettings.addCategory(Intent.CATEGORY_DEFAULT);
      myAppSettings.setData(Uri.parse("package:" + getPackageName()));
      myAppSettings.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
      myAppSettings.addFlags(Intent.FLAG_ACTIVITY_NO_HISTORY);
      myAppSettings.addFlags(Intent.FLAG_ACTIVITY_EXCLUDE_FROM_RECENTS);
      startActivityForResult(myAppSettings, REQUEST_APP_SETTINGS);
  }
  {% endhighlight %}
  
  That's all.
