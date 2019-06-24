[let's encrypt]:https://letsencrypt.org/images/letsencrypt-logo-horizontal.svg
![let's encrypt]
# ssl-cert
使用免费的let's encrypt 生成ssl证书以及续签证书
## 安装acme.sh
```acme.sh
curl https://get.acme.sh | sh
下载acme.sh包,在此过程中若出现：curl: (35) SSL connect error报错处理，请执行命令：yum -y update nss即可
执行命令后出现如下信息则表示安装成功:
[2019年 06月 24日 星期一 11:01:40 CST] Installing from online archive.
[2019年 06月 24日 星期一 11:01:40 CST] Downloading https://github.com/Neilpang/acme.sh/archive/master.tar.gz
[2019年 06月 24日 星期一 11:03:07 CST] Extracting master.tar.gz
[2019年 06月 24日 星期一 11:03:08 CST] It is recommended to install socat first.
[2019年 06月 24日 星期一 11:03:08 CST] We use socat for standalone server if you use standalone mode.
[2019年 06月 24日 星期一 11:03:08 CST] If you don't use standalone mode, just ignore this warning.
[2019年 06月 24日 星期一 11:03:08 CST] Installing to /root/.acme.sh
[2019年 06月 24日 星期一 11:03:08 CST] Installed to /root/.acme.sh/acme.sh
[2019年 06月 24日 星期一 11:03:08 CST] Installing alias to '/root/.bashrc'
[2019年 06月 24日 星期一 11:03:08 CST] OK, Close and reopen your terminal to start using acme.sh
[2019年 06月 24日 星期一 11:03:08 CST] Installing alias to '/root/.cshrc'
[2019年 06月 24日 星期一 11:03:08 CST] Installing alias to '/root/.tcshrc'
[2019年 06月 24日 星期一 11:03:09 CST] Installing cron job
50 0 * * * "/root/.acme.sh"/acme.sh --cron --home "/root/.acme.sh" > /dev/null
[2019年 06月 24日 星期一 11:03:09 CST] Good, bash is found, so change the shebang to use bash as preferred.
[2019年 06月 24日 星期一 11:03:10 CST] OK
[2019年 06月 24日 星期一 11:03:10 CST] Install success!
```
## 生成证书
* 前提：需要在腾讯云购买一个域名
* cme.sh 实现了 acme 协议支持的所有验证协议. 一般有两种方式验证: http 和 dns 验证，这里仅介绍 DNS 的方式
* acme.sh 支持直接使用主流 DNS 提供商的 API 接口来完成域名验证以及一些相关操作。
* 我们登录到：[https://www.dnspod.cn/](https://www.dnspod.cn/),使用腾讯云账户登录，打开控制台，进入左侧菜单-用户信息-安全设置进行创建API TOKEN,我这边创建成功后得到的信息是如下：
``` api token
名称: dns_tencent_cloud
ID:108415 #这里我修改过
Token:623458e5c742927e903ec41cd9993108 #这里我修改过
创建时间: 2019-06-24 10:23:45
这里需要的是ID和Token值
```
* 这里以腾讯云为例：
```
1.1 cd /root/.acme.sh #进入该目录
1.2 设置环境变量：
    export DP_Id="108415"
    export DP_key="623458e5c742927e903ec41cd9993108"
1.3 acme.sh --issue --dns dns_dp -d lilyg.cn -d *.lilyg.cn #执行验证txt解析记录及生成证书，lilyg.cn是我在腾讯云上购买的域名
执行命令后出现如下信息则表示成功:
[2019年 06月 24日 星期一 11:19:22 CST] Create account key ok.
[2019年 06月 24日 星期一 11:19:22 CST] Registering account
[2019年 06月 24日 星期一 11:19:24 CST] Registered
[2019年 06月 24日 星期一 11:19:24 CST] ACCOUNT_THUMBPRINT='57D1LkeacvHrqFxdfB4QzOhj2tYYkkyqZbzq4O8T8qs'
[2019年 06月 24日 星期一 11:19:24 CST] Creating domain key
[2019年 06月 24日 星期一 11:19:24 CST] The domain key is here: /root/.acme.sh/lilyg.cn/lilyg.cn.key
[2019年 06月 24日 星期一 11:19:24 CST] Multi domain='DNS:lilyg.cn,DNS:*.lilyg.cn'
[2019年 06月 24日 星期一 11:19:24 CST] Getting domain auth token for each domain
[2019年 06月 24日 星期一 11:19:26 CST] Getting webroot for domain='lilyg.cn'
[2019年 06月 24日 星期一 11:19:26 CST] Getting webroot for domain='*.lilyg.cn'
[2019年 06月 24日 星期一 11:19:27 CST] Adding txt value: BEkewifccRzSv4bfBFAopN-Mwg7Seq-Y2lO_DyyFZ74 for domain:  _acme-challenge.lilyg.cn
[2019年 06月 24日 星期一 11:19:27 CST] Adding record
[2019年 06月 24日 星期一 11:19:28 CST] The txt record is added: Success.
[2019年 06月 24日 星期一 11:19:28 CST] Adding txt value: 3sFiKe9hfcgEQHysswXo9daJglYt7_pksvlyhzy1h4I for domain:  _acme-challenge.lilyg.cn
[2019年 06月 24日 星期一 11:19:28 CST] Adding record
[2019年 06月 24日 星期一 11:19:29 CST] The txt record is added: Success.
[2019年 06月 24日 星期一 11:19:29 CST] Let's check each dns records now. Sleep 20 seconds first.
[2019年 06月 24日 星期一 11:19:50 CST] Checking lilyg.cn for _acme-challenge.lilyg.cn
[2019年 06月 24日 星期一 11:19:51 CST] Domain lilyg.cn '_acme-challenge.lilyg.cn' success.
[2019年 06月 24日 星期一 11:19:51 CST] Checking lilyg.cn for _acme-challenge.lilyg.cn
[2019年 06月 24日 星期一 11:19:52 CST] Domain lilyg.cn '_acme-challenge.lilyg.cn' success.
[2019年 06月 24日 星期一 11:19:52 CST] All success, let's return
[2019年 06月 24日 星期一 11:19:52 CST] Verifying: lilyg.cn
[2019年 06月 24日 星期一 11:19:56 CST] Success
[2019年 06月 24日 星期一 11:19:56 CST] Verifying: *.lilyg.cn
[2019年 06月 24日 星期一 11:20:00 CST] Success
[2019年 06月 24日 星期一 11:20:00 CST] Removing DNS records.
[2019年 06月 24日 星期一 11:20:00 CST] Removing txt: BEkewifccRzSv4bfBFAopN-Mwg7Seq-Y2lO_DyyFZ74 for domain: _acme-challenge.lilyg.cn
[2019年 06月 24日 星期一 11:20:01 CST] Removed: Success
[2019年 06月 24日 星期一 11:20:01 CST] Removing txt: 3sFiKe9hfcgEQHysswXo9daJglYt7_pksvlyhzy1h4I for domain: _acme-challenge.lilyg.cn
[2019年 06月 24日 星期一 11:20:04 CST] Removed: Success
[2019年 06月 24日 星期一 11:20:04 CST] Verify finished, start to sign.
[2019年 06月 24日 星期一 11:20:04 CST] Lets finalize the order, Le_OrderFinalize: https://acme-v02.api.letsencrypt.org/acme/finalize/59867796/608694814
[2019年 06月 24日 星期一 11:20:06 CST] Download cert, Le_LinkCert: https://acme-v02.api.letsencrypt.org/acme/cert/03f73b378acb26c6ac20443fab3b0b974b4a
[2019年 06月 24日 星期一 11:20:08 CST] Cert success.
[2019年 06月 24日 星期一 11:20:08 CST] Your cert is in  /root/.acme.sh/lilyg.cn/lilyg.cn.cer 
[2019年 06月 24日 星期一 11:20:08 CST] Your cert key is in  /root/.acme.sh/lilyg.cn/lilyg.cn.key 
[2019年 06月 24日 星期一 11:20:08 CST] The intermediate CA cert is in  /root/.acme.sh/lilyg.cn/ca.cer 
[2019年 06月 24日 星期一 11:20:08 CST] And the full chain certs is there:  /root/.acme.sh/lilyg.cn/fullchain.cer 
证书默认放到当前目录下的lilyg.cn目录
```
## 拷贝证书到指定目录
```
 cp /root/.acme.sh/lilyg.cn/fullchain.cer /data/certs/ssl/letencrpt/  #/data/certs/ssl/letencrpt/为以后配置nginx证书目录
 cp /root/.acme.sh/lilyg.cn/lilyg.cn.key /data/certs/ssl/letencrpt/
```
## 配置nginx
``` nginx.conf
server {
    listen 80;
    server_name www.lilyg.cn test.lilyg.cn; #填写绑定证书的域名
    rewrite ^(.*)$ https://$host$1 permanent; #把http的域名请求转成https
  }

  server {
     listen 443;
     server_name www.lilyg.cn test.lilyg.cn; #填写绑定证书的域名
     ssl on;
     ssl_certificate /data/certs/ssl/letencrpt/fullchain.cer;#证书文件名称
     ssl_certificate_key /data/certs/ssl/letencrpt/lilyg.cn.key;#私钥文件名称
     ssl_session_timeout 5m;
     ssl_protocols TLSv1 TLSv1.1 TLSv1.2; #请按照这个协议配置
     ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;#请按照这个套件配置
     ssl_prefer_server_ciphers on;
     root  /data/www/wwwroot/elasticsearch-php/;
     location / {
                  set $pass_host '';
                  set $pass_url '';
                  set $pass_body '';
                  set $pass_referer '';
                  set $pass_cookie '';
                  set $pass_proxy '';
                  set $pass_user_agent '';
                  set $pass_origin '';
                  set $pass_http '';
                  set $pass_exinfo '';
                  set $pass_content_type '';
                  set $pass_authorization '';          
      
          index   index.php index.html index.htm;
          
          if (!-e $request_filename) {
              rewrite ^/(.*)$ /index.php/$1 last;
          }   
      }
       
  
      location ~ .*\.php {
          include fastcgi_params;
          fastcgi_pass   127.0.0.1:9000;
          fastcgi_index index.php;
          fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
          fastcgi_split_path_info  ^(.+\.php)(/.*)$;  
          fastcgi_param  PATH_INFO $fastcgi_path_info;
      }
  }
```
## 设置续签后，自动更新证书到执行目录并重启nginx
```
acme.sh --install-cert -d lilyg.cn \
--key-file       /data/certs/ssl/letencrpt/lilyg.cn.key  \
--fullchain-file /data/certs/ssl/letencrpt/fullchain.cer \
--reloadcmd     "/usr/local/nginx-1.13.7/sbin/nginx -s reload" 
#--reloadcmd 表示更新证书后重启，具体nginx配置路径以您自己配置的为准
```
## 说明
说明：本文参考链接：[https://github.com/Neilpang/acme.sh](https://github.com/Neilpang/acme.sh),[https://www.jianshu.com/p/dbe180979e77](https://www.jianshu.com/p/dbe180979e77)
