# P1：【发布2.0】游戏检测的对抗与防御艺术-鬼手56-大咖面对面114期_x264 - 漏洞银行BUGBANK - BV1cX4y1c7ht

![](img/b353d85daebf75f275e682d3a9d29a11_0.png)

为知识而存，因技术而生。各位小伙伴们，大家晚上好，欢迎参加第114期漏洞银行安全技术直播大咖面对面。我是今晚的主持人年念，今晚要给大家做技术分享的大咖是来自奇安性反病毒工程师的鬼手五六大咖。

他带来的议题是游戏检测的对抗与防御艺术。那作为新安之路二进制小组的负责人，同时呢也是CSDN信息安全领域优质的创作者，今天晚上他要给大家讲解有关游戏安全领域领域的对抗与防护的手段。

感兴趣的小伙伴们可要做好笔记。听到最后哦。那同时欢迎各位小伙伴们登录直播间在聊天区进行交流互动。听讲的过程中有任何疑问，可以随时在聊天区提出在演讲完毕之后，大咖会在行长问答环节集中结。答小伙伴们的疑问。

同时，今晚的听讲福利也将在问答环节结束后，挑选一位幸运观众送出，是由鬼手五六大咖亲自挑选的书籍逆向工程核心原理。那么下面这让我们有请鬼手五六大咖开始今天的直播分享吧。好的，先做一个简单的自我介绍的。

我在鬼手，目前是在做反病毒的相关的是相关的工作，然后主要擅长的是环金制这一块的东西，然后对漏洞啊。然后还有安卓内向的这一块也有所涉猎啊，今天要分享的是关于游戏检测的这样一个内容。



![](img/b353d85daebf75f275e682d3a9d29a11_2.png)

然后我们直接来看PPT这样一个目录。首先呢那提到游戏检测，就不得不先聊一下关于游戏外挂的一个行为。游戏外挂的行为的话，总体来说是分两种。第一种是修改游戏的关键数据跟代码，这种的话是属于一个篡改的行为。

比如说你之前之前。我们很常见的一些像CF的无线子弹，还有经常会看到的QQ跟微信的防撤回，都是通过修改游戏关键数据跟代码去达到的这样一这样一种方式。另外呢一个是去靠游戏的一个函数。

这种是属于越权访问的行为。假如说你想要去实现一些外挂的一些功能的话，这个这个动作其实是必须要做的。你需要通过定位一些关键的函数，然后去找到一些功能靠去在汇编里面去调用来达到这样一个呃目的。

实现你想要做的一个事情。那游戏安全的游戏公司的安全跟开发人员呢，就针对这两种的外挂行为，设计出了多种多样的一个检测方式。那今天我们来讲就。这最常用的4种。首先我需要用到一个功能靠。

我这里我这里今天准备的例子是自己写了一个demo，然后去给微信手动加上一个检测。所有的一个针对靠的一个检测，都是对这个发送消息，这样一个靠来找的。所以现在我需要去找到这样一个前置的一个功能。

先来说一下这个功能怎大概怎么去找。我现在看一下效果吧。我先把这个打开。嗯。这个单。Oh。直接把它注，这个是我事先准备好的一个demo，我现在需要用到这样一个发送消息的这样一个靠，前面是微信ID。

然后里面是消息内容，我直接点发送消息，就可以不通过微信直接用我的代码去给他发一个消息。OK现在大概来讲一下这个功能大概怎么样去找。一般来说先说一下思路吧。拿一个记事本。假如说你想要去写一个啊。

先从正向的角度来说说一下这么一个东西。假如说你想要去写一个发送消息的一个功能的话，那实际上。我们现来大概猜一下有几个参数。上的MSG了吧。文本消息吧。然后首先第一个必须要具备的一个参数就是W叉杠G。

消息内容你要发送什么消息？然后第21个是要将这个消息发送给谁，这样一个唯一的一个能够让指定的人接收到这样一个东西，在微信里面实际上是用微信ID来表识，就相当于是QQ里面的QQ号，它具有唯一的一个标识。

不会重复。啊也是数据爱线是这样子。那假如说你想要去找到这么一个功能的话，就可以从这两个点去入手。可以从消息去下手，也可以从当前的一个微信ID去入手找这应个功能。实际上从微信ID是会比较的快一点。

然后我们再来拆解一下发消息这样一个步骤。首先我需要选中需要发送消息的人，比如说选中这个微信或者是选中这个微信，第一步是需要选中当前的一个微信ID。然后第二一步才是去发消息。

那么我们首先就可以先找到当前选中的一个微信ID，然后再通过选中的1个ID这样一个访问关系去找到发送消息的这样一个功能套。我们先用先用CE吧。对。Yeah。附加一下微信。

当前进程这个我现在用的这个版本的微信是3。2的，它有两个进程，你首先需要去识别一下它到底是属在哪个进场，我先打开一下任务管理器。哎呀这个窗口开的太多了。我操。我他把再关一下吧。

我先在看一下到底是哪1个ID直接拉到详细信息。这里，这里有两个进程，需要看内存占用比较大的这一个是微信的主进程，它的是进程ID是24404，然后。CE里面显示的是16静制。

所以你你还需要把它换成16静制，24404。就是5F54，那里不能附加错的。该话搜到的数据其实也是错的。54F4就是这个好。我们先选中文件传输助手，数值类，先选择搜索字符串，然后勾上UTF16。

UTF16其实就是unicode在windows的编码里面的，有两种编码形式，一种是A版的，也就是奥斯柯玛，一种是W版的。但不管你其实不管你用A版的还是用W版的，那最后都会被转成unicode的形式。

所以说一些大型的一些客户端，其实都会用选用unicode的这样一个编码。你直接优先搜索这个文件传输助手的一个微信ID是叫fi helpPR直接去搜这样一个结果，然后再切换到另外一个微信。

普通的微信ID它是用WXID下划线开头，然后再次扫描这样的话结果就剩下9个。把这个位置。把数值显示完整，修改一下类型。长度调大，这个就是完整的一个文信ID。好，然后再去切换一下。

没有发没有发现这一排的微信ID是一直在跟着当前切换的一个窗口一直变。那现在我需要筛选出唯一一个我选中的1个ID啊，怎么去做呢？选中一半，然后更改记录数值。

把当前的这个ID改成biHELD啊改成文件传输助手的ID点确定。那如果说我现在这条消息是发给了文件传输助手哦的话，那就说明当前的ID是在前面我选中的这一栏哦，然后直接发送。OK你可以看到。

但其实我是在对着我另外一个微信发的消息，对吧？但实际上收到的是文件传输助手，刚刚是一个预览的效果，我对面的另外一个微信是没有收到消息的。

那就说明刚刚我们改的这一台ID里面有一个是当前选中的1个ID现在我们可可以排除掉另外一半。还剩下一半啊，继续去刷选用这个我刚复制的是文件上作手。更改记录数值，然后。再改再发1个啊22。

okK还是发给了文件传输入口。那么我可以再筛掉另外的一个半，剩下3个，那就1。1。1。1点筛了。三3三4。哎，我没点了？嗯。哦哦，这个实际上是。还是发给了原来的一个微信。

这边是没有收到那这个其实就不是当前的1个ID可以去删掉。然后剩下两个。777。还是发给了，那其实就是这一个。我可以去把这个删掉，然后。确认一下，再把这个改一下。如果这条消息发给了文件传输助手的话。

那那这个ID是不是找对了，看到没有？它其实是已经发过去了，然后我们需要用OD去附加一下。嗯。2。当前的一个微信ID的地址已经找到了。那在你发送消息的时候，必然要对这个ID进行访问。

所以可以把这个地址给它复制下来。Yeah。然后在这个位置去下一个啊。断点内存访问下访问断点，然后点击发送，嗯，让它断下来。嗯，这什么这个这个位置是我之前打的断点，这个这这个靠是我之前打的断点。

这不是内存断点，这个才是现在内存断点断下的位置。这下面有一个一个提示。我刚那个断点没有清掉。好，现在这个位置是在判断EBX里面的内容是不是零EBX也就是微信ID的这样一个字符串。

它在比较这个位置是不是零。所以说在读取这里内容，所以这里都断下来了。然后正常来说，其实这里还有一步，你需要在所有的一个调用堆栈里面去逐个排查看哪一个到底是符合你想要的一个ID。那今天这个不是重点。

所以我就直接去。到第一层。先到这里去下一个段，在这个位置，这个位置实际上是在嗯你先把这个断点给删掉吧。这里在比较EBX的值为0，那实际上它是在干这么一件事情啊，假如说你去写这样一个代码的话。

你首先要做一个最基本的一个错误检查，判断一下。微信ID是不是唯空？不为空的话，如果它等于。弄的话，那就直接让我赶回。所以。所以说我们刚刚断下的这个位置，我就有两我这有两个断点。Yeah。哎。

我刚刚打的那个地地方跑哪去了？我操。Yeah。这是崩了吗？是吧？自带意思吧。然后把微信调出来。断电内存访问老公。这个口这个这个不是这个是我没没消掉的一个论点。在这个位置。

这个位置应该是在判断当前的微信ID是不是为0，然后返回上一层。这里是在呃大概是在判断。判断微信还。是否わ空。所以接下来的思路就是你需要顺着这个靠往下去找，直到找到刚刚直到找到我们想要的那个功能为止。好。

我先把断点给它删掉。Yeah。九运行嗯，从这个位置继续往下走嗯，随便发一条消息，再让它断下来。找什么样一个靠呢？我需要去找一个靠，必须具备有两个参数。嗯，第一个当前消息的内容。

第二1个嗯就是我要发送的一个微信ID，或者是说有一个参数。但是这个参数里面有完整的一个结构体，这个结构里面包含这两个内容，其实也是可以的。好，继续我来单部跟一下这个位置。在找到泡的时候去停一。

第一个是EX，看一下。参数的内容是什么？这个是这个是文字的要消息内容的。这个是我刚刚说的1个777消息内容，然后ECX。ECX。不知道是什么东西，这里是实际上是缺了一个要发送的1个ID，然后继续往下找。

这只有一个参数。也是只有消息内容，继续完善。这里在判断EX的值也不是。啊，大概就这个位置还是挺眼熟的。先来看一下，它有1234，有有4个参数。首先来看ECS。ECX不知道什么东西。

然后EDXEDX是文件传输助手的ID啊，这是第一个条件啊。好，这是第一个必备的传输EBX。而是这样一个操新内容。最后再来看EDIEDI其实不用看了，因为它其实具备了。所有的发消息，所以所要的一个因素啊。

怎么判断到底是不是这个功能呢？我现在发的是777，对吧？好，然后我把EDX去。给他改一下，哦不对。X00。呃，消息内容，我把这个地址给复制下来。手动添加，因为在CE里面改会比较方便。105F4750。

然后类类型选择字串unicode啊这。现在要发送的内容是777，我在这里手动给它改成999。那假如说发送发出去的内容是999的话，那么这个考实际上就是我们要找的。发放消息。嗯。F9运行。怎么又断了？哦。

看到实际上我这里发出去的已经是99。啊，这样一个功能实际上就找到了。OK接下来讲啊关于检测相关的内容，将由靠就准备好的，先来说一下关于变量检测。先在讲一下大概的原理。假如说你需要正常去调用一个功能的话。

完整的去写一个功能。肯定是首先是去执行放一，然后执行第二个函数，第三个函数，第四个函数它是有一个完整的一个调用链。在这里。那假如说这是正常的一个。函数的一个执行流程。那假如说你是写了一个外挂。

就像我这个直接去靠了里面的一个靠的话，那肯定是不会有这样一个完整的调用链。我是直接去调用放肆这样一个代码的。所以说这两者之间的区别就是其中有一。正常调用的话，我是会完整的执行所有的一个函数。

我果是外挂的功能，那。嗯，我就会直接去调用关键的函数。所以呢基于这个差异，那其实就可以用到一个设置一个变量。在关键函数的上面。假设先把这个先把肯定是先要把这个东西置为0，然后在你。走到一半的时候。

在调用这个关键功能之前，把这个位置给制成一，然后再关键靠里面去判断。假设说如果我这个标志位为一的话，那就代表一个事情，就说明我整个的一个函数调用链是完整的执行下来了。

那我就完整的去正常去执行我的一个游戏功能。那否则的话，如果说这个位置不为一，它没有被置一的话，那说明可能是被一些嗯。外挂直接去调用了这样的一个功能。那那你这个标志位实际上还是。它的状态值实际上还是0。

那如果说这个值为零的话，嗯我就会去对一些外挂去进行处理。然后在检测完这样一个值之后，我还需要做一件事情，就是把这个标志位给置回去，就是制成0。因为如果我不把这个标志位置回去的话。

实际上这个值是永远都会等于一。那这个检测实际上是没有效果的。这一个就是整个的一个变量检测的一个原理。通过这样一个变量安插在调用调用站的中间，然后来判断整个函数的调用链是否完整。如果不完整的话。

就对你进行一些游戏之外的一些处理。好，然后说完了原理这里再看一下实际的一个效果，就可以啊算了，就最后还要用我这里写了一个关于检测的一个demo，我先把变量检测开开。然后我现在正常去发一个消息。

他是没有反应的对吧？然后正我现在用我自己写的一个程序在在外部去给他发消息，它会显示警告检测的外挂哦。不管不管我这边你可以看到这是很明显的一个区别。对，这个就是变量检测的一个效果。讲完了原理。

然后再来说一下呃，处理的手法处理的手法其实很简单。变量检测它有一个最致命的一个缺陷，就是需要用到一个全局的数据。去存储这样一个变量，变量的值既然是用全局的数据存储，那肯定就能在内存里面把它搜到。呃。

我这里用的是零跟1啊，实际我们现在来过一下吧。现在是已经附加了微信，然后新的扫描。把数值类型换成四字节，然后再把。数值先去扫描一个零，因为现在是没有调用的这样一个状态，然后点首次扫描，有8万多个结果。

然后需要用到OD。前提是你过这个检测的前提是你已经找到了这个功能，这个是这个就用不着了。Yeah。然后我需要发送消息的那个代码。好像在下面吧。哦，在这个位置啊，我在这里下一个断电，然后。

在这个位置去发送一条消息。那现在是检测已经开启的一个状态。也就是说我的流程实际上是走到了发送消息的call这里。那么实际上这个标志位就已经被置为一了。那我可以在断点断下的这种状态去搜再去搜索搜索一。

这样的话就可以通过一个变化的一个方式去找到这样一个变量的值。但实际上真实的游戏它未必会用零跟一来做这样一个检测标志，可能是一个随机的值。那如果它不是用零跟一的话，实际上你要从变动未知初始值跟变动的数值。

这两个开始一一步一步去排查。然后方法都是一样的，只不过是多做一些。体力劳动而已，然后再按F9啊运行，这个时候哦这个时候我微信崩了，我重启一下吧，重新附加一下。把这个删掉啊，这个靠还要这个靠我们需要用的。

然后。啊，现程全部恢复。直接我直接来搜这个标志我。一般来说会是第一个。By brought away。还是算一下吧。我。Yeah。啊什么ID。都开了呀。378482121320。21320换成16镜子。

5348，刚刚可能回加错了。5348，就是这一个。然后我需要把另外一个demo打开，不然的话没有没有开启。好，然后把检测给打开。首先是还是刚刚的步骤，先搜索0。然后在这个位置。

下双断点去给对面发一个消息，当它断下来，断下来以后啊，这个状态值实际上是被被视为了一。然后我们可以在这个位置去搜索一，再次扫描，是1009的，然后F9再次运行。这次运行之后啊。

这个值实际上是会被置回为0的。然后这个时候再搜索0。The。好，然后重复这个步骤啊。再搜断下来断下来搜索一，还剩下36个。Fphone9。搜索0那剩下23个，应该是最后再筛一次吧。断下来去搜一。然后。

那个9。在搜索0剩下22个，我们用取巧一点的方式。这个这个变量的值实际上是用了就两个绿色的地址。画起来看一下吧。这个是微信助手的一个机制，然后这个是U的32的，这个肯定不是。这个是那个用户模块。

那这个地址实际上是没什么用的。你可以去看这里在变动的一些值，实际上就不是我们要要的一个地址，这个值在变，这个是在变，把所有的不会变的值给拉下来。这个值不会变，然后我需要去动一下微信。

然后这两个值也是不会变的。OK继续往下去筛。中间还有机器是搜0的。这所有东西其实都在动。这个有点难搞了，这个值已经变成406了，这个肯定也不是。哎呀，好烦呐啊。😔，我们换一个方式就是。我给他全部拉下来。

找检测就是这么繁琐的一个事情。然后你会看到所有东西都在动对吧？把动的全部给删掉。前面的5个也不要。因为我因为我实际上是没有去调用这样一个发消息的一个功能。所以说这些数值如果在变的话。

那肯定是跟检测的代码没有关系。这个只现在被置为一，那肯定也不是。130这几个也删掉了。然后。这三个也不要。还剩下12345。Yeah。O还剩下5个，再再断一次。发一个消息，那，这个也是一。然。

还剩下4个了。这个也在动，没有用。还剩下3个然后好，这个位置被置为一了。我在我现在是断下的一个状态。然后这个标志位其实就是我们要找的一个检测标志，剩下两个M。好，然后放开断点。

找到了这样一个检测的变量之后，其实就可以通过内存的一个访问关系去找到检测的代码。我在这个位置去下一个。滴D这样位置，然后在这里断点。下一个内存写入，然后去发一个消息。好，不下了。

这里就是第一块检测的代码，它首先会把一放到这样一个地址，然后就弹出对栈。那实际上在F9并行，然后再。就是现在看我们的1个PPT，首先把这个位置给置一，对吧？然后再正常检测这个变量的值是不是等于一。

如果等于一的话，就正常执行我们的功能代码。然后现在走到了发消息的这样一个功能。然后第二第二个段的位置，就是把当前的这这样一个值去给它还原。然后假设说如果这个值不为零的话，啊。

那实际上就是有2块的一个代码。OK那假如说是我直接用。这个东西去发消息会怎么样？检测到外挂。断下来的第一个地方，它还是会把这个位置去给它制联。

但是上面的这一块实际上就是我弹出的一个myss box的这样一个代码。它会判断这个位置这个变量的值到底是不是一。如果。如果等于一的话，那就走正常的流程。实际上这一块你可以把它看作是检测的一个代码。

我只是用弹框这里去代替了。如果不等于一的话，那就说明你是用外挂去调用了我这个功能，那我就会执行一些相应的一些处理处理手段。好，然后找到了这样一个检测代码了之后，那实际上其实就有两种处理方法。

第一种就是我先把这个断点给删掉了。我可以直接去修改关键的一个跳转。然后假如说它等于一的话，我就直接跳过，我把这个值去改为降本。那实际上我现在用这个外挂去发送消息的话，是不会去弹窗的对吧？

那这个检测实际上就是。这个消息确实也是发出去了。O。这是第一种方法，通过修改游戏的代码来过掉一个检测。然后但是我比较推荐是第二种。第二种就是你不是已经找到了这样一个变量的一个地址了吗？那你在调用之前。

先把这个位置给制成一，其实就可以了。然后在调用之后，它会把那个东西给置回去。这样做的好处，实际上是不会修改到它游戏原来的代码，因为可以有效的防止有别的一些代码的检测手段。不改能能不改代码的前提的话。

尽量不要去改它的代码。两种处理手段，一个是改代码，一个是把值给不回去。好，这一个就是完整的一个变量检测的原理跟处理办法。然后我们再来说一下第二一种是堆栈检测。假如堆栈检测的原理。

实际上跟变量检测有一点类似。嗯，因为为什么会有堆栈检测这种东西呢。因为变量检测实际上有一个很大的缺陷，就是说它有一个全局的一个地址会放在内存里，只要这个地址存在。

你就很容易用一些内容搜搜索的工具把它找到。那只是一个实践的问题。刚刚我自己写的那个代码，其实搜了也就10分钟左右吧，还不到就就会过了。所以呢他把检测的代码还原到堆栈，堆栈检测的原理，实际上就是在。

调用函数的时候，检查这个是当前我只在代码里正常执行微信的一个函数的一个调用站。你会看到所有的模块的反馈地址都是微信的主模块，叫为cha win哦，假如说你去写了一个发消息的靠ll去调用的话。

它整个的一个那整个的一个调用站是这个样子的，就是这个是整个的是你自己的一个。代码的一个模块gametex是我的DLL的一个名字，然后再往上就是user32，就个是图形界面相关的东西了。

只要你写的界面其实都会有这个东西。所以基于这个区别呢，我就可呃我就可以在站里面去检测你的返回地址是否是相同的。好，我们来同样还是先来看一下效果吧。我先把对站检测给打开，同样我现在正正常去发一个消息。嗯。

断点没关。我正常去发消息，它是不会它是不会断的，它是没有任何反应的。好，那假如说我现在去用用这个东西去发消息的话，如果谈一个提示说是堆站检测的外挂，这个跟刚刚那个提示是完全不一样的。OK讲完了原理之后。

我们直接来看一个下效果吧，直接来看代码就行了。这个确实没什么好说的，我是在发消息往里面。在这个。把消息内部下了一个钩子，然后这里实际上是它这个是它VM的一个代码，它这里是跳转到跳转到了一个。

BM的一个代码，它下面是这个是密钥了啊，我给它还原一下，看看原原来原来长什么样子。原来这个是密钥，然后这个靠从这个位置往里进，就是VM的一个减码循环了。你要想在里面再动一些手脚的话，实际上就比较困难。

因为它VM的那个校验手段实际上比较多，所以我选择最近的位置，也就是这个位置去下一个钩子。下完钩子之后呢，okK下一个断点哦，然后我先正常去发一个消息，让它断下来走一下这个流程。正常来说走到这里。

然后继续往下走，先保存一下当前的计存器，然后把ESP加24的位置的值保存到EXESP加24，就是第一层的返回地址，记住这个地址是我们现在的一个微信的主模块。

然后把这个地址存到一个主变量再去比较这个位置的EX的值是不是跟里面的呃6A641A看下。641AB1。54里面存的值就是我事先准备好的一个。我是不多。你想算。数据窗口跟随内存例子吧。

里面存的值就是我事先计算好的这样一个返馈地址。假如说这两个值相等的话，那我就正常去执行一个代码，中间是。中间是message box，就是我写的那个检测的外挂的那个提示。好。

这个是正常发送消息的一个检测的一个流程。我再来看一下，我用。现在的代码却发生消息，那流程会怎么样子？然后跳转继续往下。这个时候ESP加24位置的值，实际上这个返回地址就是我们的DLL的模块了。

那在这个位置去对返回地址做一个比较的话，它的校验肯定是不会通过的，所以就会走。下面的一个检测的代码了，这一段你可以去把它当成是检测的代码。我这个demo写的是稍微简单了一点。然后在你这次运行的时候。

它就会弹给你弹一个提示啊，不过正常来说，它是不会取，我这里实际上是拖了一个来，直接去检测当前的一个返回地址，是不是等于一个绝对的一个数值。正常来说它会去取一个模块的一个区间，比如说是主模块叫这个吧。

我会去拿到当前的一个基值跟大小，然后判断。第一层的返回地址是不是在这个区间范围之内，如果在的话，那就说明你这个功能是正常执行的。如果不在的话，那就说明你可能是调用了一个外挂这个样子。好。

这个就是变量检测的一个原理啊，这是一个最简单版本的一个demo。然后再来说一下过的方法。Yeah。哦，还有还有另外一种变量检测的一个方法是有一个靠，然后这个靠里面呢会有一些堆栈的一些数据用来校验。

这种靠其实这种检测其实看上去根本是没有什么意义。因为你在找一些功能号的时候，你肯定优先去考虑一那些可以简单最快速被调用的这样一个功能。如果说它里面的数据比较复杂，你看了都觉得构造不出来的话。

你肯定是不会去选这样这样一个功能的。所以说这个检测实际上是没有没有太大的意义。然后再来说一下过的方法。对栈检测实际上是有两种一种是一层的，一种是多层的一层的话就是我只校验一层的一个返回地址。

就是啊在这个位置，实际上我是我是只对一层一个返馈地址做的处理，我只比较了一个值，还有一种是多层的，就是我会比较多个对栈的返馈地址。假如说有一个对不上的话，那我就默认是呃调用的外挂。

我们先来说一种对症检测的返回地，一种对对阵检测的那个处理方法吧。第一种方式找到检测的代码直接给它处理掉啊。但其实这种方法不是太好。因为它没有一个内存的一个访问关系。

你其实没有一个很好的切入点去找到检测的代码，它不像变量检测，你可以先找到那个变量，再通过变量的一个访问关系，找到变量检测的代码。但是这个它是没有它是没有一个访问关系。所以你只能用第二第二种方法。

就是去修改自己的程序去伪造这样一个返回地址。伪造出来这样一个反馈地址。首先你需要去下一个钩子吧啊，在函数头的位置先存一下本程序的一个反馈地址，然后再把。原来再把返回地址改成游戏里面的一个返回地址。

然后再函数的尾部，再把这个值给改回来。不然的话你整个程序就会崩掉。这样的话，其实就可以过掉它中间的一层的对栈返回地，堆栈检测。然后这个因为你在自己的代码里面，你是没有办法修改自己的返回地址的。

所以这种方式相对来说会比较麻烦一点。你必须要去写一个勾写一个勾子，然后多层的处理起来就比较简单了。多层的话实际上你是可以去提前布局好一个对栈的。假如说你你要去调用它的功能靠的话。

在调用它的靠之前先拉伸一下对栈。然后不管它去调用几层，我就是处理几层，先先把第一层给改了，然后再把第二层给改了，改完了之后先调用靠，再把对栈给还原。这个值实际上你也不。你也不需要去保存，也不需要去记录。

因为直接把对账还原回去，所有的指都其示做已件没有了，处理多层的方法要比处理一层的方法要简单。好，然后这个就是A站检测的一个原理。最后先来做一个简单的小结吧，两种检测的方式，一一个是变量检测。

一个是堆站检测。这两个其实都是针对ca的一个检测。假如说你在实际的呃一个游戏里面。遇到了你在调用一个靠的时候，发现怎么也调用不成功。那你首先应该考虑的就是堆栈检测啊，不你首先应该考虑的是变量检测。

先去先去用CE搜一下，能不能找到这样一个变量。如果搜了搜了半天，发现还是没有的话，那多半是调用了用了一个堆栈检测。那实际上现在大部分的游戏用的都是这样一个检测方式。是有一个外服的一个游戏叫做完美国际吧。

他就是用了一个多层对栈检测的一个方式，然后还有那个叫呃魔兽世界也挺火的那个东西好像也是有一个对栈检测的吧。在你在你找不到变量检测的时候，那就要考虑用布局堆栈的方式去把这样一个东西给过掉。好。

这个就是两个的一个综合的一个解决方案。前面两种是针对Cd检测。现在我们来说一下，针对篡改数据的检测方式。就是第一个是CRC检测。先来说一下关于CRC的原理。百度百科上抄了这么一段话吧。

它叫做循环冗余校验，就什么什么什么什么鬼，我也看不懂。呃，你只你需只需要记住一个东西就是。它它是一种唯一性的一种算法，特点是只要改了一个字节，那整个CIRC的数值都会发生改变。我来举一个实际的例子。

Yeah。随便截一个桌面的图，然后把它保存成一个文本。好，然后用二进制，我现在用用一个哈西工具给它计算，它会根据我里面的所有的二进制的一些数据算出一个值。MD5值肖万值，还有CRC32。

一般在游戏里面存的值都是CRC32，因为这个值会比较小，可以一个一个deor就能保存。你如果要存ND5的话，那实际上是存不下的。然后它现在值是这个嗯。找我现在在用二进石工具。🎼，是随便改一个字节。

ctrol S保存。然后再把它关掉。我当我再把它拖进去的时候，你会发现这两个值他们算出这些所有的ND啊，12万还有C273啊，它就是一种。叫做单向散列函数吧。对，就叫这个东西。它所有的值都会发生改变。

所以说它是一种它不是一种游戏检游戏的检测手段，它是一种文件校验的一种方式。只要你改了一个字节，整个数值都会发生改变。那应用在游戏上的例子其实就是。

我先算你我先对整个关键的一个模块算一提前算一个CRC的值出来，然后单独在主模块去起一个线程。这个线程一直在对所有的呃模块的一个数据进行读取读取完了之后算这样一个算这样一个含稀值。

假如说这个值跟我原先算出来的不一样，那我就默认这个代码实际上是就是被改改掉了。这个就是CRC检测的一个原理。然后啊我这里没有做例子，那个代码写到一般崩了，然后就直接来讲一下原理吧。处理方法其实也很简单。

因为CRC检测，它需要去实时访问整个代码段，然后去计算CRC的值，所以就存在这样一种访问关系。基于这个特点，那你只需要在代码段下访问断点，直接断下来的那个位置，就是游戏的检测代码，没有第二种情况。

因为你再去想一件事情，有需要访问到游戏代码段的只可能是。检测的代码，其他的地方不需要访问代码段。你呃代码段实际上是有可执行属性，但是它是有啊跟这个没有关系。所以说你只需要在这个地方去下一个访问断点。

直接就能断到校验的代码，然后去修改一些关键的跳转也好，然后直接去给它not掉也好，总之你找不到代码，所以你怎么去处理都可以。然后这个是CRC检测的一个处理办法。然后再来说一下数据检测。

数据检测的原理跟CRC原理完全一样，只有一个区别。区别就在于代码段不需要访问其他代码段，但是数据段是会被代码段访问的。什么意思呢？假设说现最左边的这一张图是是CRC检测的一个图。

我的正常的代码只会被检测代码访问。所以说我只要在这个位置下一个断点。然后呢断下的位置一定是检测的代码，没有别的代码。那数据检测数据检测，实际上是针对数据段做这样一个唯一值的CRC校验。

区别在于不仅仅有检测代码需要访问我的数据段，我游戏的正常代码，我也需要去访问数据段，因为它的数据里面可能有一些字符串啊，包括包括一些NPC的名字，还有一些图标，我需要实时去读取。

然后这个时候就多了一个多了一个步骤。你需要在所有的你可能你你如果是去过CRC的检测的话，你只会找到一条访问代码。你如果去过数据检测的话，你可能会找到多条访问代码。

那区别就在于你需要在多条的访问代码里面找到那一个真正的检测代码。好，然后这里有一个经验，就是说检测代码一般不在游戏的主模块啊，在其他模块。但它如果去写到一起的话，那你只能是慢慢一个一个去分析了。

也是一个体力劳动劳动吧，没有什么基础含量。这个东西游戏检测这个东西说起来很很高大上。那你。知道原理了之后，其实也就那么一回事。好，最后来做一个总结变量检测处理方式是利用CE去搜索出检测标志位进行修改。

这一种双相对来说呃缺点会比较大。然后第二1种利堆栈检测，利用户可的方式去过一层堆栈检测，然后用内联汇编伪造堆栈的一个方式去过多层的一个对栈检测，然后是CRC检测。

利用访问关系像内存端点定位到检测代码进行处理。这种也也是比较简单。然后第21个数据检测。通过访问关系定位到检测代码以后，需要对所有的东西进行一个排查。好，然后今天的内容就讲完了。嗯，好。

那非常感谢呃鬼手大咖的精彩分享。比如是还有一个要跟大家分享你的，如果大家有感兴趣，想跟大咖交流，可以添加一下大咖的哦QQ后面还有一页。😊，对，大家想之前有小伙伴说想能不能加上大卡。

然后大家可以扫码加一下这个QQ嗯，那非常感谢我们鬼手五六大咖的精彩分享。听完本期直播内容，相信大家都学到了不少有关各类游戏检测的原理，还有处理办法了吧。那话不多说，接下来就是咱们的行长问答环节。

我看大家在听讲过程中提出了许多问题。如果现在还有疑问的话，也可以继续在聊天区提出鬼手五六大咖会给大家解答疑问。那么在稍后的福利赠书环节，大咖会根据聊天区的交流情况，选出一位幸运观众。

所以大家抓紧机会提问啦。我们也请鬼手五六大咖打开直播间看一下观众的聊天页面。如果弹幕比较多的话，可以勾选一下直播间右上角的只看提问来查看问题。😊，那我们现在请鬼手五六大咖开始解答问题吧。啊。

你至于踪过APT病毒吗？有啥报告能分享啊，这个其实对于APT来说的话，你可以去网上找一下那些相关的，你可以关注一下。那几个安全公司的一个公众号吧，就他们推的APT还是挺多的。

齐安县有一个叫威胁情报中心的前身叫红雨迪团队，那个公众号就叫齐安县威胁情报中心。然后还有就是包括360，其实也有360，还有安天啊，所有的做安全的一类公司，其实都有他们的一个APT的运营团队。

那里面的报告，但是他们的报告不是纯技术性的一个报告，是。这样一个故事型的一个东西吧，就是可能不是你想要的那种。他们针对的人群是稍微有那么一点安全意识。

然后会通过一个讲故事的一个方式去把整个的一个事情告诉你。然后之前。之前齐源县还出了一本书，叫什么APT什么什么鬼，那个那个也是跟这个他会他会把所有的一个。APT的一个来源，包括什么时候捕捉的呢？

比如说海淀化，他们几几年捕捉的，然后这个东西的发展历程，包括他们组织干了一些什么事情，然后是怎么挖掘的，把把所有东西都给你讲的一清二楚。你可以去看一下那本书，但是呃纯粹是讲故事的一个形式。

你想要从里面学到一些技础的话，嗯就就就算了吧。好，然后这个这个问题算就回答完了。Oh。But。Yeah。反外挂跟反病毒都是什么区别？本质上来讲，我觉得都没区别，都是去看汇编。

只不过是但是反外挂它的知识体系要比呃反病毒的要要全一点。对，就你发现就刚入行的时候，假如一个去做反病毒，然后一个去做反外挂的话，过个两三年之后你会发现。反病毒做反病毒的那个人。

然后他聊的东西跟他完全听不懂做那个游戏安全那一块的东西。就他们接触的东西会是比较高深，而且也是整个行业最前沿最前沿的一个东西。因为游戏行业这个这个东西本身就是比较多金的一个行业。Yeah。嗯。

我看一下还有别的吗。为什么只有他一个人的提问呢？啊，那个只谁这个这个弹幕跟下面的这个有什么区别吗？呃，只看提问是在你演讲的过程中，小伙伴提问，然后咱们勾选出来提出的问题？你你可以把那个只看提问去掉。

然后但这个就是小伙伴们的聊天区了呃，那一项有什么就业方向，实际上啊进制这一块windows的话，就业方向其实还是挺多的。一个是嗯一个是竞品分析吧，就是什么叫竞品分析举个例子。

假如说我的公司是做photoshop的。然后你的公司是做美图秀秀的，那如果说我有这样一个人啊，photoshop说了一个很牛逼的一个算法。然后假如说我在美图这个公司，我我如果把你那个算法给列出来的话。

那是不是用用美图秀秀的人就会更多。那实际上竞品。分析就是去分析一些竞争对手的一些功能，然后把里面的一些东西转换为源码的一个方式，然后发布到一个产品。就这样一个这样一个方向，这是其中一个。

然后还有一个分支叫做反病毒是各大安全厂商会招的一个一个岗位。但是这个的话相对来说招的比较比较少。因为有杀毒软件的那个安全厂来来回回，也就那么几家嘛。然后还有漏洞漏洞也是一块。

但是只有一些安全公司的一些实验室，他们用作研发，用来做那个呃知识存储。这个其实招的也不是很多吧，然后。精品分析是一个啊，反病毒是一个，然后漏洞是一个。然后游戏安全又是一个。

基本上如果在windows这一块的话，游戏安全应该是我觉得前景最好的一个行业呢。因为它本身业务赚钱，然后业务赚钱的话，你拿到的钱就多。而且它所需要的涉及的一个知识体系也很全。

它适合去做这样一个中级的一个岗位。假如说假如说你现在的段位已经到了是两三年这样一个水平的话，你去做这样一个事情会会是比较实效，而且钱给的也多，重点是给的钱多。Yeah。逆向怎么进一步精进？没人带的话。

其实windows的这一块，其实windows你向来来回回就是那么一点东西，你只要会看会编，然后能分析几个东西，你就算是入的门，但是精进的话还是需要看你具体做过什么样一个项目。

你在你在你在进入这样一个初级岗位的话，考察的是你的一个基基础的一个基本功。然后就是你对windows的那些，比如说是沪口de有猪助啊，还有是汇编啊，还有P文件这些基础知识的考察。

那如果你想要有一定的积累了之后，想要进一步的监进，就需要有一些运气成分在就是说你不要进到那一类的躺平的那一些公司。就是说你在那些公司干下来干了两年之后，你手里没有一个能拿得出手的项目。

你的项目部值钱的话，那你的技术也就没有发展的空间。那你下一次跳槽的时候，可能就会很被动。所以说如果你想要从初级岗去进阶到这样一个中级的岗位的话，你应该去在你。第一次第一个安全公司选的时候就要选的。

眼光要放的远一点，就不要选那种躺平的一个公司。你怎么精进取决于你做的项目到底有多少含金量？你做的项目就是你的一个技术水平的一个提升，就是这样的。安游戏安全能拖数据库吗？う。😊，拖上什么呀？拖出就裤。

你说你脱库的那个东西应该是叫社工吧，进不了这样的含金量工资怎么办？你如果是有能力够到初级岗的话，是你能不能够到其实你想够你想够到一个初级岗的话，你只需要去自学。

或者是随便去一个培训机构都都能进这一个初级公司。然后进来以后，你进了那里，但不能不能代表呃一切的东西。比如说我现在就做的是一个啊反病毒的一个事情。但是他每天干的事情就很无聊，就一天可能看几百个样本。

然后我会去业余去研究一些别的东西。那比如说游戏这一块的东西，你可以找一些视频来看，然后自己去自己去做一个项目出来，自己去做一个完整的一个项目出来。然后你想经起安信，进起安信还要思路吗？那不随便境吗？

你可以去做一个完整的项目出来，然后把这个东西写到你的简历上。假如说你这个项目的含金量够高的话，也也是可以顶替你的也是可以顶替你的工作经历的。我工作上的项目其实没有太大的一个。含金量的水准。对，是这样的。

对。Yeah。搞的C1加加，你要你要逆向了，你肯定要会争选了。in我不会呀。嗯。红队，你问的那个是属于呃网那个应该是属于网络安全的那个技术站了。那个跟我我现在说的是关于底层的一些东西。含金量是怎么看。

怎么评价的，这个说这个就不太好量化了，说实话这个不太好量化了。好，各位小伙伴们还有什么疑问？或者是针对今天晚上讲的议题，大家有没有什么想问我们鬼手大咖的？学历要求本科起步。

还是有一些公司不会去卡学历的吧。起验线就不会去卡啡利。Oh。我的g，你直接你直接你直接搜ID吧，可以说5个6，直接搜就能搜到5。好，那大家还有没有什么问题？再翻译下。Okay。嗯。그。

我我用的是全局变量，全局变量又是在PE的哪个段？全局变量好像是在那个全呃windows的内存布局里面，全局变量是在全局数据区的，在那一块地方。如果在调用这个靠之前，给这个断点下断点。

那就如果再调用给这个断下断点再调用，那样不就知道用的是哪个变量了吗？但是你在这个靠下断点。是下断的时候，你怎么找到这个变量呢？你肯定是要先把它给搜出来呀。

是边下断边用CE搜索的一个方式才能找到这样一个变量。不然的话你光下断肯定是在那么多数据里面，你怎么找到那唯一的一个值？嗯。鬼手可以带来吗？日常QQ问些问题，我操。这是干啥的？Yeah。

你要是请我吃个饭的话，我可以考虑考虑。嗯，那咱们今天晚上这个提问情况也差不多了吧，就大家没有什么疑问了。鬼兽大咖可以把刚刚那个呃联系方式在最后给亮一下。好的，大家就赶紧加啊，错过了就错过了，真的。

那我们也非常感谢鬼手五六大咖刚刚耐心的解答小伙伴们的疑问。那由于时间有限，大家如果后续还有什么问题的话，可以加入我们的技术社群，或者是添加鬼手大咖的联系方式跟他继续交流。

那么接下来就即将进行到今晚的福利放送环节啊。鬼手大咖亲自挑选的书籍逆向工程核心原理将要送出。那鬼手大咖可以说说为什么想送这么一本书。因为这一本算是呃逆向工程里面入门级含入门级的一个书吧。

他比较对新手比较友好，而且封面长得也好看。啊。颜颜值卡啊，那好的，那这一份幸运的礼物究竟要落到哪位观众的头上呢？我们有请鬼手五六大咖滚动大屏幕，来选出今晚的幸运观众吧。

你可以在想要送书的观众ID边上点击设为幸运，除了管理员，就普通小伙伴啊，然后我给我报一下他的ID然后我们后台的运营小姐姐也帮忙记录一下，那就那就这个吧，我回答的基本都是都是他的问题，叫缺手不偿吧。😊。

吹水日常吗？好的，你点击设为幸运。嗯，那我们恭喜这位直播间ID为吹水日常的小伙伴，即将获得这一本逆向工程核心原理。那需要你根据直播间的获奖提示，在相应的区域留下正确的收获信息。

或者在直播后联系我们私聊兑奖，我们会尽快将书给你寄出，那希望你耐心等待。那今晚到这里，我们的直播就要结束了。鬼手五六大咖还有没有什么最后要跟大家说的。嗯，没了，那咱们就聊完了。嗯好的。

那再次感谢鬼手五六大咖的用心准备和精彩的演讲，希望本期的知识，大家都能学有所得有所启发。如果你想回顾本期直播，我们将在下周五的时间发布录屏，尽请关注官网的更新和群里的录屏更新通知。那今后。

大家也可以多多关注我们的鬼手五六大咖。最后感谢所有观众伙伴们的守候，还有对咖面的支持和喜爱。如果你也想像大咖一样直播分享，欢迎找我们报名，大咖面对面是一个展示白毛风采和传播技术的舞台，不具年龄。

不畏资历。只要你有才华敢分享，我们都欢迎。那如果想进群交流的话，可以在页面的底部找到群后，本直播间地址固定，那大家可以收藏到浏览器。今天晚上的直播到这里就要结束啦。感谢各位小伙伴们的积极参与大咖面对面。

周五8点见，我们下次再约吧。那下面是听歌时间，小伙伴们早点休息。鬼手五六大咖也可以跟大家说再见啦，拜拜。😊。



![](img/b353d85daebf75f275e682d3a9d29a11_4.png)

![](img/b353d85daebf75f275e682d3a9d29a11_5.png)