># 服务控制
* 开启服务：nginx
* 控制服务：nginx -s stop/quit/reload/reopen
***
># 相关文件
* /etc/nginx/nginx.conf 配置文件
* /var/run/ nginx.pid
* /var/log/nginx 记录文件
***
># 更改端口后
>service nginx restart
***
># Flask搭建
1. sudo apt-get install python3-pip
2. sudo pip3 install flask
3. sudo pip3 install uwsgi
4. cd
5. vim app.py
6. vim uwsgi.ini
7. uwsgi uwsgi.ini &
8. sudo vim /etc/nginx/sites-available/default

	app.py:
	from flask import Flask
	app = Flask(__name__)
	@app.route('/')
	def hello_world():
		return 'Hello World!'
	if __name__ = '__main__':
		app.run()

	uwsgi.ini:
	[uwsgi]
	socket=127.0.0.1:2333
	wsgi-file=app
	callable=app

	default:
	server {
		listen 80;
		server_name [host_ip];
		location '/' {
			include uwsgi_params;
			uwsgi_pass 127.0.0.1:2333
		}
	}
***
># 证书验证
1. ca根证书生成root
	1. cd /etc/pki/CA
	2. 生成根密钥：openssl genrsa -out akey.pem 2048
	3. 生成根证书：openssl req -new -x509 -key cakey.pem -out cacert.pem
	4. cd /etc/nginx/ssl
	5. openssl genrsa -out nginx.key 2048
	6. 生成证书签署请求：openssl req -new -key nginx.key -out nginx.csr
	7. 签署证书：openssl x509 -req -in server.csr -CA /etc/pki/CA/cacert.pem -CAkey /etc/pki/CA/cakey.pem -CAcreateserial -out server.crt
2. 配置nginx

	server {
		listen 442;
		servername [host_ip];

		ssl on;
		ssl_certificate /etc/nginx/ssl/nginx.crt;
		ssl_certificate_key /etc/nignx/ssl/nginx.key;
	}
***
># nginx学习笔记
* 标准 ngx_rewrite 模块的 set 配置指令对变量 $a 进行了赋值操作：
set $a "hello world";
* Nginx 变量名前面有一个 $ 符号。
* 可以直接把变量嵌入到字符串常量中以构造出新的字符串：

    set $a hello;
    set $b "$a, $a";
>* nginx.conf 配置文件中最外围的 http 配置块以及 events 配置块
* 第三方 ngx_echo 模块的 echo 配置指令
* 没有办法把特殊的 $ 字符给转义掉
* 通过不支持“变量插值”的模块配置指令专门构造出取值为 $ 的 Nginx 变量，然后再在 echo 中使用这个变量。看下面这个例子：

    geo $dollar {
        default "$";
    }

    server {
        listen 8080;

        location /test {
            echo "This is a dollar sign: $dollar";
        }
    }
>* 标准模块 ngx_geo 提供的配置指令 geo 来为变量 $dollar 赋予字符串 "$"
* 当引用的变量名之后紧跟着变量名的构成字符时（比如后跟字母、数字以及下划线），我们就需要使用特别的记法来消除歧义。Nginx 的字符串记法支持使用花括号在 $ 之后把变量名围起来
* Nginx 变量的创建和赋值操作发生在全然不同的时间阶段。Nginx 变量的创建只能发生在 Nginx 配置加载的时候，或者说 Nginx 启动的时候；而赋值操作则只会发生在请求实际处理的时候。这意味着不创建而直接使用变量会导致启动失败，同时也意味着我们无法在请求处理时动态地创建新的 Nginx 变量。
* Nginx 变量一旦创建，其变量名的可见范围就是整个 Nginx 配置，甚至可以跨越不同虚拟主机的 server 配置块。
* Nginx 变量名的可见范围虽然是整个配置，但每个请求都有所有变量的独立副本，或者说都有各变量用来存放值的容器的独立副本，彼此互不干扰。
* 第三方模块 ngx_echo 提供的 echo_exec 配置指令，发起到 location /bar 的“内部跳转”。所谓“内部跳转”，就是在处理请求的过程中，于服务器内部，从一个 location 跳转到另一个 location 的过程。既然是内部跳转，当前正在处理的请求就还是原来那个，只是当前的 location 发生了变化，所以还是原来的那一套 Nginx 变量的容器副本。
* Nginx 内建变量最常见的用途就是获取关于请求或响应的各种信息。由 ngx_http_core 模块提供的内建变量 $uri，可以用来获取当前请求的 URI（经过解码，并且不含请求参数），而 $request_uri 则用来获取请求最原始的 URI （未经解码，并且包含请求参数）。
* 另一个特别常用的内建变量其实并不是单独一个变量，而是有无限多变种的一群变量，即名字以 arg_ 开头的所有变量，我们估且称之为 $arg_XXX 变量群。一个例子是 $arg_name，这个变量的值是当前请求名为 name 的 URI 参数的值，而且还是未解码的原始形式的值。

