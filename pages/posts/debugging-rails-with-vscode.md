---
  
  title: Debugging Rails with VSCode
  date: 2019-09-16
---
  
No matter how carefully coded, reviewed, and tested your Ruby code is, odds are good that at some point you'll cause a catastrophic failure to at least one system you're responsible for. How do you prepare yourself?

In this post, I'll cover the whole debugging process—from finding the issue to determining the root cause. Use these instructions for debugging a single Ruby file, a Rails app, or a gem. I'll also include a detailed explanation of how to set up your development environment and how to use the [ruby-debug-ide](https://github.com/ruby-debug/ruby-debug-ide) gem and [Visual Studio Code](https://code.visualstudio.com) to investigate the state of your system to find the root cause of an error.

## First, Some Inspiration!
An example of a high-performance team that often needs to find the root cause of a serious issue under pressure is the mission control team at NASA.

Gene Kranz, director of the mission control team that saved Apollo 13, sums up their approach to problem-solving when [he said](https://www.quotes.net/mquote/4420), "Let’s work the problem, people. Let’s not make things worse by guessing."

We can use Kranz's simple, yet important, strategy when we approach debugging a Ruby application.

![Gene Kranz control room](https://upload.wikimedia.org/wikipedia/commons/6/60/Eugene_F._Kranz_at_his_console_at_the_NASA_Mission_Control_Center.jpg)

## Finding Errors Before Your Users Do

Finding production errors before your users report them gives you some time to think calmly about the solution. When you find errors early, your users may not notice that the error occurred, which gives you more time to determine the root cause and fix the issue. Under pressure, you may be tempted to provide a quick fix for the effect rather than the cause, without understanding the purpose of the code. I've made this mistake many times, so I can tell you from experience not to do this. It often causes a different issue or accelerated [software rot](https://en.wikipedia.org/wiki/Software_rot).

A good way to find errors fast is to use an [application performance monitor](https://stackify.com/what-is-application-performance-monitoring) (APM). One such APM, Retrace, reports all errors as they occur, along with a stack trace and any related logging.

![example output from apm](https://www.hitsubscribe.com/wp-content/uploads/2019/04/retrace-exception-details.png]

## The Best Place to Start Your Investigation

The best place to start your investigation is at the point the error was first discovered. If you have an APM like [Retrace](https://stackify.com/retrace/) configured, then all errors would be searchable along with the related stack trace. However, the stack trace could be sent as a screenshot from a user or could happen during your own testing.

A stack trace is an output generated from an application when executed, showing the line of code that caused an issue and the execution path through the system that led to that point. The following is an example stack trace from a simple program that shows a common Ruby error: *undefined method*.

```
undefined method `id=' for #&lt;User:0x00007f908847b1c8 @name="bob"&gt;
app.rb:5:in `&lt;top (required)&gt;'
gems/ruby-2.3.7/gems/ruby-debug-ide-0.6.1/lib/ruby-debug-ide.rb:92:in `debug_load'
gems/ruby-2.3.7/gems/ruby-debug-ide-0.6.1/lib/ruby-debug-ide.rb:92:in `debug_program'
gems/ruby-2.3.7/gems/ruby-debug-ide-0.6.1/bin/rdebug-ide:182:in `&lt;top (required)&gt;'
gems/ruby-2.3.7/bin/rdebug-ide:23:in `load'
gems/ruby-2.3.7/bin/rdebug-ide:23:in `&lt;main&gt;'
gems/ruby-2.3.7/bin/ruby_executable_hooks:24:in `eval'
gems/ruby-2.3.7/bin/ruby_executable_hooks:24:in `&lt;main&gt;'
```

From the stack trace output, we can determine the state of the system at the time of the error. This will help us recreate locally.

* The first line states that **id=** is an **undefined method** for the **User** object and caused the application to exit.
* The first line also states the **User** object has the name **bob**
* The second line states that the error occurred on line five of the code, **app.rb:5**
