---
layout: post
title: "理解 Ruby 的闭包"
date: 2012-11-20 22:32
comments: true
categories:
- Coding
tags:
- Ruby

---

非直译，原文： [Understanding Ruby Blocks, Procs and Lambdas](http://www.robertsosinski.com/2008/12/21/understanding-ruby-blocks-procs-and-lambdas/)

Ruby 处理 [closures](http://en.wikipedia.org/wiki/Closure_%28computer_science%29) (闭包) 的方式很独特。闭包在 Ruby 里面有 4 种不同的方式，每种方式跟其余几种都有那么一点点的区别，下面就来看看……

首先， Blocks
----
最常见的，最简单的，而且被大多人认为是最具有 Ruby 风格的闭包使用方式是 blocks . 像这样：

{% gist 4120210 Ruby_blocks_example_1.rb %}

这段程序做了什么呢？

1. 定义了一个数组 `array` .
2. 把 `collect!` 这个方法连同它后面的代码块传入这个数组 .
3. 这个代码块找到 `collect!` 这个方法用的变量 (也就是 `n` )，然后对其进行操作 (在这里是平方)。
4. 每个 `n` 都操作一遍，得到最后的结果。

跟 `collect!` 使用一个 block 看起来很容易，只需想像成 `collect!` 这个方法把代码块在数组里面每个元素上都过了一遍。但是如果我们要自己写这个方法呢？假设这个方法名字叫 `iterate!` 的话，写出来是这样的：

<!-- more -->

{% gist 4120210 Ruby_blocks_example_2.rb %}

我们打开 `Array` 这个类，在里面定义 `iterate!` 方法，然后像上面使用 Ruby 内置的 `collect!` 方法一样使用它。注意看 `iterate!` 方法的定义， ( `self[i] = yield(n)` ), 不需要指定将要传入的代码块名字，只需使用 `yield` 这个关键字，调用这个关键字就会执行传入的代码块 . 另外，注意 `n` 这个变量是如何传入 `yield` 的。

1. 将 `iterate!` 方法传入某个数组。
2. 当遇到 `yield(n)` 调用时，将 `n` 传给后面给定的代码块 .
3. 代码块对 `n` 进行操作 (此处是平方)，由于此操作就是代码块最后一步，得到的数值自动成为返回值。
4. `yield` 把这个返回值回输到方法中，重写数组中对应的值。
5. 数组中的每个元素都如此操作一番。

使用 blocks 让我们能非常灵活的与方法交互，改动代码块可以实现对数组的各种不同操作。然而用 `yield` 调用 blocks 只是闭包的其中一种实现方式，下面是以上功能的另一种实现方法：

{% gist 4120210 Ruby_procs_example_1.rb %}

看起来没什么区别嘛，其实有两处：第一，定义的方法 `iterate!` 需要传入一个参数 `&code` . 这个参数所指代的就是代码块；第二，上次使用 `yield` 的地方这次使用的是 `call` 方法带着参数 `n` 去调用代码块。尽管有这两处区别，但是运行结果是完全一样的。

坑爹啊！为啥同样的功能要有两种写法来混淆视听呢？

首先看看所谓的 block 到底是什么东西，这样看：

{% gist 4120210 What_is_a_block.rb %}

原来一个 block 其实就是一个 Proc 啊！可是 Proc 又是什么呢？

Procedures, AKA, Procs
----
Blocks 使用方便，写起来也很简单，然而有时候我们需要在很多不同的地方应用相同的代码块，使用 blocks 的话则只能一遍又一遍的重复写，鉴于 Ruby 是完全的面向对象语言，我们有更优雅的解决方式 - 把需要重复使用的代码块存成一个 object , 现在这段代码就是一个「过程」(Procedure)，缩写为 Proc .

Blocks 和 Procs 的唯一区别是： blocks 没法保存；而 Procs 则可以存下来多次使用。换句话说， 一个 block 就是一个只能用一次的 Proc . 

举例看看 Proc 的复用：

{% gist 4120210 Ruby_procs_example_2.rb %}

看这里我们把一个代码块存在名字叫 `square` 的 object 里面，然后分别用在两个数组上。由于 blocks 使用的时候没有地方可以让我们传入代码块名，所以无法复用；而 Procs 允许我们传入一个代码块的名字 (本例中为 `square` )，就可以实现代码复用。

不事先把代码块存起来，而直接在方法后面跟 Proc 的代码也是可以的，如下例：

{% gist 4120210 Ruby_procs_example_3.rb %}

实际上，这才是大多数编程语言处理闭包的方式，显然这种写法不如 blocks 优雅，但其实 blocks 归根到底就是上面这东西。

除了代码复用， Procs 还有一项能力 blocks 难以做到，那就是我们只能往一个方法传入一个 block ，但是可以传入一个以上的 Procs ，像这样：

{% gist 4120210 Ruby_procs_example_4.rb %}

所以，如果你希望闭包代码能够复用，又或者你需要往方法里面传入多个闭包，那就用 Procs , 别的情况 blocks 就好了。

Lambdas
----

Ruby 语言里面的 Procs 的行为非常像别的语言里面的[匿名函数](http://en.wikipedia.org/wiki/Anonymous_function) (又称为 Lambda 方法)，然而 Ruby 似乎生怕自己不够复杂没有把大家绕晕，她除了 Procs 以外居然也还有个货真价实的 lambdas , 用起来是这样的：

{% gist 4120210 Ruby_lambdas_example_1.rb %}

看起来不是跟 Procs 一模一样嘛！ Matz 你不要太过分！

其实呢 Procs 和 lambdas 有那么两小点微妙的区别：

第一点： Procs 不检查传入代码块的参数数量，而 lambdas 是要检查的。像这样：

{% gist 4120210 Ruby_lambdas_example_2.rb %}

在 Proc 的例子中，多出来的参数就设成 nil 了，而 lambda 则直接抛错。

第二点： 一个方法中的 Proc , 碰到代码块里面有 `return` 语句的话，会返回 `return` 语句的值，然后中断这个方法的执行；而一个方法中的 lambda , 碰到代码块中有 `return` 语句的话，会返回 `return` 语句的值，但不中断方法的执行，示例如下：

{% gist 4120210 Ruby_lambdas_example_3.rb %}

`proc_return` 这个方法执行到 `Proc.new` 这一句，在 Proc 的代码块中碰到了第一个 `return` , 就被中断了，第一个 `return` 语句的值就是整个方法的返回值；`lambda_return` 这个方法在 lambda 的代码块中碰到第一个 `return` , 但方法并没有被中断，最后方法返回值是第二个 `return` 语句的值。

为什么有这样的区别，在此我们终于看到 Procs 和 lambdas 的本质差异：前者是「过程」而后者是「方法」。 一个 Proc 的行为像一段 code snippet , 是所在方法的一部分，所以运行到 return 就直接跳出方法了；而一个 lambda 的行为则像一个方法，它所在的方法对它进行调用，调用完了还返回原方法里面。这也是第一点区别的根源所在， Procs 只是代码块，不去管你传进来几个参数；而 lambdas 则是实实在在的方法，会严格检测你丫有没有搞错参数的数量。跟普通方法的区别在于 lambdas 是匿名的，就是说这个方法没有名字。

再举两个例子：

{% gist 4121472 Ruby_lambdas_example_4.rb %}

Ruby 有一个限制，调用一个方法的时候传入的参数不能含有 `return` 这个关键字，所以上例中 `Proc.new { return "Proc.new" }` 这个参数是非法的。但是为何 `lambda { return "lambda" }` 这个参数又是合法的呢？还是因为 lambdas 是方法， `return` 被封装在这个方法里面了，外面是看不到的，所以不判定为非法。

{% gist 4121472 Ruby_lambdas_example_5.rb %}

这个例子中， `generic_return` 这个方法期待代码块给它返回两个值，使用 lambdas 简单直接，就 `return` 两个值完事儿 (第 1 个 `puts`)；使用 Proc 就很考验人，首先不能出现 `return` 这个关键字 (第 2 个 `puts`)，其次要是不用 `return` 关键字的话，两个返回值不知道该怎么送回去 (第 3 个 `puts`)，当然依赖 Ruby 强大的灵活性还是能找到方案 (第 4 个 `puts` )，

如此说来，若需要的闭包是一个简单的代码块而已，那么 Procs 很好；如果需要闭包作为方法被调用的话， lambdas 是你的选择。

当你看到 lambdas 是披着 object 外衣的匿名方法的时候，有没有想到什么呢？既然一个匿名方法能当成闭包使用，那具名方法 (也就是普通方法) 呢？毕竟现成的普通方法有好多好多 …

方法对象
----
你别说，还真能这么用。这是 Ruby 提供的第四种闭包方式：方法对象 (Method Objects) .

{% gist 4121472 Ruby_method_objects_example_1.rb %}

上例中，我们有个现成的普通方法 `square` , 有需要的话，可以将其直接当成闭包复用了，做法是使用 `method` 方法将其转换成一个方法对象，然后就当普通闭包该怎么用就怎么用。

{% gist 4121472 Ruby_method_objects_example_2.rb %}

大家都能猜到 `square` 是个方法，不是个过程吧。也就是说这种闭包的行为跟 Procs 闭包不一样，而与 lambdas 闭包一致，实际上 method objects 和 lambdas 仅仅是匿名与否的区别。

总结
----
Ruby 处理闭包有两种形态：一是 snippets 型，包括 blocks 和 Procs , 其中 blocks 只是书写简化但受到一些限制的 Procs ; 二是 methods 型，包括 lambdas 和 method objects , 前者匿名后者具名。