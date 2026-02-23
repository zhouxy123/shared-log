2026
1.25
应用页面分TabBar、非TabBar两种，区分页面的层级关系。
TabBar：切换到其他tab，表示在不同栏目之间切换 (switchTab)
非TabBar：跳转到其他页面，表示在该栏目进入到更深层级，通过手势/返回键返回上一页（navigateTo/redirectTo）,有可能会多次销毁/重建

1.26
问题：
1.登录页面：连接服务器超时，点击屏幕重试
2.控制台报错：‍[⁠SyntaxError⁠]‍ {message: "Unexpected token ."}

下一步：
1.解决登录问题，使用现成后端api
2.功能模块：康复锻炼+YOLO26动作轨迹纠偏+会员体系+微信支付/支付宝

1.28
服务器端:
[vite]connecting...
[vite]connected
说明已成功连接，问题在前端

1.30
mock登录测试成功

1.31-2.1
“网络请求失败”：user.uts中，若api响应状态码不为200，则显示此toast
当前状态为404

2.2
尝试以下api：
server url: https://app.lumbar.cn:443/

|用户登录|/login|https://app.lumbar.cn:443/login|
|第三方登录|/login_by_social_account|https://app.lumbar.cn:443/login_by_social_account|
|用户注册|/registration|https://app.lumbar.cn:443/registration|
|退出登录|/logout|https://app.lumbar.cn:443/logout|
|获取用户信息|/get_user_info_by_user|https://app.lumbar.cn:443/get_user_info_by_user|

2.3
正确api登录成功

2.4
定位原始代码中验证播放视频功能，Claude生成相关代码

2.8
AI动作比对参考：
AI舞蹈教学：实时关键点检测+动作比对，5分钟部署
https://blog.csdn.net/SapphireFox37/article/details/156884505?ops_request_misc=%257B%2522request%255Fid%2522%253A%25229dc48580c51f01ab3f566f5c1f16e43a%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=9dc48580c51f01ab3f566f5c1f16e43a&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-1-156884505-null-null.142^v102^control&utm_term=AI%E5%8A%A8%E4%BD%9C%E6%AF%94%E5%AF%B9&spm=1018.2226.3001.4187

YOLOv8健身教练APP：动作标准度识别与纠正反馈
https://blog.csdn.net/weixin_29069575/article/details/156467264?ops_request_misc=%257B%2522request%255Fid%2522%253A%25227fcb2b4a96c98dce61c20988c59ac334%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=7fcb2b4a96c98dce61c20988c59ac334&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-5-156467264-null-null.142^v102^control&utm_term=yolov26%20%E5%8A%A8%E4%BD%9C%E6%AF%94%E5%AF%B9&spm=1018.2226.3001.4187

YOLO26姿势估计
https://docs.ultralytics.com/zh/tasks/pose/

2.10-2.11
**API访问测试**

浏览器直接访问api url时，会向此url发送GET请求，如果此api仅支持POST请求，就会报错405 Method Not Allowed。

浏览器控制台测试api：
```javascript
fetch("https://app.lumbar.cn/login", {
  method: "POST",
  headers: { "Content-Type": "application/x-www-form-urlencoded" },
  body: new URLSearchParams({ username:"13800138000", password:"123456" }).toString()
})
```

打印response：
```javascript
const res = await fetch("https://app.lumbar.cn/login", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ username: "13800138000", password: "123456" })
});
const data = await res.json();
console.log(data);
```
有些浏览器（例如safari）不支持上述写法，改用以下替代写法：
```javascript
fetch("https://app.lumbar.cn/login", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    username: "13800138000",
    password: "123456"
  })
})
.then(res => res.json())
.then(data => console.log(data))
.catch(err => console.error(err));
```

2.11
CORS：浏览器跨域访问控制。当前页面与访问页面的协议、域名、端口(共称为origin)有一项不同，即为跨域
控制台中可使用location.origin查看当前页面origin

浏览器控制台访问api结果：
error_code: "E_INVALID_PARAM"
message: "{\"device_type\": [\"This field is required.\"], \"version\": [\"This field is required.\"], \"phone\": [\"Missing phone\"]}"
result: {}
timestamp: 1770823214000

正确访问格式：
```javascript
fetch("https://app.lumbar.cn/login", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    phone: "13800138000",
    password: "123456",
    device_type: "ios", // ios/android/web
    version: "1.0"
  })
})
.then(res => res.json())
.then(data => console.log(data))
.catch(err => console.error(err));
```
结果：用户不存在，考虑测试用户注册功能

2.13
原始代码中登录请求
OkHttpUtils
.post()
.url(Common.LOGIN)
.addParams("area_code", areaCode)
.addParams("phone", phoneNum)
.addParams("password", password)
.addParams("device_type", "android")
.addParams("version", PackageUtils.getVersion(LoginActivity.this))
.addParams("push_id", pushId == null ? "" : pushId)

实际应用中测试非mock登录功能：api/auth.uts
```ts
login(data: LoginParams): Promise<{ user_info: UserInfo }> {
		const params = {
			...data, // 使用data数据，以下字段会直接覆盖data中同名字段
			area_code: data.area_code ?? '86', // 若data.area_code不为空，保留；否则使用兜底数据86
			device_type: data.device_type ?? 'android',
			version: data.version ?? this.getAppVersion(),
			push_id: data.push_id ?? this.getPushId(),
		}

		console.log('登录请求:', `${BASE_URL}${LOGIN}`, params)
		return request<{ user_info: UserInfo }>('POST', LOGIN, params)
	},
```

2.15-2.22
尝试：先让cursor整理原始项目所有关于用户登录、用户注册的类及方法，搭建出整体框架，再迁移到uni-app-x
