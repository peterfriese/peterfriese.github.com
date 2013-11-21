---
layout: post
title: "Debugging Tips for iOS Developers"
date: 2013-11-21 12:00
comments: true
categories: 
- iOS
- Debugging
- Xcode
- NSLog
asins:
- "0321917014" # Effective Objective-C
- "1935182536" # Objective-C Fundamentals
teaserimage: /images/2013-11-20-debugging-tips-for-ios-developers/xcode_150x150.png

---

Let's face it - no matter how hard you try, no matter how many testers you use in manual tests, no matter how disciplined you are in writing unit tests and following the TDD drill of writing a failing test first and then adding the production code that turns the test green - bugs are inevitably linked to our lives. In this post, I'll show you a number of debugging techniques that might come in handy for your next project. Hopefully, you know some of them already, but I am sure some of them will be new for you.

<!-- more -->

Sometimes, you are responsible for a bug, sometimes bugs are in a third party library you use in your project. And sometimes, the documentation of an API doesn't match the implementation, giving you funny results for code that slavishly adheres to the official documentation.

With the following techniques, you'll be able to go hunting for bugs and find out where they came from. Once you've found out (and fixed the bug), you should consider writing a unit test to prevent future regressions. So, here we go.

## Breakpoints

Most developers are familiar with the concept of breakpoints, but if you're new to Xcode you might wonder how to create or modify a breakpoint, so I'll give a brief overview. 

{% fancyimage right /images/2013-11-20-debugging-tips-for-ios-developers/breakpoint.png 300x150 Simple Breakpoint %}

You can create a breakpoint by clicking on the line number in the editor gutter. The breakpoint will be displayed as a solid blue indicator in the gutter. From that moment on, your application will pause execution as soon as execution reaches the breakpoint. 

### Editing Breakpoints

There are a few ways to manipulate breakpoints:

1. To temporarily disable the breakpoint, just click on the breakpoint indicator, which will then turn light blue to indicate it is disabled.

2. To remove a breakpoint, drag it off the editor gutter, making sure to crank up the volume to enjoy the swooshing sound ;-) 

3. Breakpoints have properties that you can edit by right-clicking on a breakpoint indicator. Selecting the *Edit Breakpoint* menu will display a tool window which allows you to customize various aspects of a breakpoint, effectively turning a breakpoint into a conditional breakpoint:

### Conditional Breakpoints

The various properties of a conditional breakpoint allow you to specify exactly under which circumstances execution should stop at the breakpoint. What's more, you can specify certain actions that should take place when the breakpoint criteria are met.

{% fancyimage right /images/2013-11-20-debugging-tips-for-ios-developers/breakpoint_condition.png 300x150 Conditional Breakpoint %}

For example, in order to stop execution inside a `UITableViewController`s `tableView:cellForRowAtIndexPath:` method when the table view controller requests the cell for the 3rd row, add this condition:

	indexPath.row == 2

Of course, you can specify more complex conditions as you see fit.

Something which is unique to Xcode is the option to execute a certain action whenever a breakpoint is hit. There are a number of interesting actions, such as:

1. Execute an AppleScript
2. Capturing an OpenGL ES frame
3. Issue a Debugger command (more on that later)
4. Log or speak a message
5. Execute a shell command
6. Play a sound

Of all those options, speaking a message or playing a sound stand out the most. While you might think these are just toys, they are actually very useful in combination with the option to continue execution after hitting the breakpoint.

{% fancyimage right /images/2013-11-20-debugging-tips-for-ios-developers/breakpoint_speak.png 300x150 Breakpoint with Action %}

Just consider for example an app that waits for the arrival of a background signal while the user does something in the app. Now, if you place a breakpoint on the line that receives the background signal and configure the breakpoint to play a signal or speak a message, you can go on and exercise the app in the foreground while waiting for the background message to arrive. As soon as the message arrives, you'll hear the sound play and can take appropriate actions.

### Exception Breakpoints

Exception Breakpoints are a very powerful tool - basically they allow you to spot places in your code where an exception is thrown (and not caught). So instead of just marvelling at a crash in `main.m`, add an Exception breakpoint to your project (by clicking the tiny `+` icon on the bottom of the breakpoints view).

### Symbolic breakpoints

Symbolic breakpoints are useful when you want to stop in a piece of code that you don't have source code for. Let's say you're interested in who (and when) calls `tableView:numberOfRowsInSection`. Just create a new symbolic breakpoint, using the symbol `tableView:numberOfRowsInSection`. The debugger will now break whenever any piece of code in your app sends this very message - no matter if it's your code or code from a third party.

## The Debugger Console

As soon as your application has hit a breakpoint, you can start inspecting its current state. Most likely, you'll use the *Debug area* at the bottom of the Xcode window to do so. 

{% fancyimage center /images/2013-11-20-debugging-tips-for-ios-developers/debugarea.png 600x400 Xcode Debugger %}

However, sometimes it can be tedious to click-navigate through a deep hierarchy of nested properties. This is when the debugger console comes in handy. It is located on the right hand side of the debug area. Just click inside the white space and type a command. When debuging Objective-C code for iOS, you're most likely using [lldb](http://lldb.llvm.org/). Former versions of Xcode used [gbd](https://www.gnu.org/software/gdb/) as their default debugger. A comparison of lldb and gdb, including a list of commands they support can be found [here](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/gdb_to_lldb_transition_guide/document/lldb-command-examples.html). A more in-depth documentation of lldb can be found [here](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/gdb_to_lldb_transition_guide/document/lldb-terminal-workflow-tutorial.html). Don't worry - you do not have to wade through all of this documentation, I'll give you a run down of the most important commands here:

### Printing Objects

If you want to peek inside an object, you can use the `po` (**p**rint **o**bject) command:

	(lldb) po index
	1

You can dig deeper into objects, like this:

	(lldb) po todo.title
	Eat an apple


### Printing Primitives

Say you want to print the contents of a variable which isn't an object, for example the length of the string in the previous example. Using the `p` (**p**rint) command you can print the value of primitives such as integers or constants:

	(lldb) p i
	17

You can even invoke methods:

	(lldb) p [todo.title length]
	10

### Assigning Values

If you quickly want to change the value of a variable without having to restart your app, use the `expr` (**expr**ession) command:

	(lldb) expr todo.title = @"Changed"
	(NSString *) $6 = 0x0c139a80 @"Changed"

### Printing the Stacktrace

The current stack can be printed using the `bt` (thread **b**ack**t**race) command:

	(lldb) bt
	* thread #1: tid = 0x1e05f, 0x0000381a TodoMobile`-[TodoListViewController tableView:cellForRowAtIndexPath:](self=0x0c795c40, _cmd=0x00fcbadf, tableView=0x0e25e400, indexPath=0x0c7f95e0) + 138 at TodoListViewController.m:67, queue = 'com.apple.main-thread, stop reason = breakpoint 1.1
    frame #0: 0x0000381a TodoMobile`-[TodoListViewController tableView:cellForRowAtIndexPath:](self=0x0c795c40, _cmd=0x00fcbadf, tableView=0x0e25e400, indexPath=0x0c7f95e0) + 138 at TodoListViewController.m:67
    frame #1: 0x0094e61f UIKit`-[UITableView _createPreparedCellForGlobalRow:withIndexPath:] + 412
    frame #2: 0x0094e6f3 UIKit`-[UITableView _createPreparedCellForGlobalRow:] + 69
    frame #3: 0x00932774 UIKit`-[UITableView _updateVisibleCellsNow:] + 2378
    frame #4: 0x00945e95 UIKit`-[UITableView layoutSubviews] + 213
    frame #5: 0x008ca267 UIKit`-[UIView(CALayerDelegate) layoutSublayersOfLayer:] + 355
    frame #6: 0x01b0381f libobjc.A.dylib`-[NSObject performSelector:withObject:] + 70
    frame #7: 0x048062ea QuartzCore`-[CALayer layoutSublayers] + 148
    frame #8: 0x047fa0d4 QuartzCore`CA::Layer::layout_if_needed(CA::Transaction*) + 380
    frame #9: 0x047f9f40 QuartzCore`CA::Layer::layout_and_display_if_needed(CA::Transaction*) + 26
    frame #10: 0x04761ae6 QuartzCore`CA::Context::commit_transaction(CA::Transaction*) + 294
    frame #11: 0x04762e71 QuartzCore`CA::Transaction::commit() + 393
    frame #12: 0x0481f430 QuartzCore`+[CATransaction flush] + 52
    frame #13: 0x0087bdc9 UIKit`_afterCACommitHandler + 131
    frame #14: 0x01d364ce CoreFoundation`__CFRUNLOOP_IS_CALLING_OUT_TO_AN_OBSERVER_CALLBACK_FUNCTION__ + 30
    frame #15: 0x01d3641f CoreFoundation`__CFRunLoopDoObservers + 399
    frame #16: 0x01d14344 CoreFoundation`__CFRunLoopRun + 1076
    frame #17: 0x01d13ac3 CoreFoundation`CFRunLoopRunSpecific + 467
    frame #18: 0x01d138db CoreFoundation`CFRunLoopRunInMode + 123
    frame #19: 0x03a3f9e2 GraphicsServices`GSEventRunModal + 192
    frame #20: 0x03a3f809 GraphicsServices`GSEventRun + 104
    frame #21: 0x0085fd3b UIKit`UIApplicationMain + 1225
    frame #22: 0x000032dd TodoMobile`main(argc=1, argv=0xbfffedc0) + 141 at main.m:16

## Alternatives

If you're not happy with the level of tool support Xcode provides you with, I recommend taking a look at [AppCode](http://www.jetbrains.com/objc/) - its debugger is a lot more powerful. However, it currently lacks support for sound breakpoints ;-)


## More Info

If you're interested in learning more about debugging iOS apps in general and some tools that might come in handy, check out this presentation:

{% slideshare 17145826 %}

Also, Heiko Behrens and I had a chat about debugging iOS apps - it is available as an episode of the excellent (ok, I am biased) podcast [UISprech](http://www.UISprech.de) (in German only, sorry) here: [UISprech #7: Debugging Tools mit Peter Friese](http://UISprech.de/7).


