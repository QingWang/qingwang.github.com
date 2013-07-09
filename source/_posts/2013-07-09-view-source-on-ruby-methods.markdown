---
layout: post
title: "在 Rails 源代码中快速定位 method"
date: 2013-07-09 18:32
comments: true
categories:
- Coding

---

[原文](http://pragmaticstudio.com/blog/2013/2/13/view-source-ruby-methods)

我(原作者)经常要去读 Rails 的源码，但是有时定位某个 method 在源码里的位置不是那么容易。比方说想看看 Rails 4 里面新的 `update` 跟原来的 `update_attributes` 有什么不一样，于是我查找 `def update` ，跳出来 50 个结果，分布在 37 个文件里面。显然我们要找的是 `ActiveRecord` 里面的哪个，但是不好找啊。实在是应该有办法直接跳到我们想找的那个 method 源码。

还真有。

<!-- more -->

假设我们有个叫 `Person` 的 model ，可以用下面的指令创建一个 object :

    person = Person.new
    
在 Rails 4 里面，我们可以使用 `update` :

    person.update(name: "Fred")
    
现在我们只是对这个 `update` method 感兴趣，于是首先想办法把它给抓出来: 

    method = person.method(:update)
    => #<Method: Person(ActiveRecord::Persistence)#update>

现在 `method` 就是一个代表了 `update` 这个 method 的 object .

高潮来了： Ruby 有这么个 method ...

    location = method.source_location
    
`source_location` 返回这个 caller 所在的源文件名及其定义所在的行数。

如果要直接打开源文件的话，用以下方法：

    `subl #{location[0]}:#{location[1]}`     # Sublime Text
    `mate #{location[0]} -l #{location[1]}`  # TextMate
    `mvim #{location[0]} +#{location[1]}`    # MacVim

然后把以上的内容组合一下，就得到一个 method ，可以放到 `.irbrc` 文件里面:

    def source_for(object, method)
      location = object.method(method).source_location
      `subl #{location[0]}:#{location[1]}` if location # 改成自己喜欢的编辑器
      location
    end
    
这样在 irb 里面就可以用

    source_for(Person.new, :update)

这样的格式来查看源码了。

class method 可以这样查看:

    source_for(Person, :update)
    
再进一步，如果想查看在 modules 里面定义的 instance methods ，比方说 Rails helper methods ，能不能用

    source_for(ApplicationHelper, :some_helper)
    
来查看呢？

答案是不行的，因为 module 不能被 instantiated (实例化)。

不过我们可以修改一下 `source_for`:

    def source_for(object, method_sym)
      if object.respond_to?(method_sym, true)
        method = object.method(method_sym)
      elsif object.is_a?(Module)
        method = object.instance_method(method_sym)
      end
      location = method.source_location
      `subl #{location[0]}:#{location[1]}` if location
      location
    rescue
      nil
    end
    
这样就行了。