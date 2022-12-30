
```swift
find ./ -name "*.meta" | xargs rm -rf
```

删除当前目录下所有后缀名为 .meta 文件  
find . -name "*.meta" |xargs rm -rf  
格式很简单，如下：  
find 目录 -name 名称|xargs rm -rf  
查找你要删除的文件夹或者文件，然后删除即可。  
但是在macos下有一个问题，文件夹中有空格是不能删除的。