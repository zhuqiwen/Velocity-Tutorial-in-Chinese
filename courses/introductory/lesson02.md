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

   * 如果我们想要知道当前对象有哪些方法，获取其类名尤为重要

7. 通过上述例子，我们知道，可以用Java的句法来调用对象方法或获取其属性（即`.`操作符）

8. 几点注意事项：

   1. 即便是调用静态方法，也需要这个方法所属的类的一个对象实例。除非我们使用反射机制。

   2. 向方法传递多个参数值的时候，需要用到逗号。

      ```velocity
      ## create a LinkedHashMap object
      #set( $states = {} )
      ## add anentry by invoking the put method
      #set( $void = $states.put('NY', 'New York'))
      ```

   3. 不能直接将运算作为参数传递给方法：

      ```velocity
      ## this will cause an error
      $greetings.charAt( 1 + 2)
      ```

### 引用（References）

1. Velocity中有三种[引用][http://velocity.apache.org/engine/devel/user-guide.html#references]:
   1. [变量][http://velocity.apache.org/engine/devel/user-guide.html#variables]
   2. [属性][http://velocity.apache.org/engine/devel/user-guide.html#properties]
   3. [方法][http://velocity.apache.org/engine/devel/user-guide.html#methods]

### 属性和方法

1. 在Velocity中，一个Java属性形如：

   ```velocity
   $var.PropertyName
   ```

2. 在Apache的官方手册里，一个属性总是以`Pascal Case`方式命名（首字母大写驼峰式），尽管`camelCase`也是可以接受的（首字母小写驼峰式）：`$greetings.Class`

3. 属性名一般是某个getter的shorthand形式

   * 比如：`$greetings.Class`之于`$greetings.getClass()`，以及`$greetings.Class.Name`之于`$greetings.getClass().getName()`
   * 尽管属性名的第一个字符可以是大写或小写，属性名的其他部分则是大小写严格敏感的。

4. 在Velocity中，方法调用形如：

   ```velocity
   $x.methodName( argList )
   ```

5. `$greetings.getClass()`即是一个通过`java.lang.String`对象调用方法的例子。

6. 注意！属性名和方法名都是大小写敏感的：`$d.getDisplayName()`对应的属性名即可以是`$d.displayName`，也可以是`$d.DispalyName`，但不应该是`$d.displayname`；换言之，`N`必须大写。

7. 按照Apache官方推荐，只要一个对象具有某个属性，就应该直接引用这个属性而不需要调用其getter。

###获取对象信息：使用`$x.Class.Name`

1. 获取一个变量对应的对象的相关信息，我们使用`$x.Class.Name`（或`$x.class.name`）；`$x`需通过`#set`指令定义：

   ```velocity
   #set( $greetings = "Hello, World!" )
   $greetings.Class.Name
   ```

   * 上述代码输出`java.lang.String`，表明与`$greetings`变量关联的，是一个`java.lang.String`对象。

2. 任何变量，只要有合法值，都可以应用这个方法

3. 如果一个变量没有被定义，并且被单独使用，比如`$var`，那么应用这个方法后，只会输出`$var.Class.Name`（字符串）

4. 总的来说，如果一个变量名没有被赋予合法值，或一个宏名没有包含任何代码，则同样只会原样输出字符串。

5. 在后面的章节里，我们会使用`$x.Class.Name`来测试变量是否被定义过。

### 如果变量`$x`的值不是对象

1. 首先，在Java里，几乎所有的东西都是对象。
2. 但是也有例外。Java有8种原始类型（不继承自Object类），其中包括`int`，`double`等等，此外，Java的arrays也不是对象。
3. 一般来说，我们不会和这些非对象类型打交道，因为他们几乎总是被打包成对象。
   1. 比如：`int`值被包裹在`java.lang.Integer`对象里
   2. `#set( $int = 1 )`，`$int`的值是一个`java.lang.Integer`对象。
4. 但是，某些方法的返回值是arrays
   1. 比如：`com.hannonhill.cascade.api.adapters.PageAPIAdapter.getStructuredData`返回的就是一个由`com.hannonhill.cascade.api.asset.common.StructuredDataNode`对象构成的array。
   2. 在这种情况下，即某个变量`$x`的值是个array，应用`$x.Class.Name`回得到一个字符串，形如`[LX;`
   3. 这个字符串由3部分构成：`[L`，`X`，和`;`，其中，`X`即对象的类名
   4. 比如，`com.hannonhill.cascade.api.adapters.PageAPIAdapter.getStructuredData`返回`[Lcom.hannonhill.cascade.api.asset.common.StructuredDataNode;`
   5. 其含义是，`getStructuredData`方法返回的是由`com.hannonhill.cascade.api.asset.common.StructuredDataNode`构成的array

### Velocity环境

1. Velocity环境实际上是一种数据的载体，其位于Java层和template层之间（参考[Developer Guide][http://velocity.apache.org/engine/devel/developer-guide.html]）
2. 这个载体由一个类表述
3. 这个载体只含有对象
4. 在Cascade Velocity中的对象有：
   * `$contentRoot`：指向相关block中XML的根结点（这个block实际上是一个`org.jdom.Element`对象）
     * 对data definition block而言，其根节点名称为`system-data-structure`
     * 对index block而言，其根节点名称为`system-index-block`
     * 对feed block而言，其根节点名称与关联feed根节点一致
     * 对XML block而言，其根节点名称与其包含的XML根节点一致
     * 如果其中没有根节点，比如一个text block，则其XML内容会被包裹在`<system-xml>`根节点内
   * `$currentPage`：只想当前page；是一个`[com.hannonhill.cascade.api.adapters.PageAPIAdapter](http://www.upstate.edu/formats/velocity/api-documentation/com-hannonhill-cascade-api-adapters/page-api-adapter.ph)`对象
   * [`$_DateTool`][]
   * [`$_XPathTool`][]
   * [`$_SerializerTool`][]
   * [`$_SortTool`][]
   * [`$_StringTool`][]
   * [`$_DisplayTool`][]
   * [`$_EscapeTool`][]
   * [`$_MathTool`][]
   * [`$_NumberTool`][]
   * [`$_NumberTool`][]
   * [`$_FieldTool`][]
   * [`$_`][](Locator Tool)
   * [`$_PropertyTool`][]
   * `$currentPagePath`：是一个`java.lang.String`对象，其值为当前page的路径
   * `$currentPageSiteName`：同上，其值为当前page所在site的名称
   * 其他Java对象