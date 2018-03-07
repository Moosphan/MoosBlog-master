---
title: Recyclerview滑动实现悬浮按钮的显示和隐藏
date: 2017-10-29 11:13:39
tags: 
      - android
      - recyclerview
      - floatingActionButton
---
- 前言:

最近项目用到很多Recyclerview方面的知识,例如复杂列表布局的显示,瀑布流数据展示,商品列表的刷新和分页加载,列表右下方的悬浮按钮随着列表滚动方式来显示和隐藏等等。此处主要记录一下悬浮按钮随着recyclerview滑动而显示与隐藏(即下拉隐藏,上拉显示)。一般有两种常用方式:

1.通过FAB(FloatingActionButton)的layout_behavior属性来为其设置相应行为.
2.通过Recyclerview的滚动事件来监听和改变FAB悬浮按钮的状态.

第一种方式网上一堆资料,具体可以参考以下文章:
http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2016/0407/4126.html
下面主要通过第二种方式来实现该效果.
<!-- more -->
- 实现思路:

>可以通过监听recyclerview的滑动事件,设置上下滑动的距离判断,然后进行显示和隐藏FAB的操作即可

- 具体实现:
>设置scrollListener,监听滑动事件:
```
private int distance;

private boolean visible = true;

......
    
    
xRecyclerView.setOnScrollListener(new RecyclerView.OnScrollListener() {
            @Override
            public void onScrolled(RecyclerView recyclerView, int dx, int dy) {
                super.onScrolled(recyclerView, dx, dy);
                if(distance < -ViewConfiguration.getTouchSlop() && !visible){
                    //显示fab
                    //iv_go_uploading.setVisibility(View.VISIBLE);
                    showFABAnimation(iv_go_uploading);
                    distance = 0;
                    visible = true;
                }else if(distance > ViewConfiguration.getTouchSlop() && visible){
                    //隐藏
                    //iv_go_uploading.setVisibility(View.GONE);
                    hideFABAnimation(iv_go_uploading);
                    distance = 0;
                    visible = false;
                }
                if ((dy > 0 && visible) || (dy < 0 && !visible))//向下滑并且可见  或者  向上滑并且不可见
                    distance += dy;
            }
        });
```
>接下来需要进行显示和隐藏悬浮按钮的动画操作了:
```
/**
     * by moos on 2017.8.21
     * func:显示fab动画
     */
    public void showFABAnimation(View view)
    {
        PropertyValuesHolder pvhX = PropertyValuesHolder.ofFloat("alpha", 1f);
        PropertyValuesHolder pvhY = PropertyValuesHolder.ofFloat("scaleX", 1f);
        PropertyValuesHolder pvhZ = PropertyValuesHolder.ofFloat("scaleY", 1f);
        ObjectAnimator.ofPropertyValuesHolder(view, pvhX, pvhY,pvhZ).setDuration(400).start();
    }

    /**
     * by moos on 2017.8.21
     * func:隐藏fab的动画
     */

    public void hideFABAnimation(View view)
    {
        PropertyValuesHolder pvhX = PropertyValuesHolder.ofFloat("alpha", 0f);
        PropertyValuesHolder pvhY = PropertyValuesHolder.ofFloat("scaleX", 0f);
        PropertyValuesHolder pvhZ = PropertyValuesHolder.ofFloat("scaleY", 0f);
        ObjectAnimator.ofPropertyValuesHolder(view, pvhX, pvhY,pvhZ).setDuration(400).start();
    }
```
这里只是通过比较简单的透明度加大小的变换动画来实现其出现和隐藏效果,大家可以根据具体需要来设置自己的动画效果,如果你对属性动画不了解,可以去看看鸿神的文章:
http://blog.csdn.net/lmj623565791/article/details/38067475
http://blog.csdn.net/lmj623565791/article/details/38092093

剩下的布局代码就不贴了,相信没什么问题。


转载前请注明原文章链接,谢谢.
