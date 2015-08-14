```

listen 80;
  server_name bellorchid.xyz;
  root  /var/www/WordPress/; #此处根据你网站实际目录添加

location / {

  try_files $uri $uri/ /index.php?$args;

}
rewrite /wp-admin$ $scheme://$host$uri/ permanent;

location ~* \.(js|css|png|jpg|jpeg|gif|ico)$ {

  expires 24h;

  log_not_found off;

}

rewrite /files/$ /index.php last;


set $cachetest “$document_root/wp-content/ms-filemap/${host}${uri}”;

if ($uri ~ /$) {

  set $cachetest “”;

}

if (-f $cachetest) {

  rewrite ^ /wp-content/ms-filemap/${host}${uri} break;

}

if ($uri !~ wp-content/plugins) {

  rewrite /files/(.+)$ /wp-includes/ms-files.php?file=$1 last;

}

if (!-e $request_filename) {

  rewrite ^/[_0-9a-zA-Z-]+(/wp-.*) $1 last;

  rewrite ^/[_0-9a-zA-Z-]+.*(/wp-admin/.*\.php)$ $1 last;

  rewrite ^/[_0-9a-zA-Z-]+(/.*\.php)$ $1 last;

}
location ~ \.php$ {

  try_files $uri =404;

  fastcgi_split_path_info ^(.+\.php)(/.+)$;

  include fastcgi_params;

  fastcgi_index index.php;

  fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;

  fastcgi_pass php;

}
```

```
server {
       listen 8080;
       server_name bellorchid.xyz;
       root  /var/www/WordPress/; #此处根据你网站实际目录添加

       #wordpress nginx的配置处--start
       location / {
           try_files $uri $uri/ /index.php?q=$uri&$args;
       }

     if ($host != 'bellorchid.xyz' ) {
   rewrite ^/(.*)$ http://bellorchid.xyz/$1 permanent;
 }

 if (!-e $request_filename) {
   rewrite (.*) /index.php last;
 }
       #wordpress nginx的配置处--end

 location ~* \.(gif|jpg|png|swf|bmp|jpeg|flv)$ {
   valid_referers none blocked *.bellorchid.xyz *.qq.com *.weibo.com *.baidu.com;
   if ($invalid_referer) {
     #rewrite ^/ http://www.54ux.com/themes/default/images/logonew.png;
     return 404;
   }
 }

       include server.conf;  

   }
```
