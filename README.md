# ByRecyclerView

[![jitpack][1]][2] 
[![Apache License 2.0][3]][4]
[![API][5]][6]

ByRecyclerView 提供了下拉刷新、加载更多、 添加HeaderView/FooterView、setStateView、item点击/长按、万能分割线、粘性header、极简Adapter(databinding)等功能。

## 功能特性
 - 1.支持 下拉刷新、加载更多
 - 2.可随意切换 自带下拉刷新布局 / SwipeRefreshLayout
 - 3.加载更多机制：**不足一屏上拉加载，超过后触底加载**
 - 4.可设置 自定义 下拉刷新布局 和 加载更多布局
 - 5.可Add HeaderView、FooterView
 - 6.可设置 StateView状态布局
 - 7.可添加 item及子view的点击/长按事件(防止重复点击)
 - 8.优化过的BaseAdapter (RV/LV)，减少大量代码
 - 9.Adapter结合DataBinding使用 (RV/LV)
 - 10.可添加 万能分隔线（线性/宫格/瀑布流）
 - 11.可配置 Skeleton骨架图

> 最新：处理在ViewPager2中使用的问题

## Document
 -  👉 [**详细使用见Wiki！！！**](https://github.com/youlookwhat/ByRecyclerView/wiki)

 - [项目介绍](https://github.com/youlookwhat/ByRecyclerView/wiki/%E9%A1%B9%E7%9B%AE%E4%BB%8B%E7%BB%8D) | [更新日志 (1.1.5)](https://github.com/youlookwhat/ByRecyclerView/wiki/Update-log)
 - [ByRecyclerView：只为改变BRVAH加载更多机制/addHeaderView的问题](https://juejin.im/post/5e0980fbe51d4558083345fc)
 - [ByRecyclerView：真·万能分割线 (线性/宫格/瀑布流)](https://juejin.im/post/5e4ff123e51d4527255ca2e1)


## Screenshots
|刷新操作|设置状态布局|多类型列表|万能分割线|
|:--:|:--:|:--:|:--:|
|![刷新操作](https://github.com/youlookwhat/ByRecyclerView/blob/master/art/gif_refresh.gif?raw=true)|![设置状态布局](https://github.com/youlookwhat/ByRecyclerView/blob/master/art/gif_statue.gif?raw=true)|![多类型列表](https://github.com/youlookwhat/ByRecyclerView/blob/master/art/gif_adapter_type.gif?raw=true")|![分割线](https://github.com/youlookwhat/ByRecyclerView/blob/master/art/gif_divider.gif?raw=true")|


### 下载试用
[AndroidX版本(Apk-Demo)](http://d.firim.top/byrecyclerview)

<img width="250" height=“250” src="https://github.com/youlookwhat/ByRecyclerView/blob/master/art/png_dowload.png?raw=true"></img>

<!--|[AndroidX版本(Apk-Demo)](http://d.6short.com/byrecyclerview)|[Support版本(CloudReader)](https://www.coolapk.com/apk/127875)|
|:--:|:--:|
|<img width="200" height=“200” src="https://github.com/youlookwhat/ByRecyclerView/blob/master/art/png_dowload.png?raw=true"></img>|<img width="200" height=“200” src="https://raw.githubusercontent.com/youlookwhat/CloudReader/master/file/download_200.png"></img>|-->

## 如何使用  👉 [**Wiki文档**](https://github.com/youlookwhat/ByRecyclerView/wiki)

### 快速开始
1.先在 build.gradle 的 repositories 添加

```
allprojects {
	repositories {
		...
		maven { url "https://jitpack.io" }
	}
}
```

2.然后在dependencies添加

```
dependencies {
	// AndroidX版本引入
	implementation 'com.github.youlookwhat:ByRecyclerView:1.1.5'
}
```

3.在XML布局中引用 ByRecyclerView

```xml
<me.jingbin.library.ByRecyclerView
    android:id="@+id/recyclerView"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:layoutManager="LinearLayoutManager"
    tools:listitem="@layout/item_main" />
```

4.使用BaseRecyclerAdapter

```java
mRecyclerView.setLayoutManager(new LinearLayoutManager(this));
mRecyclerView.setAdapter(new BaseRecyclerAdapter<String>(R.layout.item_main, list) {
    @Override
    protected void bindView(BaseByViewHolder<String> holder, String bean, int position) {
        holder.setText(R.id.tv_text, bean);
    }
});

mAdapter.setNewData(list);   // 设置第一页数据
```
5.设置监听

```java
// 下拉刷新监听
mRecyclerView.setOnRefreshListener(new ByRecyclerView.OnRefreshListener() {
            @Override
            public void onRefresh() {
                // 刷新完成
                mRecyclerView.setRefreshing(false);
            }
        });
// 加载更多监听
mRecyclerView.setOnLoadMoreListener(new ByRecyclerView.OnLoadMoreListener() {
    @Override
    public void onLoadMore() {
         mAdapter.addData(list);            // 设置及刷新数据
         mRecyclerView.loadMoreComplete();  // 加载更多完成 
         mRecyclerView.loadMoreEnd();       // 没有更多内容了
         mRecyclerView.loadMoreFail();      // 加载更多失败,点击重试
    }
});
```

### ItemDecoration
万能分割线，可给Linear/Grid/StaggeredGrid设置，并可配置去除不显示分割线的头部和尾部个数

1.给LinearLayout设置分割线

```java
// 选择1：设置drawable
SpacesItemDecoration itemDecoration = new SpacesItemDecoration(this, SpacesItemDecoration.VERTICAL)
        .setNoShowDivider(1, 1)  // 第一个参数：头部不显示分割线的个数，第二个参数：尾部不显示分割线的个数，默认为1
        .setDrawable(R.drawable.shape_line);// 设置drawable文件

// 选择2：设置颜色、高度、间距等
SpacesItemDecoration itemDecoration = new SpacesItemDecoration(this, SpacesItemDecoration.VERTICAL)
        .setNoShowDivider(1, 1)
        // 颜色，分割线间距，左边距(单位dp)，右边距(单位dp)
        .setParam(R.color.colorBlue, 10, 70, 70);

recyclerView.addItemDecoration(itemDecoration);
```

2.给宫格/瀑布流设置分割线

```java
// 10：间距； true：距屏幕周围是否也有间距
GridSpaceItemDecoration itemDecoration = new GridSpaceItemDecoration(10, true)
        .setNoShowSpace(1, 1);// 第一个参数：头部不显示分割线的个数，第二个参数：尾部不显示分割线的个数，默认为1

recyclerView.addItemDecoration(itemDecoration);
```

### add HeaderView/FooterView、setStateView
```java
// 获取view对应databinding，注意：recyclerView.getParent()
LayoutHeaderViewBinding headerBinding = DataBindingUtil.inflate(LayoutInflater.from(this), R.layout.layout_header_view, (ViewGroup) binding.recyclerView.getParent(), false);
recyclerView.addHeaderView(headerBinding.getRoot());

recyclerView.addFooterView(getView() / layoutId));
recyclerView.setStateView(getView() / layoutId);

// headerView、footerView、setStateView 支持一键隐藏，设置后需要notify
recyclerView.setHeaderViewEnabled(false);
recyclerView.setFootViewEnabled(false);
recyclerView.setStateViewEnabled(false);

```

### Item 点击/长按监听
```java
// 防重复点击使用 OnItemFilterClickListener
mRecyclerView.setOnItemClickListener(new ByRecyclerView.OnItemClickListener() {
    @Override
    public void onClick(View v, int position) {
        // 通过adapter获取对应position的数据
        DataItemBean itemData = mAdapter.getItemData(position);
    }
});
mRecyclerView.setOnItemLongClickListener(new ByRecyclerView.OnItemLongClickListener() {
    @Override
    public boolean onLongClick(View v, int position) {
        return false;
    }
});

// 添加 子View的点击/长按事件
holder.addOnClickListener(R.id.tv_text);
holder.addOnLongClickListener(R.id.tv_text);

// 防重复点击使用 OnItemChildFilterClickListener
recyclerView.setOnItemChildClickListener(new ByRecyclerView.OnItemChildClickListener() {
    @Override
    public void onItemChildClick(View view, int position) {
    }
});
recyclerView.setOnItemChildLongClickListener(new ByRecyclerView.OnItemChildLongClickListener() {
    @Override
    public void onItemChildLongClick(View view, int position) {
    }
});
```

### 设置 Item悬浮置顶
```java
// 1、使用StickyLinearLayoutManager，传入adapter
StickyLinearLayoutManager layoutManager = new StickyLinearLayoutManager(getContext(), mAdapter);

// 2、在adapter里，将悬浮的item的ItemViewType设置为StickyHeaderHandler.TYPE_STICKY_VIEW
@Override
public int getItemViewType(int position) {
    if ("title".equals(getItemData(position).getType())) {
        return StickyHeaderHandler.TYPE_STICKY_VIEW;
    } else {
        return 2;
    }
}
```

### 设置Skeleton骨架图
1.设置item骨架图

```
// 显示
skeletonScreen = BySkeleton
        .bindItem(binding.recyclerView)
        .adapter(mAdapter)// 必须设置adapter，且在此之前不要设置adapter
        .shimmer(false)// 是否有动画
        .load(R.layout.layout_by_default_item_skeleton)// item骨架图
        .angle(30)// 微光角度
        .frozen(false) // 是否不可滑动
        .color(R.color.colorWhite)// 动画的颜色
        .duration(1500)// 微光一次显示时间
        .count(10)// item个数
        .show();

// 隐藏
skeletonScreen.hide();
```
2.设置view骨架图

```java
// 显示
skeletonScreen = BySkeleton
        .bindView(binding.recyclerView)
        .load(R.layout.layout_skeleton_view)// view骨架图
        .shimmer(true)// 是否有动画
        .angle(20)// 微光角度
        .color(R.color.colorWhite)// 动画的颜色
        .duration(1500)// 微光一次显示时间
        .show();

// 隐藏
skeletonScreen.hide();
```


## 与BRVAH、XRecyclerView对比
||ByRecyclerView| BRVAH | XRecyclerView |
|:--:|:--:|:--:|:--:|
|下拉刷新布局|继承基类自定义布局|无|只能简单设置样式|
|SwipeRefreshLayout|可配合使用|可配合使用|不能使用|
|加载更多布局|继承基类自定义布局|继承基类设置简单布局|继承基类自定义类|
|加载更多机制|不足一屏上拉加载，超过后触底加载|不足一屏即加载|触底加载|
|HeaderView|多ViewType区别|同一个item|多ViewType区别|
|FooterView|同一个item|同一个item|不能添加|
|EmptyView|可设置|可设置|不能设置|
|点击/长按事件|有|有| 无 |

ByRecyclerView 是XRecyclerView的拓展，可完全替换XRecyclerView，对于BRVAH它的优势在于：

 - headerView使用的是多type的形式，即一个header就是一个position
 - 不足一屏上拉加载，超过后触底加载
 - 自带下拉加载布局，也可使用三方刷新框架，比如SwipeRefreshLayout
 - 万能分割线(LinearLayout / GridLayout / StaggeredGridLayout)
 - 可设置悬浮置顶item StickyView
 - 可设置Skeleton 骨架图

缺点是还没有BRVAH里的部分功能，比如分组adapter、DiffUtils、item扩展动画...后期会逐步完善。

## 混淆
此资源库没有使用到任何序列化、反序列化、JNI、反射，无需进行额外的混淆操作，并且已经测试通过，在实际项目中使用，如果你在项目混淆之后出现问题，请及时联系我。

## Issues
### QQ讨论群 - 831860628
大家可以加群讨论使用本库时出现的问题，也可以直接提[Issues](https://github.com/youlookwhat/ByRecyclerView/issues)，我会第一时间帮助大家解决。如果此库帮助到了你，还请给个Star、Fork一下，我将更有动力持续迭代优化，非常感谢^_^。

## Thanks
 - [XRecyclerView](https://github.com/XRecyclerView/XRecyclerView)
 - [BaseRecyclerViewAdapterHelper](https://github.com/CymChad/BaseRecyclerViewAdapterHelper)

## About me
 - **QQ**： 770413277
 - **掘金**：[Jinbeen](https://juejin.im/user/56eec46d1ea49300555a176b/posts)
 - **CSDN**：[Jinbeen](https://blog.csdn.net/jingbin_)
 - **Email**： jingbin127@163.com

## License
```
Copyright (C) 2016 Bin Jing

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

[1]:https://jitpack.io/v/youlookwhat/ByRecyclerView.svg
[2]:https://jitpack.io/#youlookwhat/ByRecyclerView
[3]:https://img.shields.io/:License-Apache%202.0-blue.svg
[4]:https://www.apache.org/licenses/LICENSE-2.0.html
[5]:https://img.shields.io/badge/API-14%2B-red.svg?style=flat
[6]:https://android-arsenal.com/api?level=14