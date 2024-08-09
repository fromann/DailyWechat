# 微信每日早安推送

    要想实现此仓库功能需要修改少量代码，若未曾了解过Python请不要复刻仓库

## 细节详见 底部更新内容

> 2023.10.9新规则运行导致部分功能失效
> 
> 2023.4.27更新 免费天气API失效更换心知天气API 详见更新日志↓↓↓

![墨菲安全](https://s.murphysec.com/badge/fromann/DailyWechat.svg)
[本项目已通过**墨菲安全**检测](https://www.murphysec.com/p/fromann/DailyWechat)


## 开源协议 [GNU GPLv3](./LICENSE)

>[General Public License v3.0 ](./LICENSE)
允许个人使用、商业使用、专利授权，允许复制、分发、修改，并且作者不承担用户使用的一切后果。但是它有很多限制：
- 必须开源
一旦使用了这个协议，如果他人想要进行分发、修改，那么他们修改后的源代码也必须开源。这是开源的核心保障，如果没有这条规定，就没有人愿意持续公开自己的源码了。
- 保留协议和版权
保留对协议和版权的叙述。
- 不允许更换协议
一旦最原始的源码使用了GPL，其衍生的所有代码都必须使用GPL。这也是开源保障之一
- 声明变更
对于代码的变更需要有文档进行说明改了哪些地方。

## 正文
### 效果
![1](pic/show.jpg)
### 思路
本项目实现单公众号对多用户发送模板信息
#### Secrets/Action Key表

![步骤](pic/buzhou.png)

将公众号的`APP_ID` , `APP_SECRET` , `Template_ID`，`WEATHER_API_KEY `填入Key表
![2](pic/secret.png)

#### 用户信息文件
用户信息文件储存于json文件中，实现用户信息的差异化储存，便于差异化分发

    开源平台也要保护好自己的名字资料
~~~json
{
  "data": [
    {
      "user_name": "用户1的名字",
      "user_id": "用户1的ID",
      "born_date": "用户1的出生日期（注意格式）",
      "city": "用户1城市"
    },
    {
      "user_name": "用户2的名字",
      "user_id": "用户2的ID",
      "born_date": "用户2的出生日期（注意格式）",
      "city": "用户2的城市" 
    }
    ]
}
~~~
以上是基本结构
若想新增用户可以按照以下格式花括号（`{}`）之间添加用`,`分割
~~~json
{
  "user_name": "用户2的名字",
  "user_id": "用户2的ID",
  "born_date": "用户2的出生日期（注意格式）",
  "city": "用户2的城市" 
}
~~~
#### 微信模板

~~~txt
{{time.DATA}}

φ(゜▽゜*)♪{{name.DATA}}小盆友

坐标城市：{{city.DATA}}o(〃'▽'〃)o
当前天气：{{weather.DATA}} ヾ(≧▽≦*)o
当前风向：{{wind.DATA}}( •̀ ω •́ )✧
今日温度：{{tem_low.DATA}}℃~{{tem_high.DATA}}℃ ( •̀ ω •́ )

庆祝自己在世界上第{{born_days.DATA}}天(❁´◡`❁)
距离下次生日还有{{birthday_left.DATA}}天╰(*°▽°*)╯

{{words.DATA}}
~~~
## 注意事项

- 生日的日期格式是：`05-20`，
- 纪念日的格式是 `2022-08-09`
- 城市应为 `beijing`,`tianjin`,`jinan`

## 更新日志
### 2023.10.09
### 微信新规则详细
>参考公告：关于规范公众号模板消息的再次公告 - 微信开放社区 https://developers.weixin.qq.com/community/develop/doc/000a2ae286cdc0f41a8face4c51801

![img](pic/newissue.png)

### 2024.8.9
- 修复了部分代码问题
- 更新wx模块文本（注意本文最下面 重新添加模块）

### 2023.4.27

由于天气接口失效，更换[心知天气 - 天气数据API接口(seniverse.com)](https://www.seniverse.com/)

#### 高德地图API获取教程

- 打开并注册**[心知天气 - 天气数据API接口(seniverse.com)](https://link.zhihu.com/?target=https%3A//lbs.amap.com/)**
- 点击添加产品选择**免费版**获取 API Key私钥

![img](pic/weather.png)

- 将APIKey 添加到`WEATHER_API_KEY `填入Key表
  ![2](pic/secret.png)

- 注意本次更新也要更新json文件 城市只能支持地级市并且全拼

  ~~~json
  {
    "user_name": "用户2的名字",
    "user_id": "用户2的ID",
    "born_date": "用户2的出生日期（注意格式）",
    "city": "用户2的城市" //只能支持地级市
  }
  ~~~


- 本次更新也要更新微信模板

  ~~~txt
  
  
  {{head.DATA}}
  {{time.DATA}}
  
  φ(゜▽゜*)♪{{name.DATA}}小盆友
  
  坐标城市：{{city.DATA}}o(〃'▽'〃)o
  当前天气：{{weather.DATA}} ヾ(≧▽≦*)o
  当前风向：{{wind.DATA}}( •̀ ω •́ )✧
  今日温度：{{tem_low.DATA}}℃~{{tem_high.DATA}}℃ ( •̀ ω •́ )
  
  庆祝自己在世界上第{{born_days.DATA}}天(❁´◡`❁)
  距离下次生日还有{{birthday_left.DATA}}天╰(*°▽°*)╯
  
  {{words.DATA}}
  {{end.DATA}}
  ~~~

  
