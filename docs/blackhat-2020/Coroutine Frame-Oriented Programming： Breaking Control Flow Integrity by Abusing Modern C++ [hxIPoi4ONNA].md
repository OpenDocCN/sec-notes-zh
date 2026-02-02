# Coroutine Frame-Oriented Programming： Breaking Control Flow Integrity by Abusing Modern C++ [hxIPoi4ONNA]

Welcome， everyone。 And thank you very much to Black Health having me。😊，50 years ago。

 in this document that you see here， we knew for the first time that corrupting the memory of a program could lead to taking control over the instructions that it executes。

We didn't know yet， but this was the very first mention of what we know today as a buffer of etlo。

And since then， we， the security community， have gone so far， both defenders and attackers。

On the one hand， defenders have created new better ways of fortifying this memory。 So we got alR。

 We got nonexecutable stacks。 And on the other hand。

 attackers have always found new ways of bypassing these defenses。 So we learn how to leak things。

 We learn how to do code reuse。😊，But eventually， on 2005。

Academics introduce what we know as control flow integrity。This is a little bit different。

And since then， these kind of defenses have slowly but sore gone into everyday systems。

You could say they are already here。Contraflow integrity。

Takes a different approach from the classic defenses。

 Instead of trying to make exploitation more difficult。

 it actually pre tackleles the problem at its root。

 which is the ability to control the instruction pointer in the program。

So what they will do is to use a static and dynamic methods to analyze your program。

 and it will construct what we know as a control flow graph。

 And this control flow graph will describe the transitions between functions inside the program。

 And after that， control flow integrity will instrument your program so that it actually forces these transitions to be the only ones that happen and not the ones that attack ones。

😊，And， yes， this is have very significant level up from the defender side because in essence。

 it prevents one of the techniques that we have been using for a very long time。

 which is return rental programming and every other codere technique out there。😊，But， of course。We。

 it is the time that attackers also level it up again。

 So we are not the first to suggest a way of bypasing Cfi， of course， But today。

 we point out a new way of doing it in something that is in C plus plus core。😊，My name is Marcos。

 and I'm a PhD student from CISPA in Germany。 I do research on system security and also malb。

 and yeah， I have a couple of purchase around。 So just check it out if you are interested。😊。

This project has been developed。 thanks to the， to my supervisor， Kristen Rosso。

 He is the leader of the As security group。 And together insist we do very awesome things。

 So feel free to reach out。 We have very cool projects。 So， and we are very happy to。

 to have feedback about it。😊，This is what we' are going to do today。 First。

 we are going to go very quickly because this is not a CI top。

 We are going to go over the existing use of space CI defenses。

 So we are going to go over how they work。 After that。

 we'll look into how these proteinss actually work under the hood and will under certain things that I consider to be very interesting。

😊，Then we'll actually have fun because we are going to take these primitives that are for security insecurity primitives。

 and were going to do attacks with them。 And finally， we'll suggest some defenses。 So let's go。😊。

The first most probably the most popular one out there。 The most popular CI scheme is Intel C。

 It's hardware assisted and it's coursearway， meaning that the instrument station that is in the program is not so strict in terms of enforcing the controlflow graph of the program。

 but it's basically two in one。 On one hand， we have subt。

 And this basically happens every time you are calling functions inside the program。

 So you will have something like when you're calling one function to another。

 you will push the written and address to the stack。 And this happens for every function。

 And in a separate memory page， which is not accessible by the rest of the user space。 you will have。

😊，At what we know as the Sa stack。 And this is not accessible by the normal user space。

 but will only store the return addresses。 And when youre returning from the functions。

 you will compare the values。 If they are the same all good。 If theyre not the same。

 then you will have a fault。 And this might happen because you might try to overcide the return address。

 right。The other complementary part of In city is indirect branch tracking。

 And this one is for protecting indirect pointers and indirect functions inside the program。

 So essentially， we can， of course， write the the indirect pointer in this case。

 and you can jump whatever you want。 But withindirect branch tracking。

 what you will have is a landing path based on an NDR instruction that is compiled with your program。

 and it will enforce that every indirect jump always lands in one of these instructions。

 So you can essentially only call functions from the very beginning。😊，Then we have controlflow guard。

 And it turns out that IBT， although Windows does have Ct。IVT is not in Windows。

 They decided to go this other path， which is essentially equivalent， but it's not how we supported。

 Instead its solve。 it happens during the completion of your program。

 So it will translate every single indirect call in your program to a direct call to this other function that you see here。

 And in this function， there will be a series of mathematical operations that basically check again say two bit map that tells you whether this call is allowed or not。

 And the most important one is， if you have10。 And this will tell you， okay。

 so you can jump to this function from the very beginning。 So if you check out probably any。

 any function in any D L， you will see oh， it has one0 meaning that I can call it from the beginning。

😊，Finally， we have not only coard rain CI， but also we have more fine rain stuff like like LVM CI。

 And in this case， what it will do is to compute some kind of dynamic type of every indirect pointer。

 So let's say you have this indirect function that is assigned to this other function like close。

 And what it will do is to define a prototype based on the return return type of of the function and also the arguments that it has。

 And it will enforce that every code goes the same to functions that have the same prototype。😊，Okay。

 so let's actually look into what is recruit。Cooutine is a function that can suspenense and resume。

 What do we mean with this。 When you have a function， you execute the function。

 you go to the function， execute it completely return。 And that's it。 And if you execute it again。

 you will actually go to the function from the very beginning again until the end。

 But with a co routine， we have what we know as suspension points。

 and suspension points are points in which you say， okay， I want to hold here。

 I will maybe resume later。 And if you actually call the core routine again。

 you will not go from the very beginning。 But rather you will go to the suspension point where you left it。

 And from that point， you continue。😊，So there are certain things that you have to do when in C plus+ to implement cor routines。

 One of them is the task object。 And you can imagine this as like the top most object where everything the corine is contained。

 Everything else that I'm going to explain to you now is contained in this object。 For instance。

 we have what we know as a core routineine handle。 And the handle is a pointer to the core routine。

 So basically， it will allow you to access the core routine and do things like resume it。

 meaning that I want to go to the last suspension point and resume it where I left it。 or destroy it。

 meaning that I'm， I'm don with this core routine。 And I will left it。

 And I will never be able to resume it again。😊，For a function to be a quaout。

 it needs to have one of these three different operators。If you have them。

 then the compiler will identify it as a core routine and generate different functions that I'm going to explain you later。

 The first one is co yield。 And this is the most basic one。 Basically。

 you reach it and you return a value。 And this is also counts as a suspension point。

 And then you have core return， which is basically the return equivalentence in a function。

 which basically allows it to return a value to。 and you are done with the core and you will never be able to resume it again。

Now， returning values works fun。 Finally， in in corine。 So actually。

 there's something we call the promise。 And this promise is some kind of objects where the corine and functions have a the space for ser data。

 So every time you return a value， for instance， you save it in the promise。

 And then the function later takes the value from the promise。😊，So when a corine is comp a quaine。

 three different functions are generated in the background， which I call the stops。

 The first one is the creation stuff。 and it creates an object that I will explain you later that defines the whole context of the corine。

 The second is that re stuff， which is in charge of actually reum the corine。😊。

And the third one is to destroy stuff， which is the one in charge of destroying the corine。

 We'll go deeper into this later。 So just to have an intuition how this looks in a real program。

 You have the task objects， the handle inside， the promise that I told you about。

 And inside this promise you have the return value and some other functions that allow you to do some customization。

 like， okay， what happens when the cor initially suspended and what it is final suspended。

 Tyical stuff that the programmers use with corine。😊，Now， corouts in C plus。

 and this doesn't happen in every programming language are stackless。

 This has certain implications for programmers。 But for us。

 what this means is that the core routine is only able to suspend inside the co routineine and not in another function。

 So imagine it cause another function。 You cannot assume from this of。

 you cannot suspend from this of the function。😊，And really。

 the main implication of this is that the compiler can generate a very optimal way of storing everything related to the co routineine in memory。

 specifically every time you have an instance of a qua routineine being executed in your program。

 you will have what we know as a co routineine frame。 And this is a heat allocated object。

 And you would have one for every qua routine that you're executing in your program。😊。

And it looks like this。 So you have the handle and the handle points to the resume pointer。 Sorry。

 it points to the core frame。 And this core frame， you have different things。

 The first one is the resume pointer。 And this pointer will point to the resume stuff that I told you before。

 And it's actually an indirect core， meaning that every time you resume the core。

 you will have an indirect pointer。😊，The second one is the destroy pointer。

 and it points to the destroy stop， similar like the resume stuff。 and it also is an in。

Then we have the promise where you say the written values， etctera， and the parameters。

 And this means that every time you're calling a co routine。

 like if you pass in some value or some pointer or whatever。

 it will actually be stored in the coout frame。😊，And then we have the variables。

 And this requires you to change your mentality， because usually， when we are executing functions。

 you know that everything is in the stack， right， So everything is going to be addressed by RP RVP。

 And you're gonna have to have all the variables there。 But in a core routine。

 everything is d in the hip。 like everythings in the hip already。 So every stack based variable。

 It's actually heat based variable。 And everything hip based variable is a pointer in the core frame that points to another qua to another heat based options。

😊，Final element in the frame is the corine index。 And this is an integer number that tells you whether you have on which suspension point。

 you have to resume the corine。 So for instance0 will tell you， okay。

 from the very beginning or another number will tell you， okay， from another suspension point。😊。

So what's really this stuffs。 The creation stuff is a call to Maoc because you need to allocate the quaine frame。

 right， So every time you actually create a quaine and use it for the first time。

 you're gonna have a call to Maoc。😊，And in this mall， after this malllo talk。

 you will initialize the core frame with the resume and destroy pointers。After that。

 you have the resume stuff， and it's like a switch。 It will take the corine index and say， okay。

 from which position do I need and do I need to resume the co and it will actually jump to the position that you are resing from。

 So the actual code inside the coine is in the resume stuff is integrated there。😊，And finally。

 you have to destroy stuff。 We just are called the free。

 So you're actually freeing the koteine frame。😊，I mentioned that there were multiple operators that I didn't mention the last one Co weight。

 And this is where the fund really appears because it's the most powerful one and the one you will most usually find in any program。

😊，It basically allows you to do all the fun stuff encouraging so you can do a synchronous job。

 You can do cooperative multitasking。 Let's， there's so much stuff you can do。😊，For instance。

 if you are talking about aable task， you can imagine this as， okay， have a task。

 which is very heavy at computational time。 So I'm just gonna put it here。 I leave it executing。

 And I'll go execute something else。 And once I am bored of waiting for it。

 I will go back and take the result and say， O， that's the result of my awaitable task。

 And to program this。 It's actually boring。 It kind of sucks。 So you have to use callback and。😊。

It is really dirty。 So programmers said， oh， let's use quaotes。 We have it nice and clean。

 And this actually really simplifies the things for every programmer out there。😊，Now。

 to be very specific， what Cooid does is to evaluate an avoidable objects。

And this avoidable object is nothing but a rubberper in which we will find an awaiter object。

 And this awaiter object is a real one， which is important because we will have three functions that are executed one after the other。

 The most important one is avoid suspend。 And in a white suspenense。

 we will have the actual things that happens once you are calling co weight。 Here you have。😊。

Creating new threats。 You might do things like resum another routine。 You can do whatever you want。

 Every problem has different things here。 And this is one of the functions that you should really look for when you're exploiting proteins。

😊，Just to show you， a very quick example， you are going to resume a routine。

 and this will co weight and a waiter。 This co waiter will call the three functions in the order。

 So you will have a weight ready after a weight topenense。 And in this case。

 we are going to execute some very expensive tasks， right， So I'm going to create some thread。

 Leave it there。 My other thread will go do something else。

 And this other tasks that is being executed eventually will finish。

 and then well resume the core routine。 meaning that you will be able to resume it wherever you left it。

 First， you call a weight resume。 And after what， you go back to the core routine。 And later。

 you want to joinable threads or whatever。 This is a very simplistic scenario。😊，Here's the fun part。

Co routines， and specifically， the task objects is unavoidable。

 And this means that you can do things like awaiting cor routines from our core routines。

 And this is very common。 So it， it allows you to do so much stuff like you may have two cor routines that cooperate in some kind of task。

 You may have one cor routineine that is in charge of maintaining a queue of other cor routines。

 And it is in charge of resuming them and destroying them。

 So theres so much stuff that is being used out there with cor routines。😊。

So let's look at what can we do with this。Our third model is going to be very similar to what CI assumes。

 which is， look， were going to go some functions in our program。 So obviously， you not need to know。

 which is the address of them。 So you need to have some alr bypass to know the address of these functions。

And we'll have some kind of memory corruption where there's gonna be some vulnerability that allows us to overwride some memory。

 I will go deeper into what exactly this looks like later。😊，Also， do you need some quaoteine。

 of course。And you will need to have CI in place in your program。 specifically。

 one of these 13 CI schemes that we have analyzed， and we will go over them later。

So the very basic observation that I'm sure of you have done at this point is that the qua routineine handle and the frames are inriable memory。

 And this is very bad， of course， because。😊，Based on this。

 we devise super primitives that basically appear every time you want to do an exploitation with core。

 The first one is frame manipulation， which consists of， oh， I found a core team frame here。

 So I'm going to overr this。😊，And the second one is frame injection。

 meaning that you will inject new frame， new frames into the memory。

 either the stack or the hip or whatever。 And you will link these frames， which are fake。

 but will now be used by the program like if they were real。😊，So since we are dealing with CI。

There's a very easy way not to trigger it， which is， let's not modify the pointers。

 So let's first look at what can we do without modifying the pointers。 This is what we call as data。

 only attacks。And it looks like this。 So you have a program， and we're going to have2。

 two co weight calls。 So three suspension points， because the first suspension point is the very beginning。

And maybe you have a parameter。 And this parameter。By。

And and this is something that happens in every core。 Para are always copied to the core frame。

 At the moment， you call the creation stuff。 So you will always have them there from the very beginning。

 meaning that you can do something like this。 You can have a co frame over Britain。

 put a value that you want there and actually put the core index so that you go to the third suspension point and execute it which whatever data that you want。

 So it's like a very powerful way of doing database attacks。😊，Variables are a little bit more tricky。

 So， for instance， let's say you have this variable。

 and it's really only defined on the first suspension point and then use on the last one。

It actually turns out that it's not。 the compiler is smart enough to say， hey。

 this variable is not really used here。 So I'm just going to initialize it in the last suspension point。

 meaning that even if you override the qua frame with whichever value that you want。

 you will not be able to modify this value in a database attack。

 So these are kind of things that you need to take into account。 For instance。

 if you have this other variable that is being used in some other suspension point， initialize。

 actually initialize。And then it's used in an over suspension point。

 Then this actually works because the value will be initialized in one of the suspension points。

 And you can modify this value for the next suspension point。

There's actually more stuff than modify data。 So if you have some kind of heat allocated object like a vector。

 this needs to be f somewhere， right， You have to call free on this。

 It's really always called the very last suspension point。 So you can do things like follows。

 Let's create a converting frame。 where I'm going to put a value some pointer instead of this vector。

 And what I will have is an arbitrary called to free in my program。

 And this you can imagine it's a very common very useful primitive。

 And you can do cool stuff with it。😊，Of course， you would have to have some metadata for the。

 for the chunk and etc cea， but。If you have ability to overgraride the memory。

 these are things that can definitely done。So really， how does this memory overcrowd look like。First。

 you can， of course， have an arbitrary memory right。 This is the simplest scenario。

 but you can also have a stack based overflow， which will overrite in the stack。

 the cor in handles that are used in the program。😊，And。This will allow you to say， okay。

 so I'm going to write a handle。 And now this handle， instead of pointing to the hip。

 maybe points to the stack again。 And in this stack， I'm going to have an arbitrary core frame there。

You can also have a stack base overflow， but inside the co routine。 And remember。

 every variable is a hip based variables。 So now。😊，You will have a heat based overflow。

 and there is no stack calories in the he。 the variable reordering that the compiler does。

 is's actually not really very well working with core routines。

 so you can actually overgraride many variables， if you're in P T molecule。 like in Linux。

 you might also be able to overgrard some core routine frames that are in higher memory addresses。

 So there's many， many things you can do。 And at the very very least。

 you will be able to overgra the core routineine index with such an overflow。😊，Of course。

 you can have an heat based overflow， the classic one where you just overwride the heat。

 and you might be able to rewrite some frames。😊，And it might be a combination of all this。

 So you maybe are able to free a pointer。 And this pointer actually allocates the next he。

 the next qua routine on top of the other one。 So there's lots of crazy ideas and。

 and things that you can do with this。😊，But let's look at what。

 let's forget about data oriented attacks。 And let's look at what can we do with the pointers。

 because as you can probably imagine， these resume and destroy pointers look very juicy for an attacker。

 And they really are。But we have to take into account that we are going to have CI。

 So we are not going to be able to do things like return address， hijacking。

 So no over return addresses in the stack。 Nothing were like overre some virtual pointers。

 This is going to be accounted for some of the other CI schemes that we have。 And also。

 the arbitrary targets for indirect functions are going to be very limited because we have so many fine grain CI there。

Or we， do we， Because it turns out that when we look at this。

 many of the CI schemes do not account for co routines。

 and they actually break when we compile them with co routines。 And most importantly。

 some of the CI schemes that are out there and our fine grain do compile the programs and they do generate instrumentation for our co routines。

 Sorry， they do generate the instrumentation for every function in the program。

 but not for co routines。 So they destroy and resume pointers are un instrumentnstrued。

 And this really shows us that。😊，These CI schemes need to be updated to with the how the programming paradigms evolve。

 because otherwise they will be left behind and they will not account for new things like the cors。

So in the end， we are left with two cord range CI schemes that are the most popular ones and are into city and world and will prevent us from calling functions from anywhere except the very beginning。

 which is still quite strict。😊，But， of course， yes， we can override these resume pointers。

 but they are not really useful right， because they were going to call one function， maybe two。

 although I tell you that not every time you're going to leverage both the resume and straight pointers。

 and they are gonna have serial arguments， which is kind of bad because we want to call functions with arguments。

 So we devise a way of using proteinss to actually have infinitely arbitrary memory infinitely arbitrary functions with arbitrary arguments。

 specifically， we devise what we call a control。😊，Frame pointer。

 And this is some pointer that might be the assume the three pointers。

 but also anything that entirely points to these pointers。 Let me。

 let me show you how this looks like。 So， yes， this is a CFP。 Both are assume the straight pointers。

 but also any handler that points the currentine frames can also be a point where you can override them that leads to to controlflow hijacking。

😊，You actually have lots of handles around。 They are very popular whenever you have a schedulers。

 And this is something that are very used in things like in browsers， in databases。

 I actually looked into some forums from Chonium， and they were thinking of implementing the the events loop from。

 from some of the internal。😊，Processes in chromroe with kine。 So this is something that is really。

 really being use， even if this was only in introduced in 2020 with C plus plus 20。😊。

And the third one is that you're going to have pointers that are internal to the qua frame and that I have not explained yet。

 but they are actually very interesting， so。I explain to you what happens when you co a co routineine from another co routineine。

 But how did you come back。 How does the other co routine routine know where to go back after it's been executed。

 It not like a function。 you just not return。 It actually uses continuation points。

 And do you find this in every program that is using this kind of a scenario。

 So you will actually say from the first coine to the second one， Okay。

 I'm going to set in your promise。 object that is called a continuation。

 And that is a pointer to my handler。 So that later when you finish executing your program。

 your co routine， you will resume into this pointer so that you go back to the previous co routineine。

😊，And there's a second thing， which is， yes， we are coaing co routines。

 but are we going to have in the hip lots of different frames。 Where， Where are these destroyeds。

 There must be somewhere where they are destroyed， right， And it actually， there is。

 So this is usually implemented as in the destroyer。

 meaning that once you go out of the scope of the， of the coerate call。

 you will have an implicit call to destroy， meaningan that they will have。

 you will have a destroy call to the coine frame that you just。You just coaed。

So every chine that you are going to be using has two， these two pointers， right。

 one that points to the next one and downward points to the previous one。And based on this。

 based on all the C FP that we have identified， we dev infinite qua chain。

 which is what we actually actually allows us to call infinitely many arbitrary functions。😊，For this。

 you will have， when you're exploiting a program， you will have to find two CFPs。

 two of these pointers that I explained you before。And it looks like this。

 So the attack consists of injecting lots of cor frames in the memory。

 And these cor frames will do something fun。 which is this。

 The first is that I am going to resume this malicious chain at the second suspension point right after a co weight call。

 And right before a call to destroy。 This will allow us to call destroy in an object that is in the cor frame。

 but which I arbitrarily control， meaning that I will destroy， and the next element in the chain。

 But I don't want to destroy it， I want to resume it。

 So I will modify the next cor frame so that destroy pointer is actually the resume pointer。

 meaning that when I call destroy it， I will actually resume it。😊，And this is how you go。

 So I assume the next element in the chain。 and the next element in the chain also has a hijacked continuation point。

 Sorry， no continuation point， but rather the object that I need to destroy。

 And this destroys the next element in the chain。 And eventually， you reach the last element。

 And in this last element is when you start making arbitrary cause。

 So you are going to destroy an object that you again control。

 And you are going to call destroy on an object that we call the trampoline frame where I'm going to put the arbitraryrarily pointer that I want to actually execute。

 So you may call here system or whatever you want。😊，After that。

 you return back and you will continue the execution inside the quaine until you reach the part where you're leveraging the second CFP。

 which was for us the continuation point。 This continuation point is going to called。

 is going to call resume on it。😊，And we will actually go to a new trampoline frame where we will go and resume this pointer。

 which is， again， another arbitrary goal。 And this repeat。 So we go。

 this moment we return back to the previous one。 We go again to a continuation。

 a continuation object。 We assume it execute another arbitrary goal。

 And this continues for as many arbitrary goals as you want to make。😊，So。In this way。

 we have arbitrary goals， but we don't have arbitrary arguments。 And for this。

 you have to know that every time you make a call with the resume and the pointers。

 the RVI register is actually fixed to the co routine handler。

 meaning that you will be pointing to the to the co frame itself。So when you call resume。

 it's not really very useful because youre going to have this memory where youre going to have the resume pointer。

It's not really a very useful argument to have。 Thetro is a little bit more useful。

 So you're going to have some by that you're going to be able to leverage there up to 8 B。

 but it's not really also not that useful in many cases。 So based on this， we decide， we look。

 we say， look， we control RDI， but not really what we point by RDI。

 but is there anywhere else where in a program made in C plus plus do use RDI。😊，And actually。

 there is。 So every time you use some function that is a member function of a class in C plus plus。

 it will these functions will actually use offsets from， from this pointer。

 which is actually used as an offset from RVI。 So every time you access a member variable in an in a class。

 you're actually using an offset from RVI。 And based on this。

 we dev what we call a collision where you're going to have your offset and it's going to load at a certain offset from RVI。

 But also， we are going to have a core frame where we're going to put at this exact same offset of RD I the values that we want to load so that when you call this function。

 you actually load them in the race that you want。😊，You mind this function， if you're very lucky。

 Maybe that makes another indirect call， which in this case。

 you can set the races and call the indirect， the indirect function altogether。

 Or maybe it just sets the races returns， and you leverage the next CFP for actually making the indirect call with all the races already set。

😊，So I look online line and say， well， which is the， the programs out there are using cors。

 And the most common one right now is the Windows terminal。 But that wasn't very interesting。

 So I saw this solar one， which is the serenity O S operating system， which is an open source O S。

 and they actually use。😊，This thing。 So they， they were playing around with quaotes for their browser。

 and I looked into this function， and I identified2 C P。 So I saw this destroy pointer。

 and I also noticed they are doing qua weight。 So maybe I have a destroy call here。😊。

And they actually do。 They actually do these CP。Unfortunately， they are not。

 they do have these codes， but they are not really using the codes yet。 So they have the code。

 but they're not calling it。 So just for this book， what I'm going to do is。

I'm going to use a real C， which is from 2021， to overwrite an existing pointer in the program。

 which is an indirect code that you control with zero arguments。

 And I'm going to use this to make three different indirect code that have arbitrary arguments。

So here in the left in the right， you have the web content process of the browser。

 and this in this process is going to be running with Intel C。

 So you can check this out under the profile system。

 You will see there that it has the subway stack labels。😊。



![](img/d19e12f1e6592c55829491c4cc440cb0_1.png)

What I'm going to do， is to， as I said， leak where this pointer is。

 And this is a integer overflow that will lead to some memory overritedes。

 And this memory overrite will be used to leak a pointer that I'm leaking there。

 So I'm going to use a malicious website that I prepare with the C where I will actually overr this pointer。

 and this pointer will jump to the to the functions that I just show you with multiple CP。

 And as you see there， I have arbitrarily many calls。 So I'm calling who am I multiple times。

 But also， I'm doing something which requires more arguments。 So as you can see。

 I'm really not limited to how R V I or any other racer really look like。😊。



![](img/d19e12f1e6592c55829491c4cc440cb0_3.png)

![](img/d19e12f1e6592c55829491c4cc440cb0_4.png)

![](img/d19e12f1e6592c55829491c4cc440cb0_5.png)

![](img/d19e12f1e6592c55829491c4cc440cb0_6.png)

![](img/d19e12f1e6592c55829491c4cc440cb0_7.png)

![](img/d19e12f1e6592c55829491c4cc440cb0_8.png)

![](img/d19e12f1e6592c55829491c4cc440cb0_9.png)

![](img/d19e12f1e6592c55829491c4cc440cb0_10.png)

So。These cors are available in every in every compiler out there。 So they are in GCC clang MSBC。

 and they all work the same in Windows， actually， you might find some other complications and other things that you have to take into account where you at the moment where you're trying to exploit this cor。

 So， for instance， you you will find that chunks are randomized。

 So maybe with have simple stack based overflow that has turned into heat base overflow。

 you might not be able to override core frames that are in higher memory addresseses。

 So you can also do do and and overrite things inside the current core frame or override the core handle。

 So there's plenty of possibilities still。😊，Bypassing CFD， as I said。

 is going to be very similar to bypassing IBT， because you will have just call the functions from the very beginning。

 And there's going to be other things like changes in race convention， etca。

 But you can still do the the same tricks that we did for having arbitrary arguments。So when。

 when we found this， we said， okay， I understand that you need to have these resume and destroy pointers in the cor frame。

 because the scenario you have with protein is that many times you're going to put them somewhere and you're going to forget about them。

 And then you need to come back and say， oh， this is how I resume the code。

 So there needs to be something in the corine frame to actually tells you how to resume and destroy it。

 But it's not very good to have the pointers there。 So because we suggested it was to say， okay。

 let's have an an idifier some way something that tells you。

Where to look in some kind of table that is in read only memory。 And， yes， you can。

 you can modify this identifier， but this is not as powerful because you're not able to just call any arbitrary function with this。

😊，Finally， there's a final thing that。By accident protects and allows you to have cos safely in a program。

 It's called heat allocationization optimization。 And this moves the coreine frames from the he to this stack。

 And as a byproduct， it actually stops using the resume and destroy pointers in the core frame。

 It's actually not documented to do this， but we found it to be doing it。 Now， in reality。

 it's extremely hard to get hello。 And if you have a very simple program， you will probably get it。

 But if you don't and you don't program your your program with cos with it。 you will probably get it。

 And basically， you need， you need the compiler to say， okay， I need to in line these functions。

 You need everything in the same translation unit， you need。😊，You need to be able to have the whole。

 to know the whole scope of the curtain。 You need to know if it's resumed and destroyed in the same place。

 So there's really lots of assumptions you need。 And unless you program your。

 your program with with hall in mind， you will not get it。 And most of the programs I look at there。

 they are not getting it。😊，And also， not every compiler supports it。 So DCC does not support it。

 Kang does support it， but I actually found that it's broken。 for some reason。 I。

 I think it's probably a bug in the latest versions of of LVM。

 And MS SBC actually started using hello after we reported this to Microsoft in version 1713。

 but it's ringing up there。 So its， it requires you not to compile with a flag that is actually by default in Vi Studio。

😊，But it's really getting improved， so。Also， I found before coming here that in the very。

 very latest version of MS SBC， the developers are actually playing around with how the co frame works looks like。

 and they actually move the promise to the very last position of the frame。

 and they are now allocating huge frames where it looks like you might not be able to leverage a a。😊。

A hip over slow unless it's very big。 So it looks like theyre modifying the core frames is not really a perfect protection。

 but unless it's getting somewhere。So if you' are interested in this project and you want to check out some exploit。

 some bookss that we discussed， if you want to learn more things about it。

 what I discussed today is just the basics of， of our research project。

 There's so much more things that we have documented about how cors work and how you can exploit them。

 So just go to this repository。 And in here you will find all the information。😊，Thank you very much。

