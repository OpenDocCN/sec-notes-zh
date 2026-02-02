# I'm in Your Logs Now, Deceiving Your Analysts and Blinding Your EDR [G3Ft0gtmm4I]

Good morning， everyone。 Thanks for coming out to so many people。😊。

One thing I probably won't have time for questions right here。 If I do， you're welcome to come up。

 Otherwise， I'm in the wrap up room。And with that， I'm Olaf。 I was on holiday two weeks ago。

 and I drove past this little town in Slovenia。 and I had to get out of my car and take a picture。

 So my kids ran after me， took a picture。 because I was in the log。 right。

 you'll get the joke maybe later。 For the people that don't know me， I work at Falcon force。

 It's a company where we do a lot of detection engineering and reing And together。

 we can actually understand the attacks really well。 So we can cover those in detections。😊。

I used to be a documentary photographer。 I still like to take a lot of pictures。

 So you might see a couple in the slides。 And otherwise， if you're interested。

 you can find me online with that， a couple of things I want to talk about today。 So，  first of all。

 I'll do a brief intro infilration for Windows， E T W。 I want to do a full deep dive。

 I only will talk about the stuff that is relevant for the rest of the talk。😊。



![](img/01917883eb0840567b40224dadd12ee9_1.png)

Then I'll also cover a couple of slides on why it is relevant for security products to actually work with E T W。

 because there is some benefits to it。Then I was kind of interested if I could spoof events what would happen。

 And if I could actually further abuse that。 And， of course， otherwise I would be here。

 So there would be some， some things。 And I'll wrap up with some things you can either do about it or do with it。

 if you're more on the offensive side。 little spoiler， probably the offensive side wins it this time。

😊。

![](img/01917883eb0840567b40224dadd12ee9_3.png)

So， first of all， a couple of slides about what what is E T W。

 It's essentially a mechanism for Windows operators machines to raise。Telemetry。

And it's primarily designed for， use for user mode or kernel mode。

 performance monitoring and debugging。 You can quickly see here。 there's no wording about security。

 This is from the Microsoft side。And you'll probably figure out later， why。

So E T W is a Windows thing， and it's basically built from a kernel perspective to be very fast。

 very reliable between quotes。 And first， all sets of features that you can have trace， so。

High level wise， the architecture has three main components。 So we have event providers。

 We have tray sessions or logger sessions， as they， as they call them themselves。



![](img/01917883eb0840567b40224dadd12ee9_5.png)

And we have consumers， right， So we generate something。 It gets orchestrated and it can consume it。

 That's sort of the high level view of it。And that works in two sides。

 So we have user mode providers， and there are kernel mode providers。And then in the middle。

 there is the E T W O kernel， which also leaves a kernel space， which will be the session controller。

 And that orchestrates the flow of messages how they are being transferred。

And how that works is if I want to listen to one of those consumer or one of those providers。

 whether that's in kernel space or in user space。I need to start a trace。

 So one of the API calls you can do is you start a trace。 You talk to the E T W kernel。



![](img/01917883eb0840567b40224dadd12ee9_7.png)

And it can instantiate a tray session。There's essentially four types of trace sessions that exist。

 So you have an autologger session， which。Starts at boot。 So it registered in the registry。

 and it will start at boot， and it will start collecting events。 and they have to be written to disk。

Then there's realtime sessions。 So essentially， that's in the name， right。

 It's a real time stream of telemetry that gets buffered。

 There will be a temporary file written to disk in a Windows directory with， with the events in it。

But you will be consuming it as， as it comes in and flows。There's a file logging session。

 which is essentially what it does， right， It writes everything that's generated to a file。

 And then lastly， which I won't cover in this session， are private sessions。

 They're primarily built for in app debugging。 So they won't be consumable outside of the of the programs that you're working with。



![](img/01917883eb0840567b40224dadd12ee9_9.png)

And whenever you do that start trace session， the E T W will create one of one of those sessions。

 I'll use the realtime session in this example， mostly because this is also what EDRs views， right。

 They want to have a realtime feed of all that telemetry。And essentially。

 what happens when you do that race session and the session is created。

 The E T W O kernel will tell the providers that you subscribe to to start admitting to that session。

Each session will have a stack of trays or a buffer pool。

 and that buffer will be used for basically catching the events until the consumer will consume it。

What is relevant you'll see later in the talk。So， of course， I'm not the first one to look at E T W。

 There's a lot of people did。 a lot of great stuff about it。

 One of the things that you see quite commonly is they patch the N T D L L E T W right function somewhere in the program to stop it from emitting events。

 And this is often used for M C bypasses， but definitely not exclusive。😊。

Another thing you see quite often is they tamper with the E TL files that are written to disk。

 Or if you have some higher privilege， you can also disable some of these sessions within the registry because every time you create a user mode session。

 it will be registry key， that will be written for it as well。What you also see quite often is they。

 they， they hook certain functions within an application。

 and they will start blocking certain events similar to the E TW right function that gets patched where certain events can't be emitted anymore。

Which， of course， needs， you need to inject into all these processes， right， So it。

 it can be tempered can be noticed quite easily by E DR。 And lastly。

 if you have kernel level access by some malicious driver that you're loading。

 you can also disable full tray sessions if you wan to do that。 Of course。

 that requires quite some privilege。So for me， that wasn't really interesting enough。



![](img/01917883eb0840567b40224dadd12ee9_11.png)

So a little bit about why， why would security products use E T W， right？ First of all。

 it's quite interesting for them in terms of stability。 you need less kernel code to do this。

 which we've all seen last year that it can bring some implications， right？

 I don't want to fault any vendor for this， but kernel drivers are notoriously difficult and of course。

's tricky if you make a mistake。 So a lot of vendors choose to opt for more E TW based information because it's。

 it's already， it， it runs in its own subcornal。 So it's way more safe to do that。

Another really good benefit is filtering。 So E T W tray sessions。

 you can actually define what you want to have。 So which event I you want with which sub attributes whereas if you rely on kernel callbacks。

 you would get like the full stream of telemetry， right。

 So you have to filter that after it's being sent to your process。 So you have to do it in process。

 which takes more performance。 So that's also one of the other benefits is E T W S are also buffered。

 So there is also some leeway where you drop is less likely to drop events。

 if your process is too busy working with all these kernel callbacks you might drop stuff。

Then additionally， there is way more flexibility。 So you can actually disable or enable new providers in your trace session to add augmented telemetry。

 whereas with a kernel driver， you probably need to reload a whole driver or maybe reboot the system if you want to expand your telemetry collection dynamically。

 So that an additional benefit。 And， of course， kernel Kobacks are quite limited in the amount of options that you have。

 right， you can't collect every type of information with a kernel Koback quite frequently。

 you have to actually get it somewhere else。😊。

![](img/01917883eb0840567b40224dadd12ee9_13.png)

And apart from that， it's， it's very low in， in terms of intrusion， right there's no nothing。

 You have to don't have to hook any function。 You don't have to inject into any process。 So， again。

 that's maybe also even a stability option where you have。 So you don't need to touch processes。

So this talk will mostly have examples based on defend for endpoints。

That's primarily because I'm the most familiar with it。

 But they be assured that most of the other EDR vendors will have similar issues or sometimes even the same。

 Keep that in mind。 The only thing this slides needs to tell you is on the left。

 you will see all the kernel Kobes that defend for Mpoint users on the right are all the individual E T W providers that they listen to just to sort of show where。

 where the breadth of the information comes from。The same applies in some part to crowd strike。

 The only big difference here is I'm not exactly sure where they get all of their information because I never got access to it to actually reverse its configuration and figure out how much detail they actually get from it。

 But this is usually the same， the same mechanism because they actually need to get that information from there。

 anyway， there's no way they can do it for to Chical back。

 And I know that they're not hooking every function in every program。 so。As an example， there。

 this is a couple of the E T W providers that the F Fpoint relies on。



![](img/01917883eb0840567b40224dadd12ee9_15.png)

And one of them over here， is as an example， if we zoom into it from the configuration perspective。

 there's a couple of relevant things。 So at the top， we can see to provide a goodI。

 which is just the U I D that is registered on the window side。

 is coupled with a name and some keywords。Then lower down， it becomes more interesting。

 So what a lot of clouds based EDRs do is they have to cap the amount of events they can actually ship towards the cloud。

 right， That's a performance reason because if you have a very noisy machine。

 you're ingesting like gigabytes of of data， you need to process that。 you need to store that。 So。

 of course， also a cost thing。 and like the bandwidth that you're also consuming。

 So if you're like flooding somebody's Internet pipe by sending all the telemetry at some point that might be annoying for the user。

 So there's multiple reasons why they're capping these events。😊。

And what you can see in this configuration is there's， there's two different ways of capping。

 So there's a global cap of1 thousands。And then there's an expiration period of 24 hours。

And this is very common for at least defend for endpoint。 So you can only have 1000 unique events。

For this machine where it's running within 24 hours。 And then below that， you have the local cap。

 And the local cap is even more interesting because here you can see that it's called Ldap first scene in this example。

 And basically what it happening here is every first iteration of a search filter with a distinguished name issued by a process。

 its original file name and the signing level。 So every process can do the same query once per 24 hours to be logged。

 If you do it 50 times with the identical information， you will get one event。

 That's what that cap means。😊，And in some cases， that's annoying， right， But in some cases。

 it's fine。 You only need to see the bad ones It， is sort of the argument behind that。

which also made me wonder is if there are multiple of those events。

 Can I actually start admitting them myself。

![](img/01917883eb0840567b40224dadd12ee9_17.png)

So that's what I did。 And the reason why I wanted to do that is a couple of things。

 The first one is more for good。 I'm a detection engineer。

 I want to be able to validate whether my detections are still working。

 And I don't always want to run bad code on a machine to actually do that。

 So one of the things I wanted to do is impersonate an attack by emitting E T W events so that the machine thinks it's real because it looks real to them。

😊，And so that was the EDR。 that was the primary goal。 But， of course。

 you can also use it for offensive purposes where you can create distractions by emitting alerts that never happened or spoing events where some analysts might start digging into something that never ever was executed in the first place。

And since most of these cloud based Es have capping， as I just showed you。

I can also maybe try to exceed that amount of defense with my process so that other processes won't be noticed anymore。

And in order to do that， we need to understand a little bit how these providers work。

 So E T W providers， as you saw， have a goit and any process on a machine provided they have the right security permissions can register a provider with the same goit。

And this includes processes or providers that already exist on a machine。Which sounds weird。

And it sort of is。 But each process registration is， is， is is specific。

 So every time you register a provider， you get a handle to that provider and you can start emitting events。

 So if you call an event register function with the same provider for a good that already exists。

 you basically get a handle to that existing goodit。 and you can both start emitting events。

And for the operating system， it looks like there's。

 there's one provider and that that gets that data out。



![](img/01917883eb0840567b40224dadd12ee9_19.png)

And in terms of providers， there's， there's essentially three major ones。 So the， the。

 the most used one currently is a manifest based provider。

 So it is introduced in Venus Vista before that， it was primarily moth based。

Which are not that common anymore。 They do still exist。 It's more from the， from the WM Y's time。

 And these manifests are X M L files that are typically stored in in the Windows system 2 system 32 directory。

There are also trace lock providers， which we were introduce a bit later in Windows 10。

 And the big difference here is there's no real hard code of manifest。

 So you can basically admit all types of events you want。

They're typically more used for in app debugging or across app debugging and not so much for。

 at least from an E DR perspective security logging。 They do use them。

 but they only use it to troubleshoot their own processes。



![](img/01917883eb0840567b40224dadd12ee9_21.png)

So these manifests look a little bit like this。 So you have a core element where the provider information is noted。

 So just a name， the go it， some other relevant information。 Then we have the event elements。

 which you could equate equate to like an event I D。So there's some。

 some high level information there， like a value， a level， which can be quite important， right。

 We can have like all kinds of levels where you w to tap into as a monitoring process。

 some keywords and a template and the template is quite relevant because that actually stores the events or the the fields and the field types that you can start monitoring for。



![](img/01917883eb0840567b40224dadd12ee9_23.png)

![](img/01917883eb0840567b40224dadd12ee9_24.png)

There's a couple of tools that I like to use， at least to to look at those my， my main favorites。

 of course， E T W explorelorer that you can also see in the screenshot。

 And the the main purpose of those tools is to basically see the whole manifest or even search for it。

 so yeah， feel free to use any of the three。 they're both great or great。 So visual what happens。

 And this is a a logical， physical logical Yeah， sorry， whatever。

 the thing you can do is you can with every process， register， a provider。 I in this example。

 I'm registering the TCP I P provider that already exists on the box。

 And it's already emitting to certain。😊。

![](img/01917883eb0840567b40224dadd12ee9_26.png)

Sessions， in this case， probably the E DR。

![](img/01917883eb0840567b40224dadd12ee9_28.png)

What then happens is since it already exists， the E T W kernel will give me a handle to that provider。



![](img/01917883eb0840567b40224dadd12ee9_30.png)

And with that handle， I can actually start imting events。

So now I'm writing events to the TC P provider， which is already connected to one of those trade sessions。

 In this case， a realtime session。 and my events will also get there and will be consumed by the consuming process。



![](img/01917883eb0840567b40224dadd12ee9_32.png)

So that's kind of interesting， right， So you can basically register one of those providers。

Provided you have the right permissions， which， in most cases， you will。

 And you can start admitting all kinds of events， and they will be treated in the same way as a real process was actually doing that。

And E T W in general， doesn't record which process has been emitting whatever the telemetry comes from。

 So what they generally emit is whatever the good it is。

 and there will be a process I D within the event。 But it's not necessarily the event that generated that event。

 So it's， if you do a real network connection。Your process will not likely be the one that we will be emitting that event。

 It will come from some other subproces within Windows。

 But the bit of the of the process that made that connection to physical connection。

Logical collection。 that will be the event。 And that if that bit is sadly not spoofable because that would be amazing。

 right， then it could spoof on behalf of any other process。 But that's handled by the E TW kernel。

 So that's sadly something you couldn't do。 Otherwise。

 I could make the EDR think it was doing bad stuff and kill itself。 that sadly didn't work。

 or I didn't get it to work， at least。😔。

![](img/01917883eb0840567b40224dadd12ee9_34.png)

On the kernel side， it works in a similar way。 The bigger downside is you need to have a signed driver to be able to achieve the same things。

 But you can still register a provider and do the same emissions。

That's the only slide I will do on the kernel side。

 I didn't have time nor the skill to actually work on that side。

 So that's something I might look into later。Then there is some security。

 E T W has a security model that originates from WMI， and they made some small additions to it。

 So most of the E T W providers have a a security descriptor stored in the registry。And in the case。

 there is no security descriptor in the registry for one of those providers。

 They will get a default set of of permission to signs。

 which are noted under that very memorable goit。

![](img/01917883eb0840567b40224dadd12ee9_36.png)

And there's a couple of things you can， you can store on on， on these S DD L。

 So it's primarily involved where it which pro which of those providers can you add to a trace session。

 Can you create a trace session for these providers and a lot of other things。



![](img/01917883eb0840567b40224dadd12ee9_38.png)

Yeah。And this is basically the default security permissions。

 I added this slide not for you to read it because there's quite a lot of information that you won't retain。

 The only interesting bit here is that everyone has very limited permissions for paramedical zooming data。

 There's no real information therefore emitting。And the other interesting bit is if you have Fi studio installed on your machine primarily for developer purposes。

 of course， But then you would automatically be added to the performance log users。

 which significantly increases your permissions。😊，So at one point。

 I was interested in figuring out for all my providers where I was interested in what kind of permissions there were。

 So I built a small tool to to enumerate these。 And here you can see the difference between when there are S else registered in the registry for certain providers。

 when in this case， we're looking at the Microsoft anti malware service provider。

 And you can see that every group has quite some permission。

 So everybody can create a trace session for the anti malware service provider。

 And then on the flip side for the Microsofts Windows Windows defender one for the A V component as well。

 there is no security permissions registered So anybody can do the basics。

 whatever the operating system allows you to do。 So in this case。

 it's only registered go is to win existing trace session。😊，So at some point。

 I got all the providers that Microsoft defendender for Endpoint relies upon。

 And I started enumerating all these providers。 So I did a very simple tool where I basically try to register each provider。

 get a handle to it and write an event and look at the output。



![](img/01917883eb0840567b40224dadd12ee9_40.png)

And that sort of looked like this。Where it looked like I had this substantial amount of permissions。

 right， there were so many that gave me at least a success。

 And there were a couple where I had failures where it was exitedised。

But then when I started looking at it further， I had quite some false positive。

 So you can actually register a lot of gos that are actually a kernel mode provider from user space。

😊，And emit to it。 But it doesn't necessarily mean that's a real thing。

 but it will still permit it for some reason。 And the same for tray sessions。

 So trace sessions are handled slightly different in the E T W kernel。

 So you can actually have a trace session and a user mode session emitting data。

 but they won't coincide。 So the data will never arrive in the real tray session that you're looking for。

 So that's say a sad thing。 So from user mode， you can't spoof any kernel mode events。

 which kind of makes sense， right， at least that is very well segregated and secured。

So at some point， I re my enumeration。 So most of the providers that we are interested in are stored in either of those registry keys。

 So I started going over with those keys with another tool that I I whip up where we can actually see a bit more information。

 So now we can see the type， whether it's a user mode or kernel mode defined provider and how it is defined。

 So most of them， as you can see， are manifest based providers。 we can see where they are stored。



![](img/01917883eb0840567b40224dadd12ee9_42.png)

![](img/01917883eb0840567b40224dadd12ee9_43.png)

![](img/01917883eb0840567b40224dadd12ee9_44.png)

And then if you don't have a full list， you can also query it based on the individual。

Gid that we have。 And as you can see， I'm not the fastest dier。 General。

 if I like to talk about it as well。 And we can see some sort of a demo there。

 and we get the basic information， which is essentially the same as we saw in the table。

 So I chose the L app client as one of the tests that I wanted to do。

 So I wrote a very basic P O C in go。 because that's the only language I really know。

 And I started building it based on the open source packages that I found available。

 So I found three of them， which are primarily focused on consuming events。



![](img/01917883eb0840567b40224dadd12ee9_46.png)

And the Microsoft 1 actually allows you to register providers and limit events。 So it， I did that。

 It worked。 I wrote a very simple book。 And on the left side。

 you can see that it ran with a certain pattern on the right side。

 I can have a small trace session where I see it floating by。



![](img/01917883eb0840567b40224dadd12ee9_48.png)

So I was super happy。 But then I started looking inly for endpoints， and I never got any telemetry。😊。

So that was a bit sad。 So after a long time of debugging。

 I started decoding the real events from an L up search versus the 1 I was emitting。

 quickly learned that little Indian is very important in Windows and also found that a lot of the packages didn't allow me control over the over the right headers。

 like keywords and and other items that are relevant for emitting a tray session based information。😔。

So I took a new approach， basically rewrote the tool。 I started doing it on S calls。

 where I just run the native API calls， which would have been more efficient in C。

 but I can't do that。 So I wrote a new po。 I emitted something a trace session。 It was still there。

 So my， my new po worked。 And then I actually got information that also showed up on the cloud side in defender for endpoint。

 So that was cool。😊。

![](img/01917883eb0840567b40224dadd12ee9_50.png)

![](img/01917883eb0840567b40224dadd12ee9_51.png)

So now I thought， okay， this is， this is fun。 What can I do with this。

 So I started looking at those cappings again， So I found that， okay， I can emit one event。

 and then there's nothing。 And we had this global cap that we'll get to later。 So I ran my tool。

 I renamed my tool to Canary at Xy， ran it again， made the same emission。😊。



![](img/01917883eb0840567b40224dadd12ee9_53.png)

And I got both events in the cloud， Right， okay， cool。 that also works。

 So it proves the theory of that first scene event。 If the process is different。

 it will be different。😊，So then I started generating just 10 garbage events。

 So I'm just randomly creating search filters here。And all of those 10 also showed up in the cloud。

 So that's still were within the working hypothesis。 So then I figured， okay。

 that global cap that I saw in the config is 100th。



![](img/01917883eb0840567b40224dadd12ee9_55.png)

So let's emit a bit over 1000 events and see what happens。And after some time。

 it needs sometimes to ingest。 I went to the cloud， and I saw 1000 items only for my machine。

So that was pretty epic。 So at least I had a way where I could reliably go over the thousands and only see 1000 events。

😊，But that was all from the same process。 So I wanted to make sure I also created the real Canary dot X E。

 where I emitted a slightly different search query。

 So everything was different just to make sure that it still operated as I expected。



![](img/01917883eb0840567b40224dadd12ee9_57.png)

But I never got logs， right So I proved my， my theory I could actually exceed the amount of events that were made。

 I would never see any data。

![](img/01917883eb0840567b40224dadd12ee9_59.png)

So the canary he still lives， he's happy。So with that。

 I could actually force the for endpoints into going into global capping mode for each event type。

 Of course， you need to do that for each provider that you care about， but it's still useful。

 Then run a real attack and never get detected。 So that actually works， works really reliably well。

 You can do whatever you like。 theR will never see it as long as the A V doesn't recognize your binary for whatever reason。

 right， That's normal。 So at some point， I also wanted to test this create a above for it。😊。



![](img/01917883eb0840567b40224dadd12ee9_61.png)

Exed it and looked at the trace session of my targets。 And I could still see it being emitted。 So it。

 that actually works。 You can also flood with the same b。

And that information still comes into the for endpoint。 So that's， that's fun。



![](img/01917883eb0840567b40224dadd12ee9_63.png)

Other things you can do with this， depending on how conditional access， for instance。

 is implemented in an organization。If a user is at a certain risk。

 you can have a a conditional access policy that blocks the user from accessing your resources。

 right， So if I'm an attacker， I can actually start generating alerts for that user to get them over that risk level。

 and he won't be able to access certain resource anymore。

 So you could imagine as a red teamer to annoy the blue team a little bit。

 and get their machines or their user heat of the network。 Of course。

 you shouldn't do that But technically， that's possible。 the same goes for devices， right。

 if you have intune based devices that have a comp check it's quite common to see policy applied where noncompliant devices are not welcome anymore。

 And one of the comp checks can be， again， a risk level。

 So if you generate too many alerts on the machine， you can actually get some machines blocked。



![](img/01917883eb0840567b40224dadd12ee9_65.png)

![](img/01917883eb0840567b40224dadd12ee9_66.png)

So another provider I was kind of interested in is the anti malware service provider。

 because that is one of the inherent providers for defend for endpoints A V and malware detection engine。

 as it says in the name。 So I wrote another thing where I emit it a bunch of random malware events。

 with real names and real detection Is， but with fake files on the， on the operating system。

 And again， only 1000 of those files were logged because that's the cap of the。😊。



![](img/01917883eb0840567b40224dadd12ee9_68.png)

![](img/01917883eb0840567b40224dadd12ee9_69.png)

The provider configured within the cofi of the F4 endpoint。

 So if I have 1000 emissions and then drop my real malware because it may be detected， it won't be。

 it won't be visible for， for the analysts anymore。

So it could also become a ransservware operator now where I can actually pretend there was all kinds of ransomware detections on this box。

 and you will get a lot of alerts within the， the the defender interface。

 which might actually distract an analyst from fully investigating that machine while you're doing something else on a very different machine。

Which is kind of painful， right， I couldn't go into full panic mode and you're doing something else。

 Again， don't do that because people have stress levels， but can be fun。

 And you could do the same with fake A V detections， mitigations on all kinds of levels。

 and you can start emitting those。 They will even show up in the normal event log。

 You could get the information there， somebody will start analyzing that while you're doing something else。

 So it could be fun。 But I also thought this kind of serious。 So I wrote a letter to Microsoft。😊。



![](img/01917883eb0840567b40224dadd12ee9_71.png)

![](img/01917883eb0840567b40224dadd12ee9_72.png)

And at some point， I did some research。 I reported it。

 I included a po tool and a list of all the E T W providers I was able to abuse。😊。

But my P 2 only coed the N malware1 because I thought that would be proof enough。

So a couple of weeks later， they were quite quick about it， but they didn't find it important。

They did think it would be something to fix at some point。

 but it wasn't a bar for immediate surfvicing。 So case closed。



![](img/01917883eb0840567b40224dadd12ee9_74.png)

Or not。 Maybe it wasn't。 So a couple of weeks ago， I wanted to prep， which I usually don't do and。

 or not that not that far ahead。 And I wanted to create some recordings for you guys to show you a。

 I can emit all kinds of ransomware events， and it will never be visible。



![](img/01917883eb0840567b40224dadd12ee9_76.png)

The events were still visible within my logging， but the alerts never came anymore。



![](img/01917883eb0840567b40224dadd12ee9_78.png)

So that was surprising， but I was kind of happy about it。 So I was like， hey， they fix it or not。😊。



![](img/01917883eb0840567b40224dadd12ee9_80.png)

So I started looking a little bit deeper and started generating all kinds of auto alerts。

 and they still worked。 So I could still like mimic a sharp pound scan or do all kinds of A D explorelorr clones and a lot of other things。

 And everything basically started alerting， except for the anti malware1。😊。



![](img/01917883eb0840567b40224dadd12ee9_82.png)

They even built some signatures for some strings within my binary。

 So I even had to go into A V invasion mode by obfuscating a lot of strings within my binary to get bypass or pass their security checks。

 And couple of days after that， I sent my deck to M RRC。

 because I promised to show what I would be talking about。

 And they told they replied to me and was like， okay， great。 werere also building a fix， by the way。

 And it will be available August 7。😊。

![](img/01917883eb0840567b40224dadd12ee9_84.png)

Which is tomorrow。 So that was kind of a weird time， time coincidence。

 And it didn't tell me a lot of other stuff。 So I sent them a couple of other emails。

 And at some point， they explained to me， well， the focus is on hardening defender product providers against nonprivileged users。



![](img/01917883eb0840567b40224dadd12ee9_86.png)

So I was kinda curious。 and yesterday evening， I figured， let's see what they did。

 So I booted up one of my windows insider machines。

 which usually gets like a couple of rings sooner in terms of patches。



![](img/01917883eb0840567b40224dadd12ee9_88.png)

And I ran my tool against the Windows defender， E T W provider。And it was still successful。

 So I tried it against the M C provider。 And again， there， no surprise。 It worked。😊。

Then I tried the anti malware1。 Remember that I in my P O C tool。

 and there it actually didn't work anymore。 So they did something。 So that's cool。 Saly。

 the other stuff didn't really work。 And I started figuring out what they did。

 So I I I still don't know to this day。 they register they use the same provider。

 The permission model looks identical to what it used to do a couple of weeks ago。

 yet I still not unable to to， to register a provider anymore。 So they。

 they fix it in such a way that I can't show it to you now。But they。

 they only fixed that single little thing， right， I reported a huge list of over 100 items。

 and they at least made a step to to start fixing it。

 But all of the providers that M the E relies on， they won't even touch。

 So one of the statements that they had is they're encouraging all the providers within their colleague base for Windows to。

 to start doing something about it。But they have to note you need go to execution on this machine。

 which usually if you can fish somebody is not that very difficult， right， So it's a fix。

 but it's not a full fix。

![](img/01917883eb0840567b40224dadd12ee9_90.png)

嗯。Later， I also started looking into buffers that you saw earlier。

 each E T W provider gets a buffer pool， and that buffer pool can be configured by the person instantiating that that tray session。

 So you can give it a certain， certain size and the amount of buffers you can configure。

 and they are all assigned non page kernel memory。

![](img/01917883eb0840567b40224dadd12ee9_92.png)

![](img/01917883eb0840567b40224dadd12ee9_93.png)

You can query that with logman。 so you can basically see what kind of settings are there。

 Not that super important。 But I figured what would happen if the buffer is full could also be interesting。

 right， So technically， how buffer works。 again， my， my perfect drawings can explain that to you。

 The buffer pool gets just flushed or filled with events that trickle down from the providers that are emitting events。



![](img/01917883eb0840567b40224dadd12ee9_95.png)

![](img/01917883eb0840567b40224dadd12ee9_96.png)

![](img/01917883eb0840567b40224dadd12ee9_97.png)

At some point， they fill the pool。 The consumer gets the information， pool gets flushed。

 and new events started tricking。 right， sort of a cash cache mechanism。

 But what is interesting when the consumer doesn't get the information yet， the pool is full。



![](img/01917883eb0840567b40224dadd12ee9_99.png)

Basically， what will happen then is the E T W kernel will tell the providers like， hey。

 I'm out a memory for this。 go away。 and it will tell it for every session that has。

 has a connection with one of those providers。 So if I fl a session with TCP events for my session。😊。

That means that the TCP provider can't emit to any other session。

 So what you would see here in the top one， the E DR has his own session。It listens to the TCP1。

 But because I flooded mine， TCP can also not emit to the EDR。



![](img/01917883eb0840567b40224dadd12ee9_101.png)

And this is kind of scary。 It is by design。 So at some point， I wanted to test this。

 I wrote a very simple tool to actually emit a bunch of events as fast as possible。

 started a real trace session for one of those test providers。 And I was kind of lazy。

 And I forgot to connect a zoo to this。😊，So this is one of the ways you can do it with Lordman。

 And basically what that looks like on the right， I created that Ldap trace on the left。

 I have my tool to actually start blasting events。 And what you see on the left is that Ldap clients。

 E T W provider with a significant amount of failures and on the right side。

 you can actually see that all the events are being lost at the bottom。😊。



![](img/01917883eb0840567b40224dadd12ee9_103.png)

![](img/01917883eb0840567b40224dadd12ee9_104.png)

So the tool you see on the right is also a tool that I will release after this talk。

 It's E T W top because theres no， I couldn't find a tool to actually monitor for these kinds of things。

So you can， you can play with that。 The big tool that I'm releasing after this talk as well is bambbuu ED R。

 It's primarily a talk to， a tool to actually do all the things that I was talking about earlier。

 iming events。

![](img/01917883eb0840567b40224dadd12ee9_106.png)

For all the providers that you want。Hit the capping for a lot of those E Ds。

Another thing you can do is simulateulator for real attack tools。 So， for instance。

 I could simulate sharp pounds with its L up queries to actually build and test from detection textures that you may have built based on custom events。

 What it could also do is start one of those trade sessions。

 But then for all the providers that defended for endpoint at least re on without adding that consumers。

 So I can actually start flooding these buffers and make it blind。So any process relying on those。

 those providers will be， will be blind to these events。So quite important， closer to wrap up。

 What can a defender do about this。 Well， we can， we can at least use this to test our custom detections。

 right， That's cool。we can try to build a detection based on log spikes， and then nothing。

hich could indicate that somebody is actually doing what I just showed you quite。

 it can be quite complex in a large environment。We can try to play a little bit with the security descriptors to see if we can block things from。

 from actually allowing this to happen。 But that can be tricky because you don' know what you're gonna break。

 But essentially， we have to hope that Microsoft at some point will realize that they need to do a little bit more about this。



![](img/01917883eb0840567b40224dadd12ee9_108.png)

So for your reference later， you'll have example detection that will actually look at all the the data events that you can gather and show you the outliers like。

 hey， there was a peak and then nothing。 you can play with that。 So look at the。

 look at the slides for that。

![](img/01917883eb0840567b40224dadd12ee9_110.png)

From the attacker perspective， it's actually more interesting。 So we can。

 we can emit enough events for a provider and make a blind for that global capping ingestion for at least 24 hours。

 right， It reset。 So we need to do it again。 But we can also register a tray session。😊。

And actually flooded。 And then it will be blind until the machine reboots because that buffer will stay full until its flushed or until the machine resets and it gets cleared again。

 So that's a lot useful， more useful。 we can also fake events。

 So we can still generate all kinds of alerts or just events within the logs analyst actually starts analyzing something that never happened and spend a lot of time investigating something instead of looking at the real one。

 And we can essentially booth machines or computers users of the network if their conditional access policies allow this And lastly。

 that I didn't explain the trade sessions also have a limit on a machine。

 So you can essentially also register hundreds of trade sessions。 And I think usually the limit 64。

 So you can't exceed that anymore。 So any provider or EDR vendorer can't also at additional ones。

 I didn't add that to this because it can be kind of dangerous。 So it is the most disturbing picture。



![](img/01917883eb0840567b40224dadd12ee9_112.png)

Had， but sorry。 So， so the essential things now are， you can't trust your logs anymore， right。

 I validated at some point with， with Pal， who's an expert and witness eternals writer。 And he。

 he sort of confirmed the same。So， it's kind of。We're in a bit of a weird mix here because we want to use our our E TW providers also to detect all kinds of malicious use。

 But as at least as a defender， we have to be mindful of where the telemetry comes from。

 how it can be tampered with and how reliable they can be。 There's nothing we can really do about it。

 but at least don't trust everything you see， like also on the Internet。 But yeah。

 since they are quite valuable。 It's also there's no real good other way to get it。 otherwise。

 we have to inject into every process。 So in the end， EDR vendors make use of this， right。

 And and rightfully so there's no other way around it。

 But the current security model on Windows at least doesn't allow it to make very trustworthy telemetry assuming that an attacker also can replicate this。



![](img/01917883eb0840567b40224dadd12ee9_114.png)

![](img/01917883eb0840567b40224dadd12ee9_115.png)

I only have 25 seconds left。 So during this research， we also figured out a way how E D R onboard。

 at least for Microsoft， we reverse engineered that it will be a future talk because I already don't have time to make this。

 This research was extremely important to get you all kinds of smart people telling me stuff。

 So things to them。

![](img/01917883eb0840567b40224dadd12ee9_117.png)

![](img/01917883eb0840567b40224dadd12ee9_118.png)

All the tools will be on Github。 The last one， the big tool I still have to upload because I made some fixtures to it。

 but it will be available tonight。And thank you。