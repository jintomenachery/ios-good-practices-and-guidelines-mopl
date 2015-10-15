#iOS Good Practices

[TOC]

##Common Libraries

Generally speaking, make it a conscious decision to add an external dependency to your project. Sure, this one neat library solves your problem now, but maybe later gets stuck in maintenance limbo, with the next OS version that breaks everything being just around the corner. Another scenario is that a feature only achievable with external libraries suddenly becomes part of the official APIs. In a well-designed codebase, switching out the implementation is a small effort that pays off quickly. Always consider solving the problem using Apple's extensive (and mostly excellent) frameworks first!

###AFNetworking
A perceived 99.95 percent of iOS developers use this network library. While `NSURLSession` is surprisingly powerful by itself, [AFNetworking](https://github.com/AFNetworking/AFNetworking) remains unbeaten when it comes to actually managing a queue of requests, which is pretty much a requirement in any modern app.

###DateTools
As a general rule, don't write your date calculations yourself. Luckily, in [DateTools](https://github.com/MatthewYork/DateTools) you get an MIT-licensed, thoroughly tested library that covers pretty much all your calendary needs.

##Architecture

### [Model-View-ViewModel (MVVM)](http://www.objc.io/issue-13/mvvm.html)
- Motivated by "massive view controllers": MVVM considers UIViewController subclasses part of the - View and keeps them slim by maintaining all state in the ViewModel.
- To learn more about it, check out [Bob Spryn's fantastic introduction](http://www.sprynthesis.com/2014/12/06/reactivecocoa-mvvm-introduction/).


##Other Cool things you don't want to miss
1. [VVDocumenter Xcode plugin](https://github.com/onevcat/VVDocumenter-Xcode). - This is tool you should use to add comments in your code. An awesome documentation on how to use this plug in is available @ [RayWendelich](http://www.raywenderlich.com/66395/documenting-in-xcode-with-headerdoc-tutorial)
2. You can find awesome XCode plugins which will make you XCode Guru @[Supercharging Your Xcode Efficiency](http://www.raywenderlich.com/72021/supercharging-xcode-efficiency). 
3. XCode Utility ** TODO **
4. Haven't you heard about CocoaPods? This is the best way you can manage your dependencies with third party project. How many times have you added somebody's  source code to your project([FacebookSDK](https://github.com/facebook/facebook-ios-sdk) :-/) and multiplied your source code size. And what about updating Facebook SDK later ? All such burdens can be lightened by using CocoaPods. Start using Cocoapods by reading [Introduction to CocoaPods Tutorial](http://www.raywenderlich.com/64546/introduction-to-cocoapods-2).
5. If you want to ask some clarifications and you are stuck with documenting the lengthy UI flow, you can start using [liceCap](http://www.cockos.com/licecap/). This simple tool can capture an area of your desktop and save it directly to .GIF.
6. [Xcode Utility](https://github.com/jintomenachery/ios-good-practices-and-guidelines-mopl/Resources/branch/Xcode-Utility.zip) - By doing below tasks, this small utility will make your life easier,
  - Creating ipa from Xcode archive
  - Symbolicating Crash logs using the crash file and the DYSM file
  - Setting path of developer directory
	As the app is not downloaded from Appstore, you might have to alter the security settings in Settings & Privacy > Allow apps download from: to Anywhere. Kudos to ==Darsan Geevarghese== for coming up with this tool.

