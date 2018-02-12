# Lession 1: 环境设置

## 学习目标

我们将学会：

* 设置调试/开发环境
* 获取XML数据
* 写formats
* 在format里使用`#import`导入可复用的代码
* 安装某个library

-----------------

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

1. 在formats文件夹里创建一个format，并命名为`hello-world`，不要写入任何内容。

2. 试着submit这个空白的format，我们会发现：

   * Cascade 不允许提交完全空白的format
   * 如果要提交format，Cascade要求其中至少包含任意一个字符

   ​