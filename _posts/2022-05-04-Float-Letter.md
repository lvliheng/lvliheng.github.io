---
title: 通讯录首字母悬停效果
---

翻出了好多年前写的代码

```java
private AbsListView.OnScrollListener scrollListener = new AbsListView.OnScrollListener() {
        @Override
        public void onScrollStateChanged(AbsListView view, int scrollState) {

        }
        @Override
        public void onScroll(AbsListView view, int firstVisibleItem,
                             int visibleItemCount, int totalItemCount) {
            if(D) Log.e("llh", "first visible item : " + firstVisibleItem);
            try {
                if (1 == firstVisibleItem || 0 == firstVisibleItem) {
                    contact_view_overlay_tv.setText("");
                    contact_view_overlay_ll.setVisibility(View.GONE);
                }
                else if (mContactAdapter.isLetterShow(firstVisibleItem - 2)) {
                    contact_view_overlay_ll.setVisibility(View.VISIBLE);
                    View v = view.getChildAt(0);
                    if (v == null) {
                        return;
                    }
                    int dex = v.getBottom() - contact_view_overlay_tv.getHeight();
                    if (dex <= 0) {
                        contact_view_overlay_ll.setPadding(0, dex, 0, 0);
                        if ("".equals(models.get(firstVisibleItem - 2).getPyin_first().trim())) {
                            contact_view_overlay_tv.setText("#");
                        } else {
                            contact_view_overlay_tv.setText(models.get(firstVisibleItem - 2).getPyin_first().trim());
                        }
                    } else {
                        contact_view_overlay_ll.setPadding(0, 0, 0, 0);
                        if ("".equals(models.get(firstVisibleItem - 2).getPyin_first().trim())) {
                            contact_view_overlay_tv.setText("#");
                        } else {
                            contact_view_overlay_tv.setText(models.get(firstVisibleItem - 2).getPyin_first().trim());
                        }
                    }
                } else {
                    contact_view_overlay_ll.setPadding(0, 0, 0, 0);
                    if ("".equals(models.get(firstVisibleItem - 2).getPyin_first().trim())) {
                        contact_view_overlay_tv.setText("#");
                    } else {
                        contact_view_overlay_tv.setText(models.get(firstVisibleItem - 2).getPyin_first().trim());
                    }
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    };
```

