# python-AI-19-
python+AI笔记(19)
# 一段-十章-pyecharts模块介绍
#pyecharts.org这个网站能有各种各样的柱状图供我们使用
#安装pyecharts包就用pip install pyecharts就可以了

# 一段-十章-pyecharts的入门操作
#导包
from pyecharts.charts import Line
from pyecharts.options import TitleOpts,LegendOpts,ToolboxOpts,VisualMapOpts
#创建一个折线图对象
line = Line()
#给折线图对象添加x轴
line.add_xaxis(["中国","美国","英国"])
#给折线图对象添加y轴
line.add_yaxis("GDP",[30,20,10])

#设置全局配置项
line.set_global_opts(
    title_opts = TitleOpts("GDP显示",pos_left = "center",pos_bottom = "1%")
    legend_opts = legendOpts(is_show = True),
    toolbox_opts = ToolboxOpts(is_show = True),
    visualmap_opts = VisualMapOpts(is_show = True)
    
)
#通过render方法，将代码生成为图像
line.render()

# 一段-十章-数据准备
import json
from pyecharts.charts import Line
from pyecharts.options import TitleOpts,LabelOpts
#处理数据
f_us = open("D:/美国.txt","r",encoding = "UTF-8")
us_data = f_us.read()  #读取到f_us的全部内容
f_jp = open("D:/日本.txt","r",encoding = "UTF-8")
jp_data = f_jp.read()。#日本的全部数据
f_in = open("D:/印度.txt","r",encoding = "UTF-8")
in_data = f_in.read()
#去掉不合JSON规范的开头
us_data = us_data.replace("json_4723895629876592(","")  #在这个文件中，把两个小括号意外的东西全部删除就可以了
us_data = jp_data.replace("json_4723895629876592(","")  #这里每个国家的json文件开头内容都不一样，注意在文件复制
us_data = in_data.replace("json_4723895629876592(","")
#去掉不合JSON规范的结尾
us_data = us_data[:-2]
jp_data = jp_data[:-2]
in_data = in_data[:-2]
#JSON转Python字典
us_dict = json.loads(us_data)
jp_dict = json.loads(jp_data)
in_dict = json.loads(in_data)

#获取trend key
us_trend_data = us_dict['data'][0]['trend']  #这些中括号都是us_dict的子目录
jp_trend_data = jp_dict['data'][0]['trend']
in_trend_data = in_dict['data'][0]['trend']

#获取日期数据，用于x轴，取2020年(到314下标结束)
us_x_data = trend_data['updataData'][:314]
jp_x_data = trend_data['updataData'][:314]
in_x_data = trend_data['updataData'][:314]

#获取日期数据，用于y轴，取2020年(到314下标结束)
us_y_data = us_trend_data['list'][0]['data']
jp_y_data = jp_trend_data['list'][0]['data']
in_y_data = in_trend_data['list'][0]['data']

# 一段-十章-生成折线图
#生成图表
ine = Line()
#添加x轴数据
line.add_xaxis(us_x_data)  #x轴是共用的，所以使用一个国家的数据即可
#添加y轴数据
line.add_yaxis("美国确诊人数",us_y_data,label_opts = LabelOpts(is_show = False))  #添加美国y轴数据
line.add_yaxis("日本确诊人数",jp_y_data,label_opts = LabelOpts(is_show = False))
line.add_yaxis("印度确诊人数",in_y_data,label_opts = LabelOpts(is_show = False))

#设置简单的全局选项
line.set_global_opts(
    #标题设置
    title_opts = TitleOpts(title = "2020年美日印三国确诊人数对比折线图"，pos_left = "center",pos_bottom = "1%")
)

line.render()
f_us.close()
f_jp.close()
f_in.close()

# 一段-十一章-数据可视化案例-地图-基础地图的使用
#演示地图可视化的基本使用
from pyecharts.charts import Map
from pyecharts.options import VisualMapOpts
#准备地图对象
map = Map()
#准备数据
data =[
    ("北京",99),
    ("上海",199),
    ("湖南",299),
    ("台湾",399),
    ("广东",499)
]
#添加数据
map.add("测试地图",data,"china")

#设置全局选项
map.set_global_opts(
    visualmap_opts = VisualMapOpts(
        is_show = True,
        is_piecewise = True,
        pieces = [
            {"min":1,"max":9,"label":"1-9","color":"#CCFFFF"},
            {"min":10,"max":999,"label":"10-99","color":"#FF6666"},
            {"min":100,"max":500,"label":"100-500","color":"#990033"}
        ]
    )
)
map.render()
