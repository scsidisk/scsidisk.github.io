Unit Testing won’t help you write good code

准确地说，我们可以认为这是Test-Driven开发，其实，这种开发就是先写unit test case，这样的开发方式的主要目的是，为了防止你不会因为一个改动而引入Bug，但这并不会让你能写出更好的代码。这只会让你写出不会出错的代码。同第一点，这样的方法，只不过是防止糟糕的程序员，而并不是让程序员或代码质量更有长进。反而，通过Unit Test会为程序员的为自己代码做辩解的一种托辞。


Most comments in code are in fact a pernicious form of code duplication.

注释应该是注释Why，而不是How和What，参看《惹恼程序员的十件事》，代码告诉你How，而注释应该告诉你Why。但大多数的程序并不知道什么是好的注释，那些注释其实和code是重复的，毫无意义。


 The only “best practice” you should be using all the time is “Use Your Brain”.

唯一的“Best Practice”并不是使用各种各样被前人总结过的各种设计方法、模式，框架，那些著名的方法、模式、框架只代码赞同他们的人多，并不代表他们适合你，你应该更多的去使用你的大脑，独立地思考那些方法、模式、框架出现的原因和其背后的想法和思想，那才是“best practice”。事实上来说，那些所谓的“Best Practice”只不过是限制那些糟糕的程序员们的破坏力。

当你的软件开发出现问题的时候，就像bug-fix一样，首要的事是找到root cause，然后再case by case的解决，千万不要因为有问题就要马上换一种新的开发方法”。相信我，大多数的问题是人和管理者的问题，不是方法的问题。

3.我们应该/不应该测试什么
BDD 的第一个单词就表明了这一点，你不应该关注于测试，而是应该关注行为。这个看似毫无意义的变化提供了应该测试什么的准确答案：你应该测试行为。

例如你设计的一个对象，它有一个接口定义了其方法和依赖关系。这些方法和依赖，声明了你对象的约定，它们定义了如何与应用的其他部分交互，以及它的功能是什么。它们定义了对象的行为。同时这也应该是你的目标：测试你对象的行为方式。

不应该测试什么呢？
不要测试私有方法：私有方法意味着私有，如果你感到有必要测试一个私有方法，那么那个私有方法一定含有概念性错误，通常是作为私有方法，它做的太多了， 从而违背了单一职责原则。

不要 Stub 私有方法：Stub 私有方法和测试私有方法具有相同的危害，更重要的是，Stub 私有方法将会使程序难以调试。通常来说，用于Stub的库会依赖于一些不寻常的技巧来完成工作，这使得发现一个测试为什么会失败变的困难。

不要测试构造函数：构造函数定义的是实现细节，你不应该测试构造函数，这是因为我们认同测试应该与实现细节解耦这一观点。

不要 Stub 外部库：第三方代码不应该在你的测试中直接出现。
4.测试的目的
使重构更简单 —— 你可以自信的修改实现细节，而不用去触及公有 API。

避免代码恶化—— 恶化在什么时候发生？在你修改代码的时候。

提供了可执行的说明和文档 —— 你在什么时候更想知道软件实际上是如何工作的？在你想修改它们的时候。

减少了创建软件的时间 —— 怎么减少时间的？是通过更快速地修改你的代码，出错时测试会自信地告诉你哪里出错了。

降低了创建软件的代价 —— 时间就是金钱，朋友。
测试应该：

很快速(Fast) —— 测试应该能够被经常执行。

能隔离(Isolated) —— 测试本身不能依赖于外部因素或者其他测试的结果。

可重复(Repeatable) —— 每次运行测试都应该产生相同的结果。

带自检(Self-verifying) —— 测试应该包括断言，不需要人为干预。

够及时(Timely) —— 测试应该和生产代码一同书写。
5.UI测试
关于UI测试，需要测试的是用户的交互，而不是应用的外观，Xcode7中新增了UI Testing，具体可以看wwdc 2015 session :406_hd_ui_testing_in_xcode。


编写哪些测试以及如何编写这些测试
在编写测试时，我喜欢覆盖以下情况：
所有正面测试
这组测试可以确保所有的东西都如我们期望的一样工作。
所有负面测试
逐一使用这些测试，从而确保每个失效或异常情况都被测试到了。
正面序列测试
这组测试可以确保按照正确顺序的调用可以像我们期望的一样工作。
负面序列测试
这组测试可以确保当不按正确顺序进行调用时就会失败。
负载测试
在适当情况下，可以执行一小组测试来确定这些测试的性能在我们期望的范围之内。例如，2,000 次调用应该在 2 秒之内完成。
资源测试
这些测试确保应用编程接口（API）可以正确地分配并释放资源 —— 例如，连续几次调用打开、写入以及关闭基于文件的 API，从而确保没有文件依然是被打开的。
回调测试
对于具有回调方法的 API 来说，这些测试可以确保如果没有定义回调函数，代码可以正常运行。另外，这些测试还可以确保在定义了回调函数但是这些回调函数操作有误或产生异常时，代码依然可以正常运行。
这是有关单元测试的几点想法。有关如何编写单元测试，我也有几点建议：
不要使用随机数据
尽管在一个界面中产生随机数据看起来貌似一个好主意，但是我们要避免这样做，因为这些数据会变得非常难以调试。如果数据是在每次调用时随机生成的，那么就可能产生一次测试时出现了错误而另外一次测试却没有出现错误的情况。如果测试需要随机数据，可以在一个文件中生成这些数据，然后每次运行时都使用这个文件。采用这种方法，我们就获得了一些 “噪音” 数据，但是仍然可以对错误进行调试。
分组测试
我们很容易累积起数千个测试，需要几个小时才能执行完。这没什么问题，但是对这些测试进行分组使我们可以快速运行某组测试并对主要关注的问题进行检查，然后晚上运行完整的测试。
编写稳健的 API 和稳健的测试
编写 API 和测试时要注意它们不能在增加新功能或修改现有功能时很容易就会崩溃，这一点非常重要。这里没有通用的绝招，但是有一条准则是那些 “振荡的” 测试（一会儿失败，一会儿成功，反复不停的测试）应该很快地丢弃。


结束语
单元测试对于工程师来说意义重大。它们是敏捷开发过程（这个过程非常强调编码的作用，因为文档需要一些证据证明代码是按照规范进行工作的）的一个基础。单元测试就提供了这种证据。这个过程从单元测试开始入手，这定义了代码应该 实现但目前尚未 实现的功能。因此，所有的测试最初都会失败。然后当代码接近完成时，测试就通过了。当所有测试全部通过时，代码也就变得非常完善了。
我从来没有在不使用单元测试的情况下编写大型代码或修改大型或复杂的代码块。我通常都是在修改代码之前就为现有代码编写了单元测试，这样可以确保自己清楚在修改代码时破坏了什么（或者没有破坏什么）。这为我对自己提供给客户的代码提供了很大的信心，相信它们正在正确运行 —— 即便是在凌晨 3 点





满足什么标准的测试才是单元测试呢？根据《修改代码的艺术》，需要访问数据库的测试不是单元测试，需要访问网络的测试不是单元测试，需要访问文件系统的测试不是单元测试……

为了更方便地进行单元测试，业务代码应避免以下情况：

存在太多条件逻辑
构造函数中做的事情太多
存在太多全局状态
混杂了太多无关的逻辑
存在太多静态方法
存在过多外部依赖
例如，在代码中存在硬编码，或者是直接创建了一个数据库连接，这种做法都是比较危险的，因为在测试时没有办法“偷梁换柱”。

想要写好单元测试，学会重构是很重要的，重构的过程类似于清理厨房，虽然和做饭没太大关系，但可以让您下次做饭更方便，心情更好。可以重构的地方包括，在待测试类与其依赖之间增加一层Test Fixture；将创建逻辑与业务逻辑分开等等。重构可以采取以下策略：

编写测试代码建立基本的防护网。在单元测试和功能测试之间要有取舍，如果单元测试实施成本很高，可以先加功能测试。
通过增加中间层来打破依赖，不是为了去掉依赖，而是为了后续的修改以及测试的便利。
将第一步中编写的功能测试换成单元测试。
最后，Daniel还为大家提了一些建议：

项目里的破窗要修好，别容忍别人加新的破窗，用测试将破窗保护起来。
当你离开一个地方的时候，要让它比你来的时候更整洁干净。（童子军军规）
不停地重构你的代码，每次走一小步。
随后有人提问，如何评估一个单元测试的质量，用代码行覆盖率是否可行。Daniel认为没必要去追求代码行覆盖率，真正要覆盖的是逻辑，而不是代码行。通过结对编程可以减少低质量的单元测试，人都喜欢改变，但没人喜欢被改变，不要强求结对编程，让他看到好处，尝到甜头，吸引他来做结对编程，没有人喜欢落后，比如50%的人在做结对了，剩下的人自然会想尝试。

当被问及单元测试是否是必须的时候，Daniel回答这并不是必要的，而是需要进行综合的衡量，比如你的竞争对手一周前推出了一个产品，你需要在一周内完成产品研发并上线，这时可以选择写或者不写单元测试，对于没有写过单元测试的人，一开始是需要上手的成本的。