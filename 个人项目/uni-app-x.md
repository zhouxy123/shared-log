26.01.25
应用页面分TabBar、非TabBar两种，区分页面的层级关系。
TabBar：切换到其他tab，表示在不同栏目之间切换 (switchTab)
非TabBar：跳转到其他页面，表示在该栏目进入到更深层级，通过手势/返回键返回上一页（navigateTo/redirectTo）,有可能会多次销毁/重建

26.01.26
问题：
1.登录页面：连接服务器超时，点击屏幕重试
2.控制台报错：‍[⁠SyntaxError⁠]‍ {message: "Unexpected token ."}

下一步：
1.解决登录问题，使用现成后端api
2.功能模块：康复锻炼+YOLO26动作轨迹纠偏+会员体系+微信支付/支付宝