# 

## Hugo有关的坑

### html代码在.md文件中无法显示

通过查询[Hugo 0.60.0 更新日志](https://gohugo.io/news/0.60.0-relnotes/)得知内嵌的html代码已经不能正常显示了，所以需要在config.toml中加入如下片段

```
[markup]
  [markup.goldmark]
    [markup.goldmark.renderer]
      unsafe = true
```



## LoveIt主题有关的坑

### 无法正常启动

需先将 `/themes/LoveIt/exampleSite/` 下的文件拷贝至你的博客根目录



### 评论无法正常显示

🌐 [运行hugo前需要设定 `HUGO_ENV=production` 使得评论有效](https://github.com/liuzc/LeaveIt#set-production-environment-when-generating-site)



### valine评论无法去除算数验证

需同时设定notify和verify为false



### 无法修改mian.mass

🌐 [LoveIt主题需要安装hugo-extended才能正常编译](https://github.com/liuzc/LeaveIt#hugo-extended-sassscss-version-required)







