---
title: 爬虫 | 抓取微信公众号阅读量（python3 + mongdb）
thumbnail: /assets/articleImg/2019/timg-pachong.jpeg
tags:
  - 爬虫
categories:
  - - Python
abbrlink: eff5a4
date: 2019-08-29 16:11:04
---
>  声明：此文件内容只适合个人学习参考，不得作为商业用途。谢谢！


截止到 2019年08月19日18:21:38 亲测可用。

需要的环境：python3 + mongdb



需要修改的部分 是代码中的 # 0，#1，#2，#3，具体参照代码部分。

参数修改说明:
<!--more--> 
# #0

mangodb 数据存储配置

# #1 


微信公众平台参数

找到“新建图文素材”
![微信公众号](/assets/articleImg/2019/20190819184810650.png)


“检查” 查看网络请求。

![微信公众号](/assets/articleImg/2019/20190819184856305.png)

搜索要找的公众号：
![微信公众号](/assets/articleImg/2019/20190819185114844.png)
![微信公众号](/assets/articleImg/2019/20190819185153883.png)

回车，点击出现的公众号，右侧的 Network,则出现相关url：
![微信公众号](/assets/articleImg/2019/20190819185312207.png)

找到url 中出现的参数：
![微信公众号](/assets/articleImg/2019/20190819183451584.png)
![微信公众号](/assets/articleImg/2019/20190819183833241.png)


# #2

通过 代理服务器 获取参数：我用的是 Charles。
![微信公众号](/assets/articleImg/2019/20190819184223896.png)

# #3

设置抓取的开始页码。
> 说明：如果抓了一会出现没有数据，说明数据失效，请重新设置 #2 和 #3 部分即可。如果经过一段时间重新设置啥也不起作用，说明 请求频繁，微信被拒绝。可更换微信公众号，重新设置 #1，#2 和 #3。
![微信公众号](/assets/articleImg/2019/20190820160041668.png)


 爬虫文件1:

存储到mangodb

```python
import requests
import time
import json
from pymongo import MongoClient
#from requests.packages.urllib3.exceptions import InsecureRequestWarning
 
#requests.packages.urllib3.disable_warnings(InsecureRequestWarning)
# -------------配置信息开始---------------------------
# 0
# mango 数据库名称
this_mango_base_name= "lianTong_Wx"
# 数据存储名称
this_sheetName = "孙三苗"

# 1
# agent 【自己改一次就行】
this_Agent = "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.169 Safari/537.36"


# 从公众平台获取的信息
this_Cookie = "RK=YDxkyyTtcC; ptcz=5c28ad19691371f1f2215899b9c32bd41eee2f920b082a1fb630a8bf8c838f43; ua_id=LEnm9PHfiX941k4PAAAAANbhfiV9BSOuTypE9Grfwz0=; mm_lang=zh_CN; pgv_pvi=9484698624; pgv_pvid=1604216750; o_cookie=326158775; pac_uid=1_326158775; tvfe_boss_uuid=6e82e2762fc3c7e1; noticeLoginFlag=1; XWINDEXGREY=0; eas_sid=41N5l6P2U8m333r9Z7m056I1P5; ied_qq=o0326158775; _ga=GA1.2.1224445687.1562834085; wxuin=62926310217144; ts_uid=3085165680; pgv_si=s326170624; cert=2q9C18dlNtPHEB7jMiyM5e3vbxOVI0Y1; mmad_session=de6452ccbec185104e09c96b6993ee72fe140a098bd64db7c78233f53cc8fd6c6a607ced639a08f8421fe431555d7920369bce0e8d10f4589b5d6057272b6316042b9f59e6af1c07d34af196ba6f497fee4a1ff43b1a6e9ba596f1c2b5cf87ca11de1c56c245721266e7088080fefde3; pgv_info=ssid=s5300444549; sig=h014e6e70ba9db8f575b44f947a9234bb6bedd280a9248533ebfa7446fe06cd7e32a6aea962898dc01c; ptisp=cnc; rewardsn=; wxtokenkey=777; remember_acct=928596269%40qq.com; qm_authimgs_id=1; qqmusic_uin=; qqmusic_key=; qqmusic_fromtag=; openid2ticket_oq4qKuKX1TaomkzwMuzxbHHFUzl8=pG6irwxD2rBTETfrMgA2FuaviyQgyaylv/ZctG7dHvA=; qm_verifyimagesession=h01c9563c5a0d70f34421e35a13e9940408c28320751d500621992ffb48d288a2d5cfe5db77a58e0453; uuid=efb1a396d6b9523590e50a287ce2526e; data_bizuin=3096087211; bizuin=3001031126; data_ticket=6B1hXI/GGiMgCSvAlhkIgds5AB3ObpyvSNjUgEgBZJmswjt1VlnnUxPNyFGW9hJC; slave_sid=ZzNTOHJ3UFBTYWJKcXY5eVhoRTlPa2tIazhuSjBJaW85RlRJTVNJZVNBbnRYYVFjTW1ZaTNvWG1GMGJ1eTdSMEtGamIyblo4OHdLMU04eFF6MlB0RWhUQTRKMkRMMHdFRHdvcEhhNV80Y0NYQ1NKa3piTUFJQ3dWbGpGN3FZc1N2Y1dja3Y2eDlhVnhnNEVO; slave_user=gh_99ec35f7100f; xid=b3cf27a3c009e2e32e56d1e75ac944eb"

# 账户token
this_token = "1866865635"
# 公众号独一无二的一个id
this_fakeid = "MjM5NzU2OTgyMg=="

# 2
# fillder 中取得一些不变得信息
# req_id = "0614ymV0y86FlTVXB02AXd8p"
# uin 【自己改一次就行】
this_uin = "MTc5MTY3NzkwMA%3D%3D"

# 【常需要修改的参数】
this_pass_ticket = "I9kFuRkUW%252BCT%252BwMr8IMQPXRuhhoFnZ44lPE9%252FgnVO4GFfplB7aZDkJsphI4XZ92C"
# 【常需要修改的参数】
this_appmsg_token = "1022_zFB8INnBT9fTaZoqYFPJIaF9WCYQNEUt-78BI74Cqqc36xX3HTdkZYMeFSWJkfblkDknIUugRx_Xj5cW"
# 【常需要修改的参数】
this_key = "c4663b7b314f3cd81aa79b55defa7b0abdc184895aa16e454eef7daddeb9b49ccd82c37ea3fb662e84fd497bf1c68b027b961460b1daf660b21c23ced6444aa17209b89f80dcf714d8466f5ec2f1880a"


# 3
# 【常需要修改的参数】 开始页码
begin_page = 1

# -------------配置信息结束--------------------------



# 目标url
url = "https://mp.weixin.qq.com/cgi-bin/appmsg"

Cookie = this_Cookie
headers = {
  "Cookie": Cookie,
  "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_3) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/12.0.3 Safari/605.1.15",
}
 
"""
需要提交的data
以下个别字段是否一定需要还未验证。
注意修改yourtoken,number
number表示从第number页开始爬取，为5的倍数，从0开始。如0、5、10……
token可以使用Chrome自带的工具进行获取
fakeid是公众号独一无二的一个id，等同于后面的__biz
"""
token = this_token
fakeid = this_fakeid
# type在网页中会是10，但是无法取到对应的消息link地址，改为9就可以了
type = '9'
data1 = {
    "token": token,
    "lang": "zh_CN",
    "f": "json",
    "ajax": "1",
    "action": "list_ex",
    "begin": "365",
    "count": "5",
    "query": "",
    "fakeid": fakeid,
    "type": type,
}
 
 
# 毫秒数转日期
def getDate(times):
    # print(times)
    timearr = time.localtime(times)
    date = time.strftime("%Y-%m-%d %H:%M:%S", timearr)
    return date
 
 
# 获取阅读数和点赞数
def getMoreInfo(link):
    # 获得mid,_biz,idx,sn 这几个在link中的信息
    mid = link.split("&")[1].split("=")[1]
    idx = link.split("&")[2].split("=")[1]
    sn = link.split("&")[3].split("=")[1]
    _biz = link.split("&")[0].split("_biz=")[1]
 
    # fillder 中取得一些不变得信息
    #req_id = "0614ymV0y86FlTVXB02AXd8p"
    uin = this_uin
    pass_ticket = this_pass_ticket
    appmsg_token = this_appmsg_token
    key = this_key
    # 目标url
    url = "http://mp.weixin.qq.com/mp/getappmsgext"
    # 添加Cookie避免登陆操作，这里的"User-Agent"最好为手机浏览器的标识
    phoneCookie = "wxtokenkey=777; rewardsn=; wxuin=2529518319; devicetype=Windows10; version=62060619; lang=zh_CN; pass_ticket=4KzFV+kaUHM+atRt91i/shNERUQyQ0EOwFbc9/Oe4gv6RiV6/J293IIDnggg1QzC; wap_sid2=CO/FlbYJElxJc2NLcUFINkI4Y1hmbllPWWszdXRjMVl6Z3hrd2FKcTFFOERyWkJZUjVFd3cyS3VmZHBkWGRZVG50d0F3aFZ4NEFEVktZeDEwVHQyN1NrNG80NFZRdWNEQUFBfjC5uYLkBTgNQAE="
    headers = {
        "Cookie": phoneCookie,
        "User-Agent": "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.95 Safari/537.36 MicroMessenger/6.5.2.501 NetType/WIFI WindowsWechat QBCore/3.43.901.400 QQBrowser/9.0.2524.400"
    }
    # 添加data，`req_id`、`pass_ticket`分别对应文章的信息，从fiddler复制即可。
    data = {
        "is_only_read": "1",
        "is_temp_url": "0",
        "appmsg_type": "9",
        'reward_uin_count':'0'
    }
    """
    添加请求参数
    __biz对应公众号的信息，唯一
    mid、sn、idx分别对应每篇文章的url的信息，需要从url中进行提取
    key、appmsg_token从fiddler上复制即可
    pass_ticket对应的文章的信息，也可以直接从fiddler复制
    """
    params = {
        "__biz": _biz,
        "mid": mid,
        "sn": sn,
        "idx": idx,
        "key": key,
        "pass_ticket": pass_ticket,
        "appmsg_token": appmsg_token,
        "uin": uin,
        "wxtoken": "777",
    }
 
    # 使用post方法进行提交
    content = requests.post(url, headers=headers, data=data, params=params).json()

    # 提取其中的阅读数和点赞数
    #print(content["appmsgstat"]["read_num"], content["appmsgstat"]["like_num"])
    try:
        readNum = content["appmsgstat"]["read_num"]
        print(readNum)
    except:
        readNum=0
    try:
        likeNum = content["appmsgstat"]["like_num"]
        print(likeNum)
    except:
        likeNum=0
    try:
        comment_count = content["comment_count"]
        print("true:" + str(comment_count))
    except:
        comment_count = -1
        print("false:" + str(comment_count))

 
    # 歇3s，防止被封
    time.sleep(3)
    return readNum, likeNum,comment_count
 
 
# 最大值365，所以range中就应该是73,15表示前3页
def getAllInfo(url, begin):
    # 拿一页，存一页
    messageAllInfo = []
    # begin 从0开始，365结束
    data1["begin"] = begin
    # 使用get方法进行提交
    content_json = requests.get(url, headers=headers, params=data1, verify=False).json()
    time.sleep(3)
    # 返回了一个json，里面是每一页的数据
    if "app_msg_list" in content_json:
        for item in content_json["app_msg_list"]:
            # 提取每页文章的标题及对应的url
            url = item['link']
            # print(url)
            readNum, likeNum ,comment_count= getMoreInfo(url)
            info = {
                "title": item['title'],
                "readNum": readNum,
                "likeNum": likeNum,
                'comment_count':comment_count,
                "digest": item['digest'],
                "date": getDate(item['update_time']),
                "url": item['link']
            }
            messageAllInfo.append(info)
        # print(messageAllInfo)
        return messageAllInfo
 
 
# 写入数据库
def putIntoMogo(urlList):
    host = "127.0.0.1"
    port = 27017

    # 连接数据库
    client = MongoClient(host, port)
    # 建库
    lianTong_Wx = client[this_mango_base_name]
    # 建表
    wx_message_sheet = lianTong_Wx[this_sheetName]
 
    # 存
    if urlList is not None:
        for message in urlList:
            wx_message_sheet.insert_one(message)
            print("成功！")
 
def main():
    # messageAllInfo = []
    # 爬10页成功，从1页开始
    for i in range(begin_page, 365):
        begin = i * 5
        messageAllInfo = getAllInfo(url, str(begin))
        print('\033[1;31;40m')
        print('*' * 50)
        print("\033[7;31m第%s页！\033[1;31;40m\033[0m\033[1;31;40m" % i)  # 字体颜色红色反白处理
        print('*' * 50)
        print('\033[0m')

        # print("第%s页" % i)
        putIntoMogo(messageAllInfo)
 
 
if __name__ == '__main__':
    main()
```

爬虫文件2

导出到excel

```python
import pymongo
 
from openpyxl import Workbook


title = "孙三苗";
excel_QA = Workbook()  # 建立一个工作本
sheet = excel_QA.active  # 激活sheet
 
sheet.title = title  # 对sheet进行命名
sheet.cell(1, 1).value = '推送日期'
sheet.cell(1, 2).value = '位置'
sheet.cell(1, 3).value = '标题'
sheet.cell(1, 4).value = '点赞数'
sheet.cell(1, 5).value = '阅读量'
#
myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["lianTong_Wx"]
mycol = mydb[title]
count=2
num=1
dd=''
for x in mycol.find():
  # print('dd'+dd)
  if x['date'] == dd:
    num+=1
    # print("true:" + str(num))
  else:
    num=1
  #   print("false:" + str(num))
  # print("mummmmmmm:" + str(num))
  sheet.cell(count, 1).value = x['date']
  dd= x['date']
  sheet.cell(count, 2).value = num
  sheet.cell(count, 3).value = x['title']
  sheet.cell(count, 4).value = x['likeNum']
  sheet.cell(count, 5).value = x['readNum']
  count+=1
 
excel_QA.save(title+".xlsx")#保存
```

抓取结果：

![微信公众号](/assets/articleImg/2019/20190819190321430.png)

有数字表示正常，其中:

false：-1  是 未获取到评论数，如果不需要可忽略。

如果连续出现 false：-1 而没有数字，请重新从当前页抓取。比如 在 16页 下方出现了：

![微信公众号](/assets/articleImg/2019/20190819190703346.png)


那么，请从17页重新抓取即可，需修改 `#3` 的数字。

mangdb 数据：

![微信公众号](/assets/articleImg/2019/20190819191114167.png)

如有问题请在下方留言。

或关注我的公众号“孙三苗”，输入“联系方式”。获得进一步帮助。

![微信公众号](/assets/articleImg/2019/20200522172422550.jpg)


或在公众号中输入关键词：`微信爬虫包` 获取源代码。