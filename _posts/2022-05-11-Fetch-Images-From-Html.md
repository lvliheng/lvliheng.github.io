---
title: 从Html获取所有图片地址
---
```java
fun getSrcFromHtml(html: String): ArrayList<String> {
    val imgPattern = Pattern.compile("<img.*src\\s*=\\s*(.*?)[^>]*?>", Pattern.CASE_INSENSITIVE)
    val imgMatcher = imgPattern.matcher(html)

    var imagesStr = ""
    val images = ArrayList<String>()

    while (imgMatcher.find()) {
        imagesStr = imagesStr + "," + imgMatcher.group()
        val srcMatch = Pattern.compile("src\\s*=\\s*\"?(.*?)(\"|>|\\s+)").matcher(imagesStr)

        while (srcMatch.find()) {
             if (!srcMatch.group(1).isNullOrEmpty()) {
                  images.add(srcMatch.group(1)!!)
             }
        }
    }

    return images
}
```
