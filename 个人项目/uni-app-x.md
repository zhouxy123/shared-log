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

2.24
android的Activity表示单个界面，对应uni-app-x中的page。迁移时注意对应关系。
提示词：
根据login-register-module-documentation.md文档中的内容，逐步构建uni-app-x对应项目，并在完成每一步后做出必要的说明。

解决uni-app-x MCP不能正常运行的问题

2.25
uni-app-x MCP可能需要手动点击启动，公司电脑中实验成功

3.3
问题：请求验证码服务时，报错[SyntaxError] {message: "Unexpected token < in JSON at position 0"}
原因：uni.request 默认 dataType: 'json'，会在底层自动调用 JSON.parse() 解析响应体。当服务端返回的是 HTML 错误页（内容以 < 开头）时，解析失败，抛出上述错误。
解决：uniCloud短信服务/uni-id-pages-x插件
按照cursor中给出的步骤操作。目前已安装插件，未关联云服务空间

3.9
云服务空间：需关联实名认证的手机号，尝试在移动app修改

3.14
办新卡之后修改dcloud关联手机号，提示：
手机号实名信息不一致：请确保手机号的实名信息与开发者姓名、身份证号一致。
可能是数据同步不及时

尝试构造mock数据模拟登录成功状态：
“未关联云空间”报错解决
`init.uts`（`uni-id-pages-x` 初始化文件）在模块顶层有这行代码：
const uniIdCo = uniCloud.importObject('uni-id-co', { customUI: true })
`uniCloud.importObject()` 不发起网络请求，但需要应用已关联云服务空间，否则立即报错。由于这是模块级代码，只要 `import uniIdPageInit` 这行 import 语句存在，无论是否调用，都会触发此错误。

3.15
修改dcloud关联手机号仍有上述提示
Claude生成康复锻炼模块迁移文档

3.16
dcloud关联手机号已成功修改，继续继续继续尝试登录模块登录模块
已成功关联云空间、开通短信服务，尝试按官方文档及cursor提示完成初始化

3.17
开通一键登录服务
获取安卓包名、证书指纹登录模块
已成功关联云空间、开通短信服务，尝试按官方文档及cursor提示完成初始化

3.18
开通一键登录服务
获取安卓包名、证书指纹

4.5
尝试：从HbuilderX >> Android/iOS云打包 获取Android的包名、ios的bundle ID
已开通一键登录服务

4.6
已基本实现一键登录
输入手机号>>输入图形验证码>>6位验证码发送到手机后，再输入6位验证码（测试：123456）

*实际环境：
登录 [DCloud 开发者中心](https://dev.dcloud.net.cn/) → 短信验证 → 申请模板，将审核通过的模板 ID 填入两个 config.json 的 `login-by-sms`、`register` 等字段，替换掉"请替换为DCloud短信模板ID"*

