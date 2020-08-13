
# esp8266_weather_server
这是一个ESP8266天气服务器,可以在windows、linux、macos服务器上面运行。

## 部署

下载操作系统对应的程序和**.env**文件(也可以自己创建一个,需要和程序在同一目录)

**.env**用记事本打开编辑，在等号后面填写你申请的APPCODE

### WEATHER_APPCODE=[天气预报APPCODE申请地址](https://market.aliyun.com/products/57126001/cmapi014123.html)
### CALENDAR_APPCODE=[农历APPCODE申请地址](https://www.juhe.cn/docs/api/id/177)

填写完毕后保存关闭，直接运行天气服务器即可
## 获取数据
GET方式获取数据，只需要在结尾写上城市名

> http://localhost:5000/weather/北京

返回的Json数据样式
```javascript
{
  "weather": [
    {
      "night_weather_code": "01",
      "day_weather": "阴",
      "night_weather": "多云",
      "jiangshui": "28%",
      "air_press": "973.1hPa",
      "night_wind_power": "3-4级 5.5~7.9m/s",
      "day_wind_power": "3-4级 5.5~7.9m/s",
      "day_weather_code": "02",
      "sun_begin_end": "06:09|19:20",
      "ziwaixian": "中等",
      "day_weather_pic": "http://app1.showapi.com/weather/icon/day/02.png",
      "weekday": 4,
      "night_air_temperature": "26",
      "day_air_temperature": "34",
      "day_wind_direction": "南风",
      "day": "08月13日",
      "night_weather_pic": "http://app1.showapi.com/weather/icon/night/01.png",
      "night_wind_direction": "南风",
      "day2": "08-13",
      "weather": "阴转多云",
      "temperature": "26~34℃",
      "icon": "02"
    }
	.........................
  ],
  "calendar": {
    "date": "2020-8-13",
    "animalsYear": "鼠",
    "avoid": "开市.立券.纳财.作灶.",
    "lunar": "六月廿四",
    "lunarYear": "庚子年",
    "suit": "纳采.订盟.嫁娶.祭祀.祈福.普渡.开光.安香.出火.移徙.入宅.竖柱.修造.动土.竖柱.上梁.起基.造屋.安门.造庙.造桥.破土.启攒.安葬.",
    "weekday": "星期四",
    "year-month": "2020-8"
  }
}
```
在ESP8266的GET请求中需要对中文字符串进行编码
urlencode("北京")
```cpp
String urlencode(String str)
{
  String encodedString = "";
  char c;
  char code0;
  char code1;
  char code2;
  for (int i = 0; i < str.length(); i++) {
    c = str.charAt(i);
    if (c == ' ') {
      encodedString += '+';
    } else if (isalnum(c)) {
      encodedString += c;
    } else {
      code1 = (c & 0xf) + '0';
      if ((c & 0xf) > 9) {
        code1 = (c & 0xf) - 10 + 'A';
      }
      c = (c >> 4) & 0xf;
      code0 = c + '0';
      if (c > 9) {
        code0 = c - 10 + 'A';
      }
      code2 = '\0';
      encodedString += '%';
      encodedString += code0;
      encodedString += code1;
      //encodedString+=code2;
    }
    yield();
  }
  return encodedString;

}
```


![4寸大屏天气预报](https://github.com/vaemc/esp8266_weather_server/blob/master/1.png)
