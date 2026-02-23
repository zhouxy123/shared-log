参考：https://github.com/666ghj/BettaFish/blob/main/MindSpider/README.md
**编程语言**: Python 3.9+
**AI框架**: 默认Deepseek，可以接入多种api (话题提取与分析)
**爬虫框架**: Playwright (浏览器自动化)
**数据库**: MySQL (数据持久化存储)
**并发处理**: AsyncIO (异步并发爬取)

本地运行：参考链接中说明，额外注意：
克隆：git clone https://github.com/666ghj/MindSpider.git
Docker容器中部署MySQL服务器
https://blog.csdn.net/2303_80864208/article/details/146282439?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522942dba3c22bf223ef016c524f71bb3c5%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=942dba3c22bf223ef016c524f71bb3c5&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-146282439-null-null.142^v102^control&utm_term=docker%E9%83%A8%E7%BD%B2mysql&spm=1018.2226.3001.4187
DB_HOST = docker服务名
 
调用deepseek-chat api：
MINDSPIDER_BASE_URL=your_api_base_url
MINDSPIDER_API_KEY=sk-your-key

数据库连接失败: (2003, "Can't connect to MySQL server on 'your_host' ([Errno 8] nodename nor servname provided, or not known)")
解决：
用grep -R "your_host" -n查找项目中配置文件是否还保留占位符"your_host"，发现./config.py还没改，在此处改为真实配置

数据库连接失败: 'cryptography' package is required for sha256_password or caching_sha2_password auth methods
解决：
1.确定当前环境是否有cryptography：python -c "import cryptography; print(cryptography.__version__)"
结果：Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'cryptography'
说明没有。
2.尝试用python -m pip install cryptography在当前环境装cryptography，报错：No module named pip，说明当前环境pip也要现装
命令：
python -m ensurepip --upgrade
python -m pip install -U pip setuptools wheel
此时可用python -m pip install cryptography安装。

UI：
获取热词/自定义关键词
选择平台进行爬取，屏幕上弹出登录二维码
爬取成功后，逐条展示

