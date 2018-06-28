># 安装
>root下:pip3 install Flask
***
># 本地调试
	export FLASK_APP=hello.py
	export FLASK_DEBUG=1
	flask run
***
># 外部可见服务器
>flask run --host=0.0.0.0
***
># URL重定位
>@app.route('URL地址', methods=['GET', 'POST'])
***
># flask-cors 实现跨域请求
安装：pip install -U flask-cors

	from flask import Flask
	from flask.ext.cors import CORS
	app = Flask(__name__)
	CORS(app)s
	@app.route("/")
	def helloWorld():
  		return "Hello, cross-origin-world!"