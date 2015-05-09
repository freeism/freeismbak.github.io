---
layout: post
title: "SublimeText에서 Swift 실행하기"
author: "freeism"
description: ""
category: "Development" 
tags: ["Mac", "Swift", "SublimeText"]
---
{% include JB/setup %}

1. Package Install에서 Swift Plugin을 설치한다.

2. ~/Library/Application Support/Sublime Text 2/Packages/Swift에 아래 코드를 *swift.sublime-build"로 저장한다.  
{% highlight json %}
{
    "cmd": ["swift", "$file"],
    "selector": "source.swift"
}
{% endhighlight %}

3. Tools > Build System > swift를 실행하거나 Cmd + B를 누르면 빌드된다.
{% highlight swift %}
#!/usr/bin/swift
let sayHello: String = "Hello Swift World"
println("\(sayHello)")
{% endhighlight %}
> Hello Swift World
[Finished in 0.6s]
