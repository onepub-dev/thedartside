# Dart the perfect CLI language

So let's not be boring, let's go right to the edge of the cliff and hope we don't get thrown over.

Dart is simply the best language for building Console applications and scripts.

That's really all you need to know. 

You can now get back on with your life knowing which language you need to use when you build your next Console application.

No? What, you expect me to justify myself?

Well OK, but only because I've got nothing better to do. The hornet's nest near my back door is already mad enough.

Firstly lets just make certain we are on the same page. For the purpose of this article \(and henceforth the rest of your existence\) I'm going to define a CLI app as any and all of:

* an application that runs from a terminal/console
* interacts with the user via text only io.
* a 10 line script replacement for bash/powershell script
* a 100,000 line app such as build or deployment scripts
* a full blown console application \(not that there is much need for these\).

So let's start with some metrics as to makes a good language for build Consle apps.

The language should:

1. easily take you from a 10 line script that somehow grew into 200,000 lines.
2. be easy to learn
3. not overly verbose nor overly cryptic. 
4. Works with minimal tooling
5. Allows you to run the script without compiling
6. Is fast to start and fast to run
7. Is type safe
8. Uses a protected memory model
9. comes with a debugger
10. allows threading for the occasional heavy lifting
11. can be deployed as a single file exe without needing a  VM installed.
12. provides good third party package support
13. Doesn't encourage the creation of DSLs
14. Doesn't encourage the creation of magic code.
15. Is cross platform

To avoid being lynched rather than comparing Dart to a range of other languages let's have a look at why Dart is particularly strong in each of these metrics and understand why they are important.

## 1\) easily take you from a 10 line script that somehow grew into 200,000 lines.

Cli scripts have a habit of evolving rather than being designed.  What started out as a simple 20 line script to automate a couple pieces in the build process,  somehow evolved into the corporations primary build and deployment tooling.

Dart excels in these type of evolutions. It's easy to throw a quick script together with a single file and a 5 line pubspec.yaml \(in Dart a pubspec.yaml defines the packages dependencies\).

You can then grow out the directory structure as the collection of dart files grows and pull in third party packages by adding a single line to your pubspec.yaml.

You can create your script with vi or notepad using print statements to debug your code and when you need to move to a full blow IDE like Visual Studio Code \(yes I know they aren't allowed to call it and IDE\).

## 2\) be easy to learn

This is an important metric and one that I think makes Dart stand out.

Dart takes its syntactic heritage from C and has been described as the Love child of Java and Javascript.

You can port a Java or JavaScript program to Dart with very little effort \(I'm talking syntactically here, not about libraries\). 

What this means is that just about anyone can pick up a Dart program and read the code with little training. This perhaps compare sharply with the likes of Ruby that really has a significantly different syntactical structure to the likes of Javascript.

The easy to learn metric is really important for the long term maintenance of programs and the adoption with in a team.

Languages like C fail at this miserably. Whilst I loved C when used it a realized many years later that its simply too expensive to use for most applications. It takes several years to get a junior programmer to the point where you can trust their code won't crash the whole system and you spend countless hours diagnosing memory corruption. Unless you a writing an OS or some real time app, the days of C are over.

Rust and Go seem to be designed as a replacement for C and I'm not going to be overly critical of these languages except to say the you don't need to be burdened with Rust's memory management and Go just feels like its too low level for what you need building a console app \(although I do like Go's coroutines\).  

When I first started using Dart I really found it was delightful. With my background in C/Java/Javascript it was really easy to pick up and start using Dart.

## 3\) not overly verbose nor overly cryptic. 

This is a real bug bear, and really goes back to point 2.

Java is often \(fairly\) criticised for being overly verbose and requiring excessive boiler plate code.

I found this delightful description of Perl:

> Global variables abound. Crazy cryptic syntax. Ridiculous scoping rules with hacked on workarounds.

Great languages need to live in the Goldilocks zone. Not too cryptic, Not too verbose, but just right; like porridge with honey.

Dart feels like it nailed this one. The language is in no way cryptic and it manages to remove lots of the common boiler plate code we get from the likes of Java.

Cryptic languages are hard to maintain. When you come back to the code in two years and  it's just not obvious what it's doing, you have a problem. Really clever solutions that are had to maintain are bad solutions.

## 4\) Works with minimal tooling

Whilst I hate bash with a passion \(that's why I wrote [DCli](https://pub.dev/packages/dcli)\) it requires absolutely no tooling, just an simple editor.

Dart almost gets there. You do need to install Dart, but that's it. You don't need a whole tool chain to get a script up and running and you don't even need to compile a script to run it.

You can create a Dart script with vi or notepad and then migrate to using an IDE like Visual Studio Code when you need to.

```dart
sudo apt install dart
dart pub global activate dcli
dcli create hello.dart
./hello.dart
```

## 5\) Allows you to run the script without compiling

This is a pretty important one when you writing a little script that you want to quickly iterate on.

Edit/Run

Being required to compile between every edit/run cycle just slows things down.

Python and Ruby do a nice job of this as well, but C fails.

## 6\) Is fast to start and fast to run

This leads of from the prior point. If you are building little script then fast turn around time is important.

Except for languages like C that require a compile this is pretty well supported across most of the language alternatives.

## 7\) Is type safe

So up to this point, I've said nothing really controversial \(unless you like Perl but I think I'm safe as most of those devs are either dead or having their afternoon nap\), so I guess it's time to upset some people

I know that there will be readers who swear that typeless languages are the only way to go.

This graph tells a different story:

![](.gitbook/assets/image.png)

Source: [https://www.researchgate.net/](https://www.researchgate.net/)

As the title states, the graph shows the relative cost of fixing a bug in the different stages of an applications life. Fixing a bug in maintenance cost 100 times as much as it cost to fix in design.

Typeless language move more bugs into testing or maintenance, this is a simple fact.

A type language will generate a compiler error if you trying to use a type inappropriately. The bug is caught during implementation. A typeless language leave those bugs to be found in testing or maintenance.

What this graph doesn't show is the consequential costs. A bug may take 100 times more to fix in maintenance due to the significant effort required to find and fix, but this doesn't reflect the consequential costs of losing a customer because of a bug. 

As a case in point. I was recently involved in porting a large and mature javascript project to Dart.

The first team working on the project converted it to Dart without turning on Darts type safe feature \(yes Dart can be used without types\).

We we joined the project we where having significant difficulty under standing how the project worked. It used 30 or 40 different messages types that were being passed around the code base.

So we turned on typed safety and typed each of the messages. What fell out of this? Two things.

1\) the compiler immediately identified some 20 bugs where a field was being accessed on a message that simply didn't have that field. At least 3 of these were causing real bugs in the code.

2\) the team was able to understand the code base as it was now clear what message was being operated on.

So type safety made the code easier to read and fixed a number of bugs simply by compiling the application.

Type safety may take a little more time when writing the code, but you save that in spades in reduced bugs and in lower cost over the full life cycle of the code base.

Languages like Python and Javascript fail completely on this metric and Ruby is only sort of type safe.

## 8 \) Uses a protected memory model

I don't think this is a particularly controversial one, although I do wonder at the amount of C programming that is still going on.

99.9995% of console apps do not need to directly manage memory \(yes I made that stat up\). 

The cost of use a non protected language such as C/C++ is considerable at every stage of an apps life;  coding, debugging, testing  and maintenance.

As someone who loved C/C++, when I moved my team off C/C++ to Java, I never looked back.

## 9\) comes with a debugger

I've met developers who think you don't need a debugger, print statements are sufficient.

Well, I really don't need a car, a horse and buggy will get me there.

You need a debugger and it needs to be a good one and you need to be using it:\)

Don't get be wrong print/log statements still play an important part when debugging.

Most of the languages that you would consider for console apps now have reasonable debuggers so this isn't really a point of differentiation.

## 10\) allows threading for the occasional heavy lifting

This isn't actually that important. Must console apps tend not to do a lot of cpu intensive work so multiple threads isn't usually that critical, but when you need it you need it.

Dart provides Isolates which are like threads but with isolated heaps \(hence the name\).  The down side is you have to jump through a couple of hoops to start an isolate, the upside is it removes most of the complexity/dangers of threads.

Python is is probably language with the biggest problem in this area and is perhaps the slowest of the languages you are likely to use for a console app.

## 11\) can be deployed as a single file exe without needing a  VM installed.

This is one where Dart shines. You can compile a Dart script without any special tooling, copy just the single file exe to another machine and run the code.

```dart
dart compile hello.dart
chmod +x hello
scp hello someremotemachine:
ssh someremotemachine
./hello
```

## 12\) provides good third party package support

I have to admit that, if Dart has a weakness its this one. 

[pub.dev](https://pub.dev/) is the package repository for Dart. As of May 2021 there were 22,000 packages on pub.dev that is double what it was 12 months earlier.

So nothing to sniff at but there are still a number of significant holes in Darts' package eco-system.

These holes are being rapidly filled and with a new focus on Dart server side they will start to move even faster.

The likes of Java's package eco system is certainly way more mature as will by Python and Ruby's.

## 13\) Doesn't encourage the creation of DSLs

I'm look at you Ruby.

DSL's seem like a nice idea, until you have to maintain someone else's code. Its bad enough having to learn a well maintained language and well documented language without having to maintain a half arsed DSL. Languages like Ruby encourage developers to build code that is just hard to maintain. If feels like a nice idea, but it just ain't so.

## 14\) Doesn't encourage the creation of magic code.

An Ruby gets a guernsey again.

Ruby has this neat feature where you can call a method on an object that doesn't actually exists.

You can then write code that captures the call and do some sweet, sweet magic.

This is really lovely unless you inherit the code and have to work out what its doing. Static analysis is almost impossible on this sort of code.

There are a number of languages that encourage bad behaviour.  Languages that have a history of encouraging developers to use poor programming patterns should be avoided.

## 15\) Is cross platform

This falls into the category of nice have. Many corporate environments are homegenous so cross platform isn't that big a deal. Having said that a recent survey I conduct on reddit shows a significant number of Windows and MacOS users are deploying to Linux servers. Have a single script that works on your dev machine and your production system is nice and saves you having to learn multiple languages or libraries.

## Conclusion

Dart performs strongly in virtually every category. Its greatest weakness currently is the maturity of third party packages. For most console apps this isn't much of a problem and the Dart ecosystem is maturing nicely.

If you are using Bash script for building your console apps,  moving to Dart using the DCli package is a no brainer.

If you are doing Flutter development then using Dart/DCli for your build production systems is really a know brainer.

For those of you coming from a Ruby/Python/C environment, you really should take a look at Dart for future console apps. It solves a lot of problems and really is delight for to work with.

{% hint style="info" %}
Dart is delightful.
{% endhint %}

















