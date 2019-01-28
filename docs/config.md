# 配置

## 配置首个节点

1. 创建配置文件

  在`~/.ssmgr`目录下创建配置文件，支持 yaml 和 json 两种格式，使用 yaml 格式请注意保证正确的缩进

```yaml
type: s
shadowsocks:
  address: 127.0.0.1:6001
manager:
  address: 0.0.0.0:6002
  password: '123456'
db: 'db.sqlite'
```

```json
{
  "type": "s",
  "shadowsocks": {
    "address": "127.0.0.1:6001"
  },
  "manager": {
    "address": "0.0.0.0:6002",
    "password": "123456"
  },
  "db": "db.sqlite"
}
```

2. 运行 shadowsocks

  两种版本的命令有一些差异，都要保证`--manager-address`的参数和上一步配置文件一致

  - libev `ss-manager -m aes-256-cfb -u --manager-address 127.0.0.1:6001`
  - python `ssserver -m aes-256-cfb -p 12345 -k abcedf --manager-address 127.0.0.1:6001`


3. 调用刚刚的配置文件运行 ssmgr

  `ssmgr -c /your/node/config/file`

!> 此处需要让程序后台运行，关于后台运行的方法请参考`pm2`、`byobu`等工具

## 配置并运行Web界面

  创建配置文件，将`1.1.1.1`替换成节点的实际IP地址

```yaml
type: m
manager:
  address: 1.1.1.1:6002
  password: '123456'
plugins:
  flowSaver:
    use: true
  user:
    use: true
  account:
    use: true
  email:
    use: true
    type: 'smtp'
    username: 'username'
    password: 'password'
    host: 'smtp.your-email.com'
  webgui:
    use: true
    host: '0.0.0.0'
    port: '80'
    site: 'http://yourwebsite.com'
    # icon: 'icon.png'
    # skin: 'default'
    # googleAnalytics: 'UA-xxxxxxxx-x'
    # gcmSenderId: '476902381496'
    # gcmAPIKey: 'AAAAGzddLRc:XXXXXXXXXXXXXX'
db: 'webgui.sqlite'
```

!> email 部分用于给注册用户发送验证邮件，请填写正确的参数并选用支持 smtp 的 vps

!> site 字段填写网站的实际访问地址

  调用此配置文件运行：

  `ssmgr -c /your/webgui/config/file`

  若一切正常，便可看到主界面：

![](/_media/home.png)

!> 成功运行后，首个注册用户为管理员



## 配置更多的节点

1. 仿照上文的步骤，创建配置文件，运行`shadowsocks`和`ssmgr`

2. 在管理界面的“服务器”页面，点击右下角“+”按钮，填上对应的地址、端口、密码、加密方式