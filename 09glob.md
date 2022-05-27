# [glob语法](https://www.ruanyifeng.com/blog/2018/09/bash-wildcards.html)

>glob语法最早应用于`linu shell`的一部分,现在在几乎所有的linux系统中都可以使用.(git bash等也可以在windows使用)

* 基础语法：`/`、`*`、`?`、`[]`
* 拓展语法：`**`、`{}`、`()`

```bash
# 生成a1.js,a2.js,b1.js,b2.js
touch {a,b}{1..2}.js
```

## 基础语法

>分隔符和片段

* `src/index.js`有两个片段,分别是`src`和`index.js`
* `src/**/*.js`有三个片段,分别是`src`,`**`和`*.js`

> 单个星号:单个信号`*`用于匹配单个片段中的零个或者多个字符

* `./img/*.png`表示`img`目录下所有以`png`结尾的文件,但是不能匹配到`img`子目录中的文件
* 这时候可以使用`./img/*/*.png`:可以匹配所有img子目录下的`png`结尾的文件(不包括img目录下的png)

```bash
ls ./img/*/*.png
```

>`?`:匹配单个片段中的单个字符

* 匹配单个文件名,例如`a.md`或者`b.md`...

```bash
ls ?.md
```

* 如果要匹配多个字符,例如`index.md`则需要使用`?????.md`,匹配五个字符.
* <span style="background-color:red">注意:`?`不能匹配空字符,他占据的位置必须有字符存在</span>

>`[]`:同样时匹配单个片段中的单个字符,但是字符集只能从括号中选择,如果字符集内有`-`,表示范围

* 该`[]`使用方式和正则一样

   ```bash
   ls test/[c-f]at.js
   #test/cat.js  test/dat.js  test/eat.js  test/fat.js
   ```

* `[!..]`和`[^..]`完全等价:匹配除了括号字符的所有文件

   ```bash
   ls test/[!c-d]at.js
   # test/eat.js  test/fat.js
   ```

## 扩展语法

* `**`/`*`:可以跨片段匹配零个或者多个.这两个在递归目录时并没有本质的区别.`/**/`或者`/*/`

```bash
#匹配img下的所有文件和文件夹
ls ./img/*
```

* `{}`:匹配大括号内的所有模式,并支持使用`..`匹配连休的字符`{start,end}`
  * 即使只有一个模式也需要使用`,`隔开

   ```bash
   # a.png a.jpeg a.jpg
   touch a.{png,jp{,e}g}
   ```

  * `{}`和`[]`的其中一个区别:<span style="color:red">如果匹配的文件不存在,[]会失去模式的功能</span>.变成一个单纯的字符串,但是`{}`依然可以展开

* `()`:小括号必须跟在`?`、`*`、`+`、`@`、`!` 后面使用,且小括号里面的内容是一组以`|`分隔符的模式集合

* `?(pattern|pattern|pattern)`:匹配0次或1次给定的模式
* `*(pattern|pattern|pattern)`:匹配0次或多次给定的模式
* `+(pattern|pattern|pattern)`:匹配1次或多次给定的模式
* `@(pattern|pattern|pattern)`:严格匹配给定的模式
* `!(pattern|pattern|pattern)`:匹配非给定的模式
