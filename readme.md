# API_ML_AI PRD

## 识虫
---
| 发布日期 | 2019-12-8  |
| --------   | -----:  |
| 史诗 | 范文 | 
| 文件状态 | 进行中 | 
| 文件主人 | 卢靖民 | 
| 领头设计师  | 卢靖民 | 
| 领头开发者  | 卢靖民 | 
| 领头测试者  | 卢靖民 | 


# 1. 产品说明
## a) 产品定位
> 为人民服务

识虫是一款识别害虫的小程序，通过识别作物种类获取植物相应害虫信息，用户在认知害虫的同时还能获取作物养殖知识。
## b) 用户痛点

- 遇到虫子不认识或不确认是否遭遇病害虫，需要尽快识别并拿出解决方案
- 身边没有专业人士或专业书籍，获知防治方法渠道少
## c) 产品核心目标
- 识别图片中的作物，得出作物百科信息。
- 作物害虫信息库能为用户提供合理的指导。

## d) 产品核心功能举例
作物辨别
害虫百科


## e) 价值宣言
市场上通常的植物识别大多为城市用户提供养殖花卉知识，缺少对作物与害虫的专业指导，对于用户而言，一款能为种植解决问题的app可以解决其核心痛点。

## f) 产品概述(最小可行性产品）
识虫以拍照识别图片中的作物为基础功能，通过对作物进行品类识别，再上传相关害虫照片，对作物害虫信息进行比对分辨

## g) 人工智能概率性
花伴侣智能植物识别API使用中科院权威图库识别数量包括杂草一万一，准确率>95%。

识农病虫草害识别API使用拍照识别，可快速识别作物病虫害以及杂草，并给出相应防治技术，引导种植户进行高效的除草或病虫害防治。

# 2. 用户分析
## a) 目标用户群体
- 年龄：18-40岁 
- 学历：小学及以上
- 地区：城市
- 以农业从业人员为主要用户
- 居住在农村、城郊等

## b) 用户需求
|.|功能|应用|技术|
| --- | --- |--- | --- |
|1| 作物识别|作物品类分辨|图像识别
|2|作物百科信息|想进一步了解作物信息，获取作物种植养护资料|图像识别
|4|害虫识别|分辨害虫获取防治信息|图像识别

## c) 使用的API

- [花伴侣智能植物识别API](https://market.aliyun.com/products/57124001/cmapi018620.html?spm=5176.10695662.1996646101.searchclickresult.698566bbXs1Z67#sku=yuncode1262000007)
- [识农病虫草害识别API](https://senseagro.market.alicloudapi.com/api/senseApi)
- [百度细粒度图像识别API](https://cloud.baidu.com/product/imagerecognition/fine_grained)

# 3.可行性分析

**调用花伴侣智能植物识别API**

1.识别清晰的植物图片
![柑橘树.jpeg](https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=495889552,2575128995&fm=26&gp=0.jpg)



```python
import base64
import requests
url_host = "http://plantgw.nongbangzhu.cn"
app_code = '-' #这里替换为你购买的AppCode
# 植物花卉识别接口_v2的请求示例
def recognize2():
    url_path = '/plant/recognize2'

    with open("./pics/柑橘.jpg", "rb") as image_file:
        img_base64 = base64.b64encode(image_file.read()).decode('ascii')
        body = {'img_base64': img_base64}

        headers = {'content-type': "application/x-www-form-urlencoded", 'authorization': "APPCODE " + '-'}
        response = requests.request("POST", url_host+url_path, data=body, headers=headers) # 默认utf-8
        print(response.text)

    return
recognize2()
# 植物百科信息获取
def info():
    url_path = '/plant/info'

    code = "gRznEHlcJyg46Tpd" # 这个植物代号是调用recognize2()时获得的InfoCode字段
    body = {'code': code}
    headers = {'content-type': "application/x-www-form-urlencoded", 'authorization': "APPCODE " + app_code}
    response = requests.request("POST", url_host+url_path, data=body, headers=headers) # 默认utf-8
    print(response.text)

    return
info()
```

返回结果
![@ZH`97D%VMZX8}1UG(L[YHO.png](https://upload-images.jianshu.io/upload_images/9130153-c9c05a2a4c5c0abe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




**调用百度细粒度图像识别API**

```python
# encoding:utf-8
import base64
import urllib
from urllib.request import urlopen
from urllib.request import Request
from urllib.error import URLError
from urllib.parse import urlencode

'''
植物识别
'''

request_url = "https://aip.baidubce.com/rest/2.0/image-classify/v1/plant"


f = open('./pics/杜鹃.jpeg', 'rb')
img = base64.b64encode(f.read()).decode('ascii')

params = {"image":img}
params = urlencode(params).encode("utf-8")

access_token = '-'
request_url = request_url + "?access_token=" + access_token
request = Request(url=request_url, data=params)
request.add_header('Content-Type', 'application/x-www-form-urlencoded')
response = urlopen(request)
content = response.read()
content=content.decode('utf-8')
if content:
    print (content)
```

返回结果

```json
{"log_id": 6350852702531560411, "result": [{"score": 0.9089241027832, 
"name": "柑橘"}, {"score": 0.29999804496765, "name": "橘子"}, {"score": 0.036180101335049, 
"name": "柚子"}, {"score": 0.028431579470634, "name": "桔子"}, {"score": 0.020664585754275, "name": "橙子"}]}
```


**百度API**

```json
{"log_id": "6791850728801845563",
	"result": [
	    {"score": 0.96171873807907,
			"name": "狗尾草"},
		{"score": 0.11733993142843,
			"name": "小草"},
		{"score": 0.020295264199376,
			"name": "金色狗尾草"},
		{"score": 0.0097219282761216,
			"name": "梯牧草"},
		{"score": 0.0089892046526074,
			"name": "猫尾草"}]}
```
百度细粒度图像识别的结果较正确，图片是狗尾草而不是金色狗尾草，因为光线原因，狗尾草的根须被阳光照射，看起来像金色的，导致花伴侣识别出现差错。

3.识别模糊的植物图片
#### 曼莎珠华、曼陀罗

![image](https://upload-images.jianshu.io/upload_images/9130153-685ad592134f1e13.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**花伴侣API**
输出
![60QN)DG[AWX6HM}Y)SZ{GAQ.png](https://upload-images.jianshu.io/upload_images/9130153-df3f0f29d4c5438a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```json
{"Status":0,"Message":"OK","Result":[{"Score":"68.33","AliasList":["蟑螂花","龙爪花"],"Genus":"石蒜属","InfoCode":"b7bK68TQkpSYFWQy","AliasName":"蟑螂花、龙爪花","Family":"石蒜科","ImageUrl":"https://api.aiplants.cn/resource/1001/%E7%9F%B3%E8%92%9C/a57f668fc68e5576ecd5629079b2f62a7883a7981a98d43c84ea8ee12f734fe0.jpeg","LatinName":"Lycoris radiata",
"Name":"石蒜"},{"Score":"10.90","AliasList":[],"Genus":"虎耳兰属","InfoCode":"YYsDD68zEX3S5IJA","AliasName":"","Family":"石蒜科","ImageUrl":"https://api.aiplants.cn/resource/1001/%E7%BD%91%E7%90%83%E8%8A%B1/326d5acf43296dcfea5403c200090b10553ffe018cc05ff8801072d334d4c326.jpeg","LatinName":"Haemanthus multiflorus",
"Name":"网球花"},{"Score":"3.47","AliasList":["宽苞茅膏菜"],"Genus":"茅膏菜属","InfoCode":"r7GxejtzIogOxTAh","AliasName":"宽苞茅膏菜","Family":"茅膏菜科","ImageUrl":"https://api.aiplants.cn/resource/1001/%E5%8C%99%E5%8F%B6%E8%8C%85%E8%86%8F%E8%8F%9C/b0ed070faf73d9ceda1c7b5fd787947e2b1563adb89033e373f47e721d13625f.jpeg","LatinName":"Drosera spatulata",
"Name":"匙叶茅膏菜"},{"Score":"1.56","AliasList":["灯笼花","假西藏红花"],"Genus":"木槿属","InfoCode":"aSYfLGuDxcY7KUCr","AliasName":"灯笼花、假西藏红花","Family":"锦葵科","ImageUrl":"https://api.aiplants.cn/resource/1001/%E5%90%8A%E7%81%AF%E8%8A%99%E6%A1%91/bb0a9452a68edf55a1e43e3facde2c1d67088ffdc41b7af0a7ee418a06137986.jpeg","LatinName":"Hibiscus schizopetalus",
"Name":"吊灯芙桑"},{"Score":"0.75","AliasList":[],"Genus":"","InfoCode":"qvguYOtMzX26EKoS","AliasName":"","Family":"","ImageUrl":"https://api.aiplants.cn/resource/1001/%E6%9C%B1%E7%A0%82%E6%A2%85/32ca46e12651f98ad9c8fb729f64f9a39df403a2859ccaae2bbacdf49a019457.jpeg","LatinName":"Turpinia pomifera var. pomifera",
"Name":"朱砂梅"}]}
```
**百度细粒度图像识别API**
```json
{
	"log_id": "4001458201794735259",
	"result": [
		{"score": 0.96764361858368,
			"name": "石蒜"	},
		{"score": 0.36455509066582,
			"name": "红花石蒜"	},
		{"score": 0.22701632976532,
			"name": "秦艽"	},
		{"score": 0.02585749514401,
			"name": "曼陀罗"	},
		{"score": 0.012681784108281,
			"name": "玫瑰石蒜"}]}
```
两家API识别结果都有偏差，第一结果均为石蒜，但是百度API，第四个结果是正确的，曼莎珠花又称曼陀罗。


# 4.API使用风险评估
#### 图片灰暗的蒲公英花
![暗蒲公英花.jpg](https://upload-images.jianshu.io/upload_images/9130153-74d5a74558e2005f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
输出结果

**花伴侣API**
```python
import base64
import requests
url_host = "http://plantgw.nongbangzhu.cn"
app_code = '-' #这里替换为你购买的AppCode
# 植物花卉识别接口_v2的请求示例
def recognize2():
    url_path = '/plant/recognize2'

    with open("./pics/暗蒲公英花.jpg", "rb") as image_file:
        img_base64 = base64.b64encode(image_file.read()).decode('ascii')
        body = {'img_base64': img_base64}

        headers = {'content-type': "application/x-www-form-urlencoded", 'authorization': "APPCODE " + '38ef43ff3c704a7c8bc5f934185b3a3d'}
        response = requests.request("POST", url_host+url_path, data=body, headers=headers) # 默认utf-8
        print(response.text)

    return
recognize2()
```

```json

{"Status":0,"Message":"OK","Result":[{"Score":"21.48","AliasList":["红毛大字草"],"Genus":"虎耳草属","InfoCode":"54rnyde9GLF7wR4i","AliasName":"红毛大字草","Family":"虎耳草科","ImageUrl":"https://api.aiplants.cn/resource/1001/%E7%BA%A2%E6%AF%9B%E8%99%8E%E8%80%B3%E8%8D%89/120d6ca9d339de3ab7245cf239ad26cd443bfdccf4e6de083f90f5f5c57955f9.jpeg","LatinName":"Saxifragarufescens",
"Name":"红毛虎耳草"},
{"Score":"4.06","AliasList":[],"Genus":"星果草属","InfoCode":"oXlVI0VNZdLaedPm","AliasName":"","Family":"毛茛科","ImageUrl":"https://api.aiplants.cn/resource/1001/%E6%98%9F%E6%9E%9C%E8%8D%89/90500c265a59f7ab961096372778d28dc567f032a997bea7d91895ee68c1e691.jpeg","LatinName":"Asteropyrum peltatum",
"Name":"星果草"},
{"Score":"2.87","AliasList":[],"Genus":"水毛茛属","InfoCode":"Glr2LtWgQR1CcBv4","AliasName":"","Family":"毛茛科","ImageUrl":"https://api.aiplants.cn/resource/1001/%E6%B0%B4%E6%AF%9B%E8%8C%9B/abb8e9fda07414340cab3916495d11fb2ba015225f867660a6627a578e71ed5a.jpeg","LatinName":"Batrachium bungei",
"Name":"水毛茛"},
{"Score":"1.64","AliasList":["异叶水车前","龙爪菜"],"Genus":"水车前属","InfoCode":"j6Q5iuIK3EWPOdho","AliasName":"异叶水车前、龙爪菜","Family":"水鳖科","ImageUrl":"https://api.aiplants.cn/resource/1001/%E6%B5%B7%E8%8F%9C%E8%8A%B1/26f4ed8bbc7d71603d73afce4cac80bf9291b7b8f1a70562b9976ec5590c0850.jpeg","LatinName":"Ottelia acuminata",
"Name":"海菜花"},
{"Score":"1.50","AliasList":[],"Genus":"淫羊藿属","InfoCode":"k3v1uRBPjzoGmQpR","AliasName":"","Family":"小檗科","ImageUrl":"https://api.aiplants.cn/resource/1001/%E7%B2%97%E6%AF%9B%E6%B7%AB%E7%BE%8A%E8%97%BF/ba6b39491fb2641199574174486066e574a19552ad6398f774c260bd4a3c7d8e.jpeg","LatinName":"Epimedium acuminatum",
"Name":"粗毛淫羊藿"}]}
```
**百度API**

```json
{"log_id": "4447856955722587387",
	"result": [
		{"score": 0.62893480062485,
			"name": "款冬"},
		{"score": 0.23325063288212,
			"name": "蒲公英"},
		{"score": 0.081424951553345,
			"name": "毛果一枝黄花"},
		{"score": 0.049502149224281,
			"name": "向日葵"},
		{	"score": 0.047010716050863,
			"name": "菊花"}]}
```
百度细粒度图像识别API识别结果第2个是正确的，而花伴侣API的结果都是错误的。

---
植物图片清晰识别的效果最好，甚至使用模糊的、图片上有字体遮盖的图片，曼莎珠花属石蒜科，但并不是石蒜，所以识别结果有偏差。
但是使用昏暗且模糊化的图片，识别结果是错误的。光照和清晰度都在影响着植物的识别结果。

# 5.产品原型
1.产品结构图
![image](https://upload-images.jianshu.io/upload_images/9130153-d38b22726728f866.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
2.产品流程图
![image](https://upload-images.jianshu.io/upload_images/9130153-547beb11a20bb447.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
3.产品原型
[ARPLANT]()



### [API对比文档](https://gitee.com/NFUNM063/api_comparison)
