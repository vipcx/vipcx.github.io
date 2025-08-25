location /fuint-application/ {
proxy_pass http://localhost:8080/;
client_max_body_size 5m;
proxy_set_header host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}


 location /fuintAdmin{
                    alias   html/fuintAdmin;
                    index  index.html;
                    try_files $uri $uri/ /fuintAdmin/index.html;
                }

 location /fuintCashier{
                    alias   html/fuintCashier;
                    index  index.html;
                    try_files $uri $uri/ /fuintCashier/index.html;
                }




#### 113的部署

server {
    listen 80;
    server_name www.jdc51.com  jdc51.com;
    #rewrite ^(.*)$ https://${host}$1 permanent; 
 location /fuint-application/static {
        alias /home/admintalu/u01/application/fuint/static;
    }

  location /ecapsvc{
                    alias   html/ecapsvc;
                    index  index.html;
                    try_files $uri $uri/ /ecapsvc/index.html;
                }


}





 location /fuint-application/static {
        alias /home/admintalu/u01/application/fuint/static;
    }

location /fuint-application/ {
proxy_pass http://localhost:8080/;
client_max_body_size 5m;
proxy_set_header host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}


 location /fuintAdmin{
                    alias   html/fuintAdmin;
                    index  index.html;
                    try_files $uri $uri/ /fuintAdmin/index.html;
                }

 location /fuintCashier{
                    alias   html/fuintCashier;
                    index  index.html;
                    try_files $uri $uri/ /fuintCashier/index.html;
                }

#### 对应的图片配置

 上传图片本地地址
images.root=/home/admintalu/u01/application/fuint
images.path=/static/uploadImages/

 上传图片服务器域名
images.upload.url=http://www.jdc51.com/fuint-application

 上传图片允许的大小（单位：MB）
images.upload.maxSize=5