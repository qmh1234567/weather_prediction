# weather_prediction

## 数据集概述
数据集是datasets/weather.csv，该表格包含2013年1月1日-2017年12月31日，来自北京、上海、重庆的共58个站点按天记录的逐日天气观测数据。
- 形如：
date,station,city,county,pressure,wind_speed,wind_direction,temperature,humidity,rain20,rain08,cloud,visibility,phenomenon
20130101,巴南,重庆市,巴南市,959.1,3.4,999003,6,48,0,0,3,8000,"02,03,"
20130101,綦江,重庆市,綦江市,962.6,6.5,999012,5.6,59,0,0,35,15000,"02,"
20130101,渝北,重庆市,渝北市,963.9,4.1,999003,5.9,44,0,0,0,10000,"01,02,03,"
20130101,梁平,重庆市,梁平市,965.3,2.2,999001,2.1,68,0,0,0,9000,"01,10,02,03,"
20130101,垫江,重庆市,垫江县,968.9,1.7,999004,3.8,61,0,0,0,8000,"01,02,03,"
20130101,大足,重庆市,大足市,972.4,2.4,999003,3,72,0,0,0,8000,"01,02,03,10,"
20130101,长寿,重庆市,长寿市,974,2.3,999001,5.4,61,0,0,0,9000,"10,02,03,"
20130101,涪陵,重庆市,涪陵市,975,4.1,999003,4.6,70,0,0,3,1200,"10,02,03,"
20130101,永川,重庆市,永川市,977.2,3.3,999003,5.6,54,0,0,0,7000,"02,03,10,"
- 字段说明

| 字段名 | 中文名称 | 说明 |
| :-: | :-: | :-: | 
| date | 日期 | 无 |
| station | 站点 | 观测站点名称|
| city	| 城市|	站点所属城市|
| county	| 市辖区/县/自治县	| 站点所处县级行政区|
| pressure |	平均气压(百帕)	| 
| wind_speed |	最大风速(米/秒)|
| wind_direction	| 最大风速的风向(度)| 
| temperature |	平均气温(摄氏度℃)|
| humidity	| 平均相对湿度(百分率)	|
| rain20	| 20-20时降水量(毫米)	代表观测日期当天20时获取，是从前一日20时至当日20时这24小时的累计降水量数据，是常用的降水量数据|
| rain08 |	08-08时降水量(毫米)	前一日8时至观测日8时的累计降水量数据|
| cloud	| 平均总云量(百分比)	观测到的天空有效面积内云量的占比|
| visibility	|最小水平能见度(米)	|
| phenomenon |	天气现象记录	| *见下方“天气现象记录说明”|

- 天气现象记录说明

| 数值 | 类别名 |  说明 | 对应phenomenon的代码值 |
| :--: | :--: | :--: |  :--: |
| 0 | sunny | 晴天/无特殊天气状况 | 0, 2, 100, 102, 211, 508-511 |
| 1 | cloud | 多云 | 1, 3, 19, 101, 103 |
| 2 | rain | 降雨 | 14, 15, 16, 20, 21, 23, 24, 25, 26, 27, 29, 50-69, 80-84, 87-99, 121-123, 125, 126, 140-144, 150-168, 180-184, 190, 192, 193, 195, 196, 227, 250-266, 280-282, 285, 286, 289, 290, 292, 293 |
| 3 | fog | 有雾 | 4, 10, 11, 12, 28, 40-49, 104, 105，120, 130-135, 224, 241-249 |
| 4 | haze | 有霾 | 4, 5, 104, 105, 110, 206, 210 |
| 5 | dust | 尘土/沙尘 | 6, 7, 8, 9, 30-35, 98, 104, 105, 204-209, 220, 221, 230 |
| 6 | thunder | 打雷 | 17, 29, 91, 95-99, 126, 190-196, 217, 292, 293 |
| 7 | lightning | 闪电 | 13, 17, 29, 91, 95-99, 112, 126, 190-196, 213, 217, 292, 293 |
| 8 | snow | 降雪 | 22, 23, 26, 36-39, 68-78, 83-90, 93-95, 97, 111, 122, 124, 127-129, 145-148, 170-173, 177, 178, 185-187, 192, 195, 223, 239, 259, 270-279, 282-287, 290, 291 |
| 9 | hail | 冰雹 | 23, 27, 79, 87, 89, 93, 94, 96, 99, 174-176, 189, 193, 196, 284-291 |
| 10 | wind | 强风 | 17, 18, 29, 91, 95-99, 118, 126, 190-196, 199, 217, 292, 293 |

注意上述各种类别的天气可能会重叠，即在同一天中出现多种天气状况，因此该字段以onehot的形式保存，例如：

| sunny | cloud |  rain | fog | haze | dust | thunder | lightning | snow | hail | wind |
| :--: | :--: | :--: |  :--: |  :--: |  :--: |  :--: |  :--: |  :--: |  :--: |  :--: |
| 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 0 |

### 脚本概述
1. 本项目的主要代码在weather.ipynb中呈现，该脚本主要实现两种任务：回归任务和多标签分类任务。
2. 回归任务中采用LSTM模型是多对一的形式，即利用处理好的全部26个属性来预测单一属性，如温度，可见度和湿度等。
3. 多标签分类任务采用LSTM模型是多对多的形式，即利用处理好的全部26个属性来预测气象的11类属性，即上方的天气现象记录说明。

