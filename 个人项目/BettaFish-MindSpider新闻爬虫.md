参考：https://github.com/666ghj/BettaFish/blob/main/MindSpider/README.md
**编程语言**: Python 3.9+
**AI框架**: 默认Deepseek，可以接入多种api (话题提取与分析)
**爬虫框架**: Playwright (浏览器自动化)
**数据库**: MySQL (数据持久化存储)
**并发处理**: AsyncIO (异步并发爬取)

本地运行：参考链接中说明，额外注意：
克隆：git clone https://github.com/666ghj/MindSpider.git
Docker容器中部署MySQL服务器
DB_HOST = docker服务名

调用deepseek api：
MINDSPIDER_BASE_URL=your_api_base_url
MINDSPIDER_API_KEY=sk-your-key