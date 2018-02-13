# Lesson 2: Java 以及Java对象

## 学习目标：

我们将理解：

* 在Velocity环境下，一个对象的本质
* 为什么Java API文档会和Velocity相关

我们将学会：

* 获取某个对象的相关信息
* 阅读Java API文档
* 使用Java对象

-------

### Region中的Block和Format

1. 回到`hello-world` page，确保`hello-world` block和`hello-world` format被关联到这个page上的同一个region。
2. 编辑`hello-world` format，删除所有内容，只保留`##`。
3. 现在，尽管在这个page里关联了一个XML block， 但这个page不显示任何内容，因为与其关联的同名format里没有任何与这个block相关的code。
4. 如果我们取消这个format与当前page的关联，则page会显示XML block里的内容。
5. 亦即，如果一个page region关联了一个format和一个block，决定page输出的是这个format，与block的内容无关。

### Java在Velocity里扮演的 角色

1. Velocity基于Java。

2. 在Velocity环境下，几乎所有的东西都是Java对象。

3. 合法地给一个变量赋值，实际上就是将这个变量和一个Java对象关联起来。

4. 每个Java对象都有类型信息（比如这个对象的类名）。

   * 例如，当我们将字符串`Hello, World!`赋值给变量`$greetings`，这个变量的类型就成了`java.lan.String`。

5. 因此，我们可以用Java 的方式来调用对象方法或静态方法，来操纵相关联的数据：

   ```Velocity
   #set( $greeingts = "Hello, World!" )
   $greetings.charAt(0) ##=> H
   ```

6. 还可以获取对象属性：

   ```velocity
   $greetings.Class ##=> class java.lang.String
   ```

7. 待续