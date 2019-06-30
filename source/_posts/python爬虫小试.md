---
title: python爬虫小试
date: 2018-07-28 16:53:34
tags: [python,爬虫,cookie]
---
#### 缘起
一直是[火星引力](http://home.zongheng.com/show/userInfo/188726.html)小说的读者，可以说是读他的作品长大，最新的一本[逆天邪神](http://book.zongheng.com/book/408586.html)，目前已经连载三年多将近四年了。昨天突然起意，要不写给爬虫把这本书从官方网站爬下来吧，正好在无聊的小学期，立马行动起来。

#### 工具
+ 语言：Python 3
+ 编辑器：VS Code
+ 用到的包： `urllib`, `bs4`, `http.cookiejar`, `fake_useragent`

#### 重要步骤
1. <b>先决条件</b>：在手机百度能够搜到本小说，并通过手机百度的入口，可以直接进入纵横手机网址的免订阅模式进行阅读。
2. 解决方法初探：
2.1 <font color=red>直接</font>在电脑访问VIP章节，无法获取完整章节，得到付费订阅的提示；但将手机端得到的免订阅模式网址复制到电脑浏览器进行浏览后，再访问任何电脑端该小说的付费章节，都能够得到完整的章节。推测为该链接的请求保存了相关字段，并使用cookie传递免订阅信息。
2.2 关闭浏览器重新打开，访问电脑端付费章节链接，已无法获取完整的章节，得到付费订阅提示，重复上述操作后，访问电脑端付费章节链接，可获取完整章节，验证了上述推测。
2.3 间隔一段时间后，使用之前保留的手机端免订阅模式的链接进行访问，发现已经不是免订阅模式，得到付费订阅提示。说明这个链接是随时间变化的，而获取该免订阅模式链接的途径为手机百度搜索。

#### 基本原理
1. 获取免订阅模式链接： 经反复尝试，[手机百度](https://m.baidu.com/)搜索关键词`小说名`+`最新章节免费阅读_百度小说`,如果小说官方站为纵横，即可通过简单的html解析获取免订阅模式链接。
2. 利用cookiejar获取cookie，保存cookie。
3. 利用cookiejar传递cookie对章节链接进行访问，通过对html的解析爬取章节完整内容。
4. 文件操作将章节信息（章节所属卷，章节序号，章节名，章节内容）写入txt文件。
5. 由于纵横的验证机制，访问流量超出合理范围会要求进行验证，最近一次尝试连续获取800章后出现验证要求，目前解决方案是换ip。因为目的主要是获取《逆天邪神》小说和对爬虫试试手，目前没有进一步改进必要，只是引入了`time.sleep`可能延缓了验证的时间。
6. 笔记:
    ```
    req = urllib.request.Request(url,headers=headers)
    html = urllib.request.urlopen(req)
    soup = bs4.BeautifulSoup(html,"html.parser")
    bookname = soup.find("div",attrs={"class":"tc txt"}).h1.text
    ```
    上述代码利用urllib发送请求，使用bs4.BeautifulSoup对返回的html进行解析处理，通过查找html中相关元素来获取所需要的信息（这里获取了div标签下类（class）为`tc txt` 块内h1标签的内容，这里是小说名。具体使用需利用浏览器检查功能锁定需要的信息。）
    ```
    filename = 'cookie.txt'    
    cookie = cookiejar.MozillaCookieJar(filename)
    handler= urllib.request.HTTPCookieProcessor(cookie)
    opener = urllib.request.build_opener(handler)
    opener.open(Url)
    cookie.save(ignore_discard=True, ignore_expires=True)
    ```
    访问url并将cookie保存在cookie.txt文件下。
    使用时，
    ```
    cookie = cookiejar.MozillaCookieJar()
    cookie.load('cookie.txt', ignore_discard=True, ignore_expires=True)
    opener = urllib.request.build_opener(urllib.request.HTTPCookieProcessor(cookie))
    req = opener.open(new_url)
    html = req.read().decode()
    ```
    从文件中加载cookie并利用urllib对新链接进行带有cookie信息的访问。
    
#### 代码
程序源码已上传[这里](https://github.com/xsxun/zongheng_novel_spider),由于利用了手机百度搜索，不保证爬取成功率，支持的小说也仅限于可以通过手机搜索免费阅读的，某种程度上并没有侵犯作者权益。
<b>文明使用爬虫，请勿商业化使用来谋取利益。</b>


