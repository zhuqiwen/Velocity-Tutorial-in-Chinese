---
typora-copy-images-to: ../../../../Desktop/Screen Shot 2018-02-09 at 4.22.51 PM.png
---

# Lession 1: 环境设置

## 学习目标

我们将学会：

* 设置调试/开发环境
* 获取XML数据
* 写formats
* 在format里使用`#import`导入可复用的代码
* 安装某个library

===============

### 环境设置

1. 在Cascade里创建一个site；本教程中的site，命名为`velocity-test`

2. 如果这个site有多个用户，为每个用户创建一个文件夹并以用户名为其命名

3. 在用户文件夹里创建一个page，命名为temp；我们会通过复制这个page来创建其他page

4. 至此，文件夹结构如下：

   ```shell
   Base Folder
       wing(folder)
           blocks(folder)
           formats(folder)
           temp(page)
   ```

   ​

### 第一个format

1. 在formats文件夹里创建一个format，并命名为`hello-world`

   ​