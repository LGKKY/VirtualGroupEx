# Virtual Group Ex



Yet Another v Group Forum | 又一个v字幕组综合平台。

使用 Server-Side Blazor on ASP.NET Core with .NET 5.0.7，数据库使用SQLite 3。

## 一点有的没的

之前做的那个TewiClipool其实是数据库课的课程设计作业，虽然能用但是技术栈选的不太合适，而且写得太烂了。所以重做了这个，现在效果还可以吧。

开发过程其实是在另一个Repo，但是那个Commit太杂乱了，所以新开了一个。

## 编译

编译前需安装 [.NET 5.0 SDK](https://docs.microsoft.com/zh-cn/dotnet/core/install/linux)

另外，反代需要配置一下 Web Socket。
#### 克隆代码

```shell
git clone https://github.com/DCTewi/VirtualGroupEx.git
```
#### 编译项目

```shell
#进入项目文件夹
cd VirtualGroupEx/
# 编译程序到 "published/"
dotnet publish -c Release -o published/ 
# 进入编译完成文件夹
cd published/
```

## 运行

```shell
# 启动 VirtualGroupEx
dotnet VirtualGroupEx.Server.dll
```
网站会默认开启在

https://localhost:5001
http://localhost:5000
配置文件在 ` appsettings.json `

## 配置网站访问

以下仅提供nginx简易配置方式

找到`nginx.conf`中的


```
	##
	# Virtual Host Configs
	##

	include /etc/nginx/conf.d/*.conf;
	include /etc/nginx/sites-enabled/*;

```
改成这样

```
	##
	# Virtual Host Configs
	##
	map $http_upgrade $connection_upgrade {
        default Upgrade;
        ''      close;
        }
	include /etc/nginx/conf.d/*.conf;
	include /etc/nginx/sites-enabled/*;
```

然后在/etc/nginx/conf.d/里面新建一个.conf文件（写什么都行.conf）

```
server {
    listen      80;
    #“你的域名.com” 对应的你的域名，把下面的域名改成你们自己的域名就可以了
    server_name 你的域名.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection $connection_upgrade;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}

```

如果要配置https就在上面的文件最后添加以下代码（请自行申请证书）
```
server{
        #监听443端口
        listen 443;
        #对应的域名，把下面的域名改成你们自己的域名就可以了
        server_name 你的域名.com;
        ssl on;
        #证书的第一个文件的全路径
        ssl_certificate *.crt;
        #证书的第二个文件的全路径
        ssl_certificate_key *.key;
        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
        ssl_prefer_server_ciphers on;
        location /{
            proxy_pass         https://localhost:5001;
            proxy_http_version 1.1;
            proxy_set_header   Upgrade $http_upgrade;
            proxy_set_header   Connection $connection_upgrade;
            proxy_set_header   Host $host;
            proxy_cache_bypass $http_upgrade;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header   X-Forwarded-Proto $scheme;
    }
 }
```

其他程序请参见[配置Web Socket](https://docs.microsoft.com/zh-cn/aspnet/core/blazor/host-and-deploy/server?view=aspnetcore-5.0) 。


## 截图

截图可能不是最新的，可自行在软件内体验

[![RpH7VS.md.png](https://z3.ax1x.com/2021/06/18/RpH7VS.md.png)](https://imgtu.com/i/RpH7VS)
[![RpHob8.md.png](https://z3.ax1x.com/2021/06/18/RpHob8.md.png)](https://imgtu.com/i/RpHob8)
[![RpHIDf.md.png](https://z3.ax1x.com/2021/06/18/RpHIDf.md.png)](https://imgtu.com/i/RpHIDf)
