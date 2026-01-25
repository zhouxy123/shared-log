26.01.25
应用页面分TabBar、非TabBar两种，区分页面的层级关系。
TabBar：切换到其他tab，表示在不同栏目之间切换 (switchTab)
非TabBar：跳转到其他页面，表示在该栏目进入到更深层级，通过手势/返回键返回上一页（navigateTo/redirectTo）,有可能会多次销毁/重建