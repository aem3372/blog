title: 'SwipeMenuListView派生Adapter造成滚动显示异常'
id: 1518
categories:
  - android
date: 2015-05-14 23:47:39
tags:
---

因为测试时，暂时没有将Adapter的getView写成最大化利用原View的方式，而直接填充了一个新View，所以发现了项目的一个bug。该bug表现为在getView时传入旧View，你却返回了新View，新View不能被利用。现已将bug修复并提交到原仓库。

> 项目地址：[SwipeMenuListView](https://github.com/baoyongzhang/SwipeMenuListView "https://github.com/baoyongzhang/SwipeMenuListView")

<!--more-->

原始代码（有bug）：

```java
public class SwipeMenuAdapter implements WrapperListAdapter,
		OnSwipeItemClickListener {

	private ListAdapter mAdapter;
	private Context mContext;
	private OnMenuItemClickListener onMenuItemClickListener;

	@Override
	public View getView(int position, View convertView, ViewGroup parent) {
		SwipeMenuLayout layout = null;
		if (convertView == null) {
			View contentView = mAdapter.getView(position, convertView, parent);
			SwipeMenu menu = new SwipeMenu(mContext);
			menu.setViewType(mAdapter.getItemViewType(position));
			createMenu(menu);
			SwipeMenuView menuView = new SwipeMenuView(menu,
					(SwipeMenuListView) parent);
			menuView.setOnSwipeItemClickListener(this);
			SwipeMenuListView listView = (SwipeMenuListView) parent;
			layout = new SwipeMenuLayout(contentView, menuView,
					listView.getCloseInterpolator(),
					listView.getOpenInterpolator());
			layout.setPosition(position);
		} else {
			layout = (SwipeMenuLayout) convertView;
			layout.closeMenu();
			layout.setPosition(position);
			//这里有问题，可以看到如果新生成view，这个view不能被利用
			View view = mAdapter.getView(position, layout.getContentView(),
					parent);
		}
		return layout;
	}
	//...
}
```

改写逻辑修正：

```java
/**
 * 
 * fix: Display abnormal, When mAdapter return View different from 
 *      convertView and convertView is not null
 */
@Override
public View getView(int position, View convertView, ViewGroup parent) {
	SwipeMenuLayout layout = null;
	View view = null;
	if (convertView != null) {
		layout = (SwipeMenuLayout) convertView;
		layout.closeMenu();
		layout.setPosition(position);
		view = mAdapter.getView(position, layout.getContentView(),
				parent);
	}

	if (convertView == null || view != layout.getContentView()) {
		if(view == null) {
			view = mAdapter.getView(position, convertView, parent);
		}
		SwipeMenu menu = new SwipeMenu(mContext);
		menu.setViewType(mAdapter.getItemViewType(position));
		createMenu(menu);
		SwipeMenuView menuView = new SwipeMenuView(menu,
				(SwipeMenuListView) parent);
		menuView.setOnSwipeItemClickListener(this);
		SwipeMenuListView listView = (SwipeMenuListView) parent;
		layout = new SwipeMenuLayout(view, menuView,
				listView.getCloseInterpolator(),
				listView.getOpenInterpolator());
		layout.setPosition(position);
	}
	return layout;
}
```