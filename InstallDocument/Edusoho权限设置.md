##文件目录权限问题


#####对于linux服务器环境，当用户部署完Edusoho系统后准备安装的时候经常会遇到只显示空白页或者出现权限不足不能安装的提示，如下图：

![1-1](http://7xjexf.com1.z0.glb.clouddn.com/安装权限图片.png)

这主要是由于用户没有足够的权限去访写入修改我们系统的某些配置文件和缓存文件，我们需要给系统目录赋予`www-data`用户操作权限，做法如下：

------

###第一步：定位到Edusoho目录

   打开Terminal，使用`cd` 命令定位到Edusoho的根目录，路径以客户的实际目录为准，命令实例：
   
```python
cd /var/www/edusoho
```

###第二步：给Edusoho目录`www-data`用户权限
    
   在Edusoho目录下使用命令：
    
```python
sudo chown -R www-data:www-data ./
```

###当然也可以直接跳过第一步，直接执行：

```python
sudo chown -R www-data:www-data /var/www/edusoho
```

   `/var/www/edusoho`为Edusoho系统的绝对路径，请根据实际目录做相应修改。

------

####到此为止，当我们再次安装我们的网站的时候就没有报错啦~