ping + ip： 查看某一个ip地址是否能够连通，如： ping 164.80.77.163(或者ping 域名)
telnet ip port ： 查看某一个机器上的某一个端口是否可以访问，如：telnet 164.80.77.163 8088

# window

```python
del /a /f /s /q "*.meta" "*.meta.*"
```

该指令可删除后缀为 **xxx.meta** **xxx.meta.xxx** 的文件  
*为通配符  
/a /f 是强制删除所有属性的文件  
/q是无需确认直接删除  
要是再加上/s开关，就可以删除子文件加中的文件
