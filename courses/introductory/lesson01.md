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
   * 如果要提交format，
     * Cascade要求其中至少包含任意一个非`#`的字符
     * 或者，包含`##`（单行注释）

3. 这里我们先写入`##`

4. 现在，应该可以submit这个format了

5. submit之后，我们可以进一步编辑format的内容：

   1. 写入字符串`Hello, World!`

   2. 点击右上角的`Test Format`按钮

   3. 在`Transformation Result`里，应该可以看到刚才写入format的`Hello, World!`，如图

      * 如果format里没有任何的可执行代码，这些纯字符串就会被显示出来；如果写入的是XHTML，那么这些XHTML就会被原样显示出来

   4. 现在写入一些可执行代码：

      ```velocity
      ## initialize a variable
      #set ($greetings = "Hello, World!")

      ## output the value of the variable
      $greetings
      ```

   5. 点击`Test Format`按钮预览代码执行结果

6. 这四行代码意味着什么？

   1. `## intialize a variable`是一条单行注释；`##`意味着，直到当前行结束，其后的内容都是注释

   2. 因此，单行注释也可以跟在同行的可执行代码后：

      `#set ($greetings = "Hello, World!") ## initialize a variable`

      这样写也是合法的

   3. `#set`或更准确地说`set`，是Velocity的[指令](http://velocity.apache.org/engine/1.7/user-guide.html#velocity-template-language-vtl-an-introduction)

      1. 每条Velocity指令，开头都是`#`符号

      2. `#set`指令用来创建变量或给变量赋值

      3. `$greetings`是变量，变量以`$`符号开头

      4. `"Hellow, World!"`是字符串；在Velocity里，字符串可以用单引号或双引号

      5. `#set ($greetings = "Hello, World!")`是一条可执行的statement

         1. 这条statement创建了一个变量`$greetings`，并以字符串`"Hello, World!"`对其赋值

      6. 变量被定义并赋值之后，只要在可执行代码里提及变量，就可以输出其内容：

         ```velocity
         $greetings
         ```

### 关于Test Format按钮

1. 编辑format的时候，总是可以用这个按钮来预览format的输出；这个输出线是在Transforation Result里。

2. 预览之前，我们也可以通过`Preview Options`来选择某个block和/或某个page，来作为当前format渲染的对象。

3. 当选定某个block和/或某个page后，这个block里的XML内容会显示在asset preview里；这些XML内容与format里的代码不一定有任何关联。

4. 也就是说，如果想要查看任意block的内容，可以先编辑任意format，然后通过`Preview Options`选择目标block来查看它；如果这个block是个index block，可能还需要选择相应的page。

   * 当block是个index block的时候，为了查看相应的page里的XML内容，需要选择这个page，并选择被命为`calling-page`的block（即，关联在这个page的`DEFAULT` region上的block）。
   * 然后，可以将这个page所包含的XML内容，全部或部分地复制到某个XML blcok里
   * 是通过这种方式来查看page里的xml内容么？换言之，需要新建一个非index的block，用其来展示XML的内容？也就是说，总是需要一个非index的block来作为format的数据源？待考证。

5. `Transformation Result`将显示这个format的全部输出，包括XHTML标签。

6. 注意！换行符也会被输出。

7. 如果要移除换行符，只需要在一行的末尾加上`##`，比如：

   ```velocity
   $greetings##
   ```

### 关联一个format到一个page

1. 创建一个page，命名为`hello-world`；这里可以直接copy前面新建的temp page。
2. 编辑`hello-world` page，将`hello-world` format关联到这个page的某个region上。
3. 现在，存储在`$greetings`变量里的字符串（“Hello, World!”）会显示在这个page上。
4. 注意！即便没有block，format也可以输入内容，这些内容里也可以包含XHTML标签。
5. 原则上来说，一个format里可以同时包含整个页面的内容（及其XHTML标签）。

### 创建一个XML Block

1. 在`blocks`文件夹里，新建一个XML block，命名为`hello-world`。

2. 在其中写入如下内容：

   ```velocity
   <greetings>
   Hello, World from the XML block!
   </greetings>
   ```

   * 注意！这里我们有意识地在`<greetings>`标签里放入了不同于变量`$greetings`的内容

3. 现在再次编辑`hello-world` page，将这个block关联到前述format所在的相同region。

4. 此时，page上显示的应该仍旧是`Hello, World!`，而不是`Hello, World fromt he XML block!`。

   * 为什么会这样呢？很简单，因为当前关联的这个format里的code，没有任何部分用于处理这个block的内容：

     ```velocity
     ## initialize a variable
     #set ($greetings = "Hello, World!")

     ## output the value of the variable
     $greetings
     ```

     这段代码所做仅仅是设立了一个变量、给它赋值、然后打印这个变量的内容。

5. 那么，要在page里显示这个block的内容，最简单的办法就是取消当前format和这个region的关联。现在，page上会显示`Hello, World from the XML block!`。

6. 结论：当page上的某个region关联了一个block，同时也关联了一个format，如果这个format里没有任何用于处理这个block的代码，这个block就会被忽略掉

   * 换言之，当page寻找可显示的内容时：
     1. 先找format
     2. format找block
     3. format看看自己的code有没有用来处理block的
        1. 有，block —> format —> page
        2. 没有，format —> page

### 一些注意事项

1. 命名blocks、formats、以及test pages的时候，使用有足够描述性的名称。
2. format的名称应该总是和与其关联的test page的名称一致。
3. 与XSLT format不同，含有可执行代码的velocity format并不总是需要block为其提供内容（参看前一小节）。
4. 除非format输出的内容已经被格式化为table或list或其他类型，应该总是将输出的内容放在`<pre>`标签里，以便于方便地察看输出。
5. 如果一个format并不针对与其关联的block，那么这个block里的内容将被忽略。

### 继续了解velocity format

1. 多行注释

   ```velocity
   #*
   This is a multi-line comment.
   It can occupy several lines, starts with # followed by *,
   and ends in * and #(without spaces).
   *#
   ```

2. 多行注释内部不能包含任何`#*`或`*#`；`##`则是被允许的。

3. 对于format的内容，常见的规则和命名惯例都是适用的。此外，变量可以包含连字符（hyphens - ），但是`$`后紧跟的第一个字符必须是字母。

4. 字符串内包含引号的一般规则也适用。

5. 如果某个变量没有经过`#set`定义，并且被单独使用，则其输出就是它自身的字符串形式，比如：

   * `$var`在没有用`#set`定义，也没有与其他变量或语句发生交互的情况下，其输出就是`"$var"`，四个字符而已。

6. 记住！除了语法错误，velocity一般不会提示错误。

7. 若单独使用（即不与format里的内容发生交互），format里任意位置的XHTML元素或字符串都会被原样输出。

8. Velocity是基于Java的。

9. （因此），Velocity代码是大小写敏感的。

### 在字符串里对引号做转义

1. 字符串要么用一对单引号要么用一对双引号包裹。

2. 这一对单引号或双引号，是代码的一部分，不是字符串的一部分。

3. 在字符串内部，可以使用引号。

4. 一对单引号内部的单引号，必须被转义。

   * 用两个连续的单引号来转义：`''`转义的是第二个单引号。

5. 一对双引号内部的双引号，必须被转义。

   * 用两个连续的双引号来转义：`""`转义的是第二个双引号。

6. 例子：

   ````velocity
   ## single quotes within double quotes are OK
   #set($str1 = "Pete's code")

   ## double quotes within single quotes are OK
   #set($str1 = 'What do you mean by "simple"?')

   ## to escape single quotes within single quotes
   #set($str1 = 'Pete''s code')

   ## to escape double quotes within double quotes
   #set($str1 = 'what do you mean by ""simple""?')
   ````

### 获取XML数据

1. script format的一个用途就是格式化显示block里的数据。
2. 一个block可以是：
   * 一个index block
   * 一个data definition block
   * 一个XML block
   * 一个feed block
3. Text blocks不需要formats。
4. block总是应该和format成对地关联到同一个page region上。
5. Faked data：只需要将相应的数据放在一个XML block里；index block里的数据也可以被copy到XML block里。
6. faked data对测试非常有用：我们可以自己写一些数据，或者从index block里拷贝一些复杂的nodes然后简化一下，或者直接写一个element来触发format里的可执行代码。

### 安装一个库

1. 使用`#import`指令来导入可复用的代码。

2. UPState提供了一个可复用的[Velocity library][https://github.com/wingmingchan/velocity/tree/master/library]

3. 可以[`Clone or download`][https://github.com/wingmingchan/velocity]整个velocity库。

4. 在下载的zip文件里，可以在`library`文件夹里找到这些库文件。

5. 也可以通过（在Github上）查看每个format的代码，拷贝其源文件。

   * 查看代码的时候，确保以`Raw`格式查看，这样才能直接从浏览器里拷贝。

6. 我们应该在Cascade建立一个专门的site来存储可复用的资源。

   1. 首先，假设我们有这样一个专门的site，命名为`_brisk`。
   2. 在根目录下创建一个文件夹，命名为`core`。
   3. 在`core`里创建一个文件夹，命名为`library`。
   4. 在`library`里创建一个文件夹，命名为`velocity`，也可以在这里再创建一个`xslt`文件夹。
   5. 在`velocity`里创建一个`chanw`文件夹。
   6. 将需要复用的format库放在`chanw`文件夹里。
      * `Add Content` —> `file`，然后`choose`这些库文件（目前为止，这样做仅能上传这些库文件，Cascade 8似乎不能够正确识别这些库文件）。

7. 为了在我们的本地formats里复用这些库，我们需要导入一个入口库文件，`chanw_library_import`

   1. 在`formats`文件夹里新建一个format，命名为`import-library`。

   2. 在其中写入下列代码：

      ```velocity
      #import("site://_brisk/core/library/velocity/chanw/chanw_library_import")
      ```

   3. 如有必要，可以改变site的名称。

8. 点击`Test Format`。

9. 如果所有这些库安装正确，此时在`Transformation result`里应该会显示几个空白行。

10. 然后，继续编辑`import-library` format，加入：

    ```velocity
    $chawLocalTimeNow
    ```

11. 如果本机时间（local time）成功显示，则说明这个库已经部署成功。