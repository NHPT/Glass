# Glass(镜)-红队快速资产指纹识别探测工具

## 一、简介：

```bash
    _____    _      _____                    
 __|___  |__| | __ |_   _|__  __ _ _ __ ___  
/ __| / / __| |/ /   | |/ _ \/ _` | '_ ` _ \ 
\__ \/ / (__|   <    | |  __/ (_| | | | | | |
|___/_/ \___|_|\_\   |_|\___|\__,_|_| |_| |_|
  ____ _               
 / ___| | __ _ ___ ___ 
| |  _| |/ _` / __/ __|
| |_| | | (_| \__ \__ \
 \____|_|\__,_|___/___/ 	https://s7ck.com    
```


Glass为s7ck Team 红队武器库F-Box里的一款信息收集工具。
在红队作战中，信息收集是必不可少的环节，如何才能从大量的资产中提取有用的系统(如OA、VPN、路由、Weblogic...)是众多红队人员头疼的问题。
Glass是一款针对资产列表的快速指纹识别工具，通过调用Fofa Api快速查询资产信息并识别重点资产的指纹，也可针对IP/IP段或资产列表进行快速的指纹识别。
Glass旨在帮助红队人员在资产信息收集期间能够快速从C段、大量杂乱的资产中精准识别到易被攻击的系统，从而实施进一步测试攻击。

 s7ck Team F-box旨在通过开源或者开放的方式，长期维护并推进涉及安全研究各个领域不同环节的工具化，高度自动化，将立足于不同安全领域、不同安全环节的研究人员和工具链接/封装/优化起来。主要目的是改善安全圈内工具庞杂、水平层次不齐、开源无人维护、工具找不到的等多种问题，营造一个更好更开放的安全工具促进与交流的技术氛围。

### 开发语言

* Python3

### 运行环境

* Linux
* Windows
* Mac

### 使用依赖库

* requests
* colorama
* prettytable


### 安装
	git clone https://github.com/s7ckTeam/Glass
	cd Glass
	pip3 install -r requirements.txt


## 二、使用：

```
Usage: python3 Glass.py -i 127.0.0.1 or 127.0.0.0/24
Usage: python3 Glass.py -f ips.txt
Usage: python3 Glass.py -u http://s7ck.com/
Usage: python3 Glass.py -w urls.txt

Options:
  -h, --help            show this help message and exit
  -i IP, --ip=IP        Input your ip.
  -f FILE, --file=FILE  Input your ips.txt
  -u URL, --url=URL     Input your url.
  -w WEB, --web=WEB     Input your webs.txt

-i 可指定单独IP或者IP段（需添加您fofa的API）
-f 批量要扫的IP或IP段（需添加您fofa的API）
-u 单个url识别
-w 批量url识别
```
#### 相关配置更改

* API设置在`config/config.py`中`fofaApi`设置，输入对应的`email`与`key`即可
* 线程默认 `100`可在`config/config.py`中`threadNum`修改线程数 **（注：建议在200以内）**
* 每日一说可设置开启关闭，在`config/config.py`中`tosayRun`，`True`为开，`False`为关

 
Glass提供了**四种**指纹识别方式，可从本地读取识别，也可以从FOFA进行批量调用API识别(需要FOFA密钥)。

**1.本地识别：**

```bash
python Glass.py -u http://s7ck.com  // 单url测试
python Glass.py -w url.txt  // url文件内

```

**2.FOFA识别：**

注意：从FOFA识别需要配置FOFA 密钥以及邮箱，在`.../config/config.php`内配置好密钥以及邮箱即可使用。

```bash
fofaApi = {
    "email": "admin@s7ck.com",
    "key": "1234567890",
}
```

```bash
python Glass.py -i 127.0.0.1  //支持单IP资产
python Glass.py -i 127.0.0.1/24 //支持IP段资产
python Glass.py -f ips.txt //支持文本内IP资产，可添加IP段

```

#### 结果输出

结果输出在`../output/xxxxx.txt`

#### 识别规则添加方法

目前只加了对源码和header头文件进行识别，后面会陆续添加识别的规则以及方式

规则文件在`config/rules.py`

**添加方法**

```
    "CMS": {
        "regex": "(<title>Test Demo</title>)",
        "type": "code",
    },
    "CMS2": {
        "regex": "(set-cookie:xxx-xxx)",
        "type": "headers",
    },
```

* `regex`为正则规则
* `type`为识别方式


## 三、效果：

**1.本地识别：**
![](./Image/u1.png)
![](./Image/u2.png)

**2.fofa识别：**

![](./Image/c0.png)
![](./Image/c1.png)
![](./Image/c2.png)
![](./Image/Glass.gif)
## 四、更新：
### 测试更新信息

- 0.9（优化IP段的扫描，以及批量下可添加IP段）
- 0.8（更改线程运行方式）
- 0.7（与协程对比，最好还是用线程并发）
- 0.6（更改配色方案，采用线程并发，增加单url以及批量url识别）
- 0.5（优化采集速度以及与识别分开，增加输出）
- 0.4（整理识别规则）
- 0.3（添加指纹扫描）
- 0.2（更新每天一说，让红队不再乏味）
- 0.1（写出整体实现功能，Fofa接口的设置与采集）

### 正式版

* 1.1（处理每日一说超时后写入空文件问题）
* 1.0（全面优化识别效率，命中率99%）
* 0.9（添加每日一说获取异常，添加识别规则）

## 五、特别感谢
* s7ck Team
*  Morker
* XBJ
* d0gamn

**特别感谢EHole(棱洞) 运行思路**

## 六、文末
Github项目地址（BUG、需求、规则欢迎提交）: https://github.com/s7ckTeam/Glass
