from wechatpy import WeChatClient
from wechatpy.client.api import WeChatMessage
import os
import json
from datetime import datetime, timedelta
import random
import requests
import locale

locale.setlocale(locale.LC_CTYPE, "chinese")

nowtime = datetime.utcnow() + timedelta(hours=8)
today = datetime.strptime(str(nowtime.date()), "%Y-%m-%d")


def get_time():
    dictDate = {'Monday': '星期一', 'Tuesday': '星期二', 'Wednesday': '星期三', 'Thursday': '星期四',
                'Friday': '星期五', 'Saturday': '星期六', 'Sunday': '星期天'}
    a = dictDate[nowtime.strftime('%A')]
    return nowtime.strftime("%Y年%m月%d日") + a


def get_words():
    words = requests.get("https://api.shadiao.pro/chp")
    if words.status_code != 200:
        return get_words()
    return str(words.json()['data']['text'])

def get_shiju():
    shiju = requests.get('https://v2.jinrishici.com/one.json')
    my_list = shiju.json()['data']['content'].split('，')  # 假设这是你的初始列表，现在它是空的
    # 首先检查列表是否为空
    if not my_list:
        # 如果列表为空，添加两个相同的元素
        my_list.append('今日暂无')  # 或者你可以添加任何你想要的默认值
        my_list.append('今日暂无')
    # 现在检查元素个数是否少于两个
    elif len(my_list) < 2:
        # 如果元素个数少于两个，则扩充列表
        my_list.append(my_list[0])  # 添加与第一个元素相同的元素
    return my_list

def get_weather(city, key):
    url = f"https://api.seniverse.com/v3/weather/daily.json?key={key}&location={city}&language=zh-Hans&unit=c&start=-1&days=5"
    res = requests.get(url).json()
    print(res)
    weather = (res['results'][0])["daily"][0]
    city = (res['results'][0])["location"]["name"]
    return city, weather


def get_count(born_date):
    delta = today - datetime.strptime(born_date, "%Y-%m-%d")
    return delta.days


def get_birthday(birthday):
    nextdate = datetime.strptime(str(today.year) + "-" + birthday, "%Y-%m-%d")
    if nextdate < today:
        nextdate = nextdate.replace(year=nextdate.year + 1)
    return (nextdate - today).days


if __name__ == '__main__':
    # app_id = os.getenv("APP_ID")
    # app_secret = os.getenv("APP_SECRET")
    # template_id = os.getenv("TEMPLATE_ID")
    # weather_key = os.getenv("WEATHER_API_KEY")

    app_id = 'wx15f8a714689bf4f2'
    app_secret = '03d838dfd5bad7f8de3269ed3a01d228'
    template_id = 'd1QMF0s8tmTiyAVME9QhoLT-bMWcx7-L4KQrGMeW4KA'
    weather_key = 'SchADXcMrBSMavb5K'

    client = WeChatClient(app_id, app_secret)
    wm = WeChatMessage(client)

    f = open("./users_info.json", encoding="utf-8")
    js_text = json.load(f)
    f.close()
    data = js_text['data']
    num = 0
    words = get_shiju()
    out_time = get_time()

    for user_info in data:
        born_date = user_info['born_date']
        birthday = born_date[5:]
        city = user_info['city']
        user_id = user_info['user_id']
        name = user_info['user_name'].upper()

        wea_city, weather = get_weather(city, weather_key)
        data = dict()
        data['time'] = {'value': out_time}
        data['words1'] = {'value': words[0]}
        data['words2'] = {'value': words[1]}
        data['weather'] = {'value': weather['text_day']}
        data['city'] = {'value': wea_city}
        data['tem_high'] = {'value': weather['high']}
        data['tem_low'] = {'value': weather['low']}
        data['born_days'] = {'value': get_count(born_date)}
        data['birthday_left'] = {'value': get_birthday(birthday)}
        data['wind'] = {'value': weather['wind_direction']}
        data['name'] = {'value': name}

        res = wm.send_template(user_id, template_id, data)
        print(res)
        num += 1
    print(f"成功发送{num}条信息")
