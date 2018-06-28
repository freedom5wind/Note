># 实现结构体
	class Struct:
		pass
	struct1 = Struct()
	struct1.var = ...
***
># 不允许（函数）前向引用
***
># strip & split
>strip用于除去换行符等，split用于分割
***
># 在函数内使用全局变量
>global variable
***
># unix时间戳字符串转换
	import time
	def timestamp_datetime(value):
    	format = '%Y-%m-%d %H:%M:%S'
    	value = time.localtime(value)
    	# 经过localtime转换后变成
    	# time.struct_time(tm_year=2012, tm_mon=3, tm_mday=28, tm_hour=6, tm_min=53, tm_sec=40, tm_wday=2, tm_yday=88, tm_isdst=0)
   		# 最后再经过strftime函数转换为正常日期格式。
    	dt = time.strftime(format, value)
    	return dt
	def datetime_timestamp(dt):
     	# 中间过程，一般都需要将字符串转化为时间数组
     	time.strptime(dt, '%Y-%m-%d %H:%M:%S')
     	# time.struct_time(tm_year=2012, tm_mon=3, tm_mday=28, tm_hour=6, tm_min=53, tm_sec=40, tm_wday=2, tm_yday=88, tm_isdst=-1)
     	# 将"2012-03-28 06:53:40"转化为时间戳
     	s = time.mktime(time.strptime(dt, '%Y-%m-%d %H:%M:%S'))
     	return int(s)
***
># 发送POST方式的http请求
	import urllib.request
	import urllib.parse
	data = urllib.parse.urlencode({'spam': 1, 'eggs': 2, 'bacon': 0})
	data = data.encode('utf-8')
	request = urllib.request.Request("http://requestb.in/xrbl82xr")
	# adding charset parameter to the Content-Type header.
	request.add_header("Content-Type","application/x-www-form-urlencoded;charset=utf-8")
	f = urllib.request.urlopen(request, data)
	print(f.read().decode('utf-8'))
***
># url转码
	urllib.parse.quote(str) 		# 未编码斜线
	urllib.parse.quote_plus(str) 	# 编码斜线
***
># 字符串格式化
格式化字符串时，Python使用一个字符串作为模板。模板中有格式符，这些格式符为真实值预留位置，并说明真实数值应该呈现的格式。Python用一个tuple将多个值传递给模板，每个值对应一个格式符。比如下面的例子：print("I'm %s. I'm %d year old" % ('Vamei', 99))
***
># 类的封装
>类中所有双下划线开头的名称如__x都会自动变形成：_类名__x的形式，一般通过self.__x引用
***
># 手动调试
* type()：获取变量的类型
* dir()：获取变量的所有属性
* getattr()：访问对象的属性
* hasattr()：检查是否存在一个属性
* setattr()：设置一个属性。如果属性不存在，会创建一个新属性
* delattr()：删除属性
***
># 特殊属性和用法
[链接](http://blog.csdn.net/qq_30175203/article/details/51704636)
***
># *和 **
用于打包和拆包参数，* 对应元组, **对应字典
***
># property方法
property是一种特殊的属性，访问它时会执行一段功能（函数）然后返回值（就是一个装饰器）,被property装饰的属性会优先于对象的属性被使用，而被propery装饰的属性,分成三种：property、被装饰的属性名.setter、被装饰的属性名.deleter（都是以装饰器的形式）。将函数定义成特性能实现属性和函数的统一访问。
***
># do-while
	while True:
		...
		if 条件：
			break
***
># sorted
定义：sorted(iterable, key=None, reverse=False)，key一般用lambda表达式，类似匿名函数，形如lambda 输入：返回值
***
># 数据动态可视化
[链接](http://python.jobbole.com/81185/)
***
># sir模型模拟Zombie breakout
* [链接](http://maxberggren.se/2014/11/27/model-of-a-zombie-outbreak/)
* [zombie_france.py](https://gist.github.com/Zulko/6aa898d22e74aa9dafc3)
***
># 修改pip源
修改 ~/.pip/pip.conf (没有就创建一个)， 内容如下：
	[global]
	index-url = https://mirrors.aliyun.com/pypi/simple/
***
># 利用系统Hook从异常进入pdb
	`crash_on_ipy.py`:
		import sys
		class ExceptionHook:
    		instance = None
    		def __call__(self, *args, **kwargs):
        		if self.instance is None:
           	 		from IPython.core import ultratb
            		self.instance = ultratb.FormattedTB(mode='Plain',
                 		color_scheme='Linux', call_pdb=1)
        		return self.instance(*args, **kwargs)
		sys.excepthook = ExceptionHook()
然后在项目代码某个地方 import crash_on_ipy 就可以了。
***
># pdb
[链接](http://blog.csdn.net/linda1000/article/details/11031771)
***
># 返回当前命名空间中所定义的变量
who
***
># 快速定义函数
	function_name = [expression_about_i for i in input]
	function_name(input)
***

