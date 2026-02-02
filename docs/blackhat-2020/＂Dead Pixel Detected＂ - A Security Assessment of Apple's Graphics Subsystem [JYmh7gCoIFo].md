# ＂Dead Pixel Detected＂ - A Security Assessment of Apple's Graphics Subsystem [JYmh7gCoIFo]

Hello， the one。I'm Wei Tongcheng from Microsoft in today's talk。 as the title suggested。

 I will talk about how secure or insecure Apple's graphic subsystem was。 spoiler alert。

 iss not as secure as you may expect。So before we start， I want to clarify something。

 This is This work was solely done by Yu Wang。 He's supposed to come here and present his own work。

 but unfortunately， he couldn't make it because of visa issue。 I have known him for over seven years。

 I was his intern back in 2018 after the internship。

 I went back to school and was looking for a new direction to work on。

 He told me that he just found it and reported a few bugs in the Bluetooth driver and it's highly likely that I can find more bugs there。

 And that's how I got into the field and had my first C VE and bug Monty from Apple。

 He didn't teach me like how to do reverse engineering or implementinguzz tools to find more bugs。

 he simply point out that he had just found some bugs。 in a particular module that was buggy。

 So if you don't remember anything I talk about today。

The only takeaway you should remember is that the graphic subsystem in Apple is a good target to start with。

 if are， if you are looking for zero day box there。

So let's first take a quick look at the architecture of Apple's graphics subsystem。

I know it looks crazy。 please do not read it now you can take a look a close look later on。

 The point we want to make here is that the graphic subsystem is too complex to be perfect。😊。

Let's first focus on the kernel modules， in particular， the middle part。

 which contains the kernel modules responsible for communicating with different GPUus from different vendors。

 including Intel， EM M D and Apple's on silicon GPU。

Here we listed the three vulnerabilities we found in the past from those modules。

 and I will go through each of them one by one。 They correspond to the AM MD GPU platform。

 Intel based kernel plug extension and the Apple Intel and E frame buffer kernel extension。

 respectively。The first vulnerability， I wna talk about can causes arbitrary memory。

 right Here is the string screen shot from the debugger of the crush。 We can see that the offite。

Was used to compute a kernel memory address。And can you guys see the mouse。 Okay， yep。

 so here's the instruction where this crush occurs。

 So you can see that the register R C X is a offsite used to compute a kernel memory address。

If you look at the value stored in that register， it has a magic value。

 which means it's fully controllable by the user。From the back。

 we can also see that this bug was originated from the AM MD support kernel extension。

 The root cause of the bug was that this function didn't sanitize the user input enough。Similarly。

 due to lack of checks， the this display matrix module also had an auto bound read and write bug。

 If you pay attention to the batteries， you can find that none of the related functions in this module have symbol names。

 This is not as un willing to， you know， share the details。

 but rather that Apple didn't provide kernel debu for this module。 As you may expect。

 reverse engineering， this module took significant effort for us。

Here's another type confusion bug in the Apple Intel M E client controller module。

 under normal conditions， these functions would allocate an object of size40 in hacks。 However。

 theres vulnerable branch that incorrectly treat these objects as a larger structure。

 which leads to auto bound memory access。 The screen showed here shows where the auto bound access occurs and is called stack。

All these three bugs I just talked about were you know， relatively old。 Now。

 let me share a new one we found recently in the Apple Intel graphic module。

 The root cause was that this particular function In content key didn't perform any checks on the parameters provided by user。

 which would be used for indexing of a memory address to write As we can see。

 the index is stored in the register R C X， which is set to a magic number。

 which also means that is controllable to user。😊，Another interesting bug we found and submitted in October last year will not be fixed until this fall。

 so we can not share any more details about it。 We suspect that this module is undergoing some major code refactoring to eliminate the whole。

 you know， a text surface。 That's why it takes such a long time to fix that almost like a year。

Now we have talk about Apple's kernel modules for Intel and E M D GPUs。

 We shouldn't leave Apple's GPO law， right。To start with。

 here are some good materials for previous bugs found by other security researchers。

 which we highly recommend to read if you want to learn more about those modules。😊。

Among them the first and third blockss are both classic vulnerabilities of Apple's graphic accelerator module。

 Although the second blog is a case study for the in based architecture。

 the attack surface mentioned in the article is directly related to this to a new vulnerability I will talk about shortly。

So regarding the box we found in Apple's own GPU module。

 the first 1 I would like to share relates to GPU's notification Q mechanism。

When reverse engineering the interface， we found this particular function create notification queue。

 which accepts two critical parameters from user with insufficient sanitizations。

 the number of the entries and the entry the size of the entries。

 which will lead to auto bound access。So to verify this。

 we found the bug through reverse engineering， right， to verify this is a true bug。

 we also crafted a simple P O C to trigger it。 Here's the screenshot from the debugger for that crash。

And the the patch of the bug is also straightforward。

 We just need to make sure that the input are sanitized， and they are valid。

An interesting story we would like to share is that we know for sure this bug can cause autobound right。

 which could be further exploited。 However， the description from Apple security updates simply says that this is a deny of service。

 This is because the P O C， we submitted to Apple only crush the kernel due to some invalid address on some random due to invalid access of some random address。

 However， if we carefully craft the P OC， it becomes a you know。

 more powerful autobound right primitive。You may ask why， you know。

 we care so much about this one sentence that probably no one will ever pay attention to。 Well。

 we do care about that。 Den of service is not as severe as out right。

 The security community have already， you know， reached a clear consensus that。

A local denial of service shouldn't qualify for C assignment。 So we are serious about this。

 And this is not just me。 such description lead to confusion。

 We also found that some other researchers who got confused by the descriptions from Apples updates and they have even written some articles explaining why。

 for example， no pointer reference cannot be exploited and what why apples even give CVs for those low risk box。

 The truth is， they just didn't。Care the actual root cause about and security implications of that。

 about that。So we have talked to Hypo， and they promise to update the description to make it clear。

Another vulnerabilities I we would like to share today is related to GPU's resource allocation function。

The description this time is clear。 It's a kernel memory right vulnerabilities that affects both i O S and microwavewises。

As I mentioned earlier， to make that happen to make the description as accurate as possible。

 you have to show that this bug is exploitable。 So here's the screenshot of the crush for our initial P OC。

 we didn't submit this one because we know for sure。

 Apple wouldn Apple would simply treat this as a local deny of service。 So instead。

 we have done a lot of work to refine the P OC to demonstrate that this is actually exploitable。

 here we can see that the register used to compute the kernel memory address this X8 and Xite we successfully manipulated these two register so that it can point to a valid kernel address。

 which leads to you know out of bound that can be exploited。



![](img/302f76300944753de947e20632e04cdc_1.png)

![](img/302f76300944753de947e20632e04cdc_2.png)

Another interesting thing regarding this book was that the initial patch only takes care of the code case we reported in the P OC。

 And therefore， they didn't have a sufficient check。

 We found another different input that could bypass the initial patch they have。

So this is another crush for the new P OC we croped。

 and they have to patch the syn bug again after we submit another crush， another P OC to them。Next。

 I will talk about bugs we found in another kernel component。 in particular。

 this I O mobile frame buffer。I mobile free buffer。

 if you know this is actually a high profile attack surface in the I O security community。

 this is because this can be directly accessed from web content。

 which means that this component can help attackers break defenses like a browser sandbox。

According to public records，16 kernel vulnerabilities had from this module have been reported in the past20 years of which four were were actively exploited by A T groups two were used in I S drillbri tools and one was featured in a security competition contest。

We can see from the timeline from 2011 to 2020， the number of vulnerabilities in this particular module。

 I mobile frame buffer was not that much。 Just a couple of the blocks Cs reported。However。

 there was a turning point around 2020。 A major code refactoring introduced numerous kernel box。

 This is why we witness a dramatic you know surge in the security issues within this module in the following two years。

Here， we would like to emphasize two points。 First， you can see there's approximate six month window。

Which reflects that the security community typical response time to vul in a new module。

 roughly is like half an year。 Second， we believe that you know。

 such reckless refactoring can sometimes bring catastrophic consequences to a system。Since then。

 no new bug was reported in this module。 Does that mean it's already secure enough。

One thing to note here is that actually， there were some other vulnerabilities reported in 2024。

 They were actually misclassified as I mobile free buffer vulnerabilities。 In fact。

 they are phoneware issues from the display call processor， which I will talk about shortly。

So for folks who would like to learn more about this module， please refer to these resources。

 These are really good materials。 They start with some simple and very easy targets you can start with。

Starting from the second half of 2021， a new trend has emerged。

 Many key function of implementations have have been moved from this I O module a mobile frame buffer module to one layer below。

 which is the phoneware of display call processor。 This architecture shift means that user mode can no longer directly access this attack surface。

 effectively creating some form of defense in depth。In this case。

 can we still find new bug in this module。 The answer is， of course， yes。 Otherwise。

 I wouldn't talk about it。Here's the decocompd code snippet。

 I will post for a few seconds to see if anyone in the audience can spot the issue。

 This is just the straight lines of the code with single parameter。

I also highlight something you should pay attention to。As we can see from the screenshot。

 the input parameter is a sign integer， but this was actually used as an onside integer at15。

 there was also a conditional check in the middle to ensure that the index was not too large。

 However， as you can imagine， this can be easily bypassed。 if the index is a negative value。Um。

I believe， you know， more than compilers should be able to easily detect such issues。

 but we were surprised to learn that this vulner， this vulnerability still survived。 You know。

 until recently， here's the screenshot of the crash for this bug。

And here's the code stack of the bug。 It was noting that this vulnerability can be triggered directly by user without the need to apply for any permissions or entitlement。

Another interesting thing is that we actually found there were two different implementation for the fix from two major versions of Macwise。

 The the first approach is to keep the input parameter index as a sign integer and added another negative detection check in this function。

 The second approach is completely different。 It converts this index to an uns value。

 Then they don't need to add any new checks there。😊，We were curious why， you know。

 two distinct pages exist， which seems to imply that they have different things working on the same module for different versions of Macwise。

 I don't know if anyone in the audience have insights on this。 So not。

 it would be really interesting to know why it happened。We have also identified some other issues。

Yeah， but they are all similar。 So we won't elaborate further。Now。

 let's go down one more layer to the phoneware。First。

 I would like to refer to this Asaki Linux and the M1 and1 project as background。

 The team has done a very solid job in reverse engineering on the DC CP。Architecture。

We have also listed several well known vulnerabilities in this field。

 including some CB E called Intro and called invite。

 The the analysis report on this vulnerabability is highly recommended。

To understand the DCP and its potential attacks surfaces。

 we performed reverse engineering on both the D CP architecture and its communication interfaces with the applications processor based on this analysis。

 we developed a customized fuzzing tool。And through fighting。

 we successfully triggered a large number of crushes。 For example， like this one。

 you can see from the memory， there was some information about the crush。 for example。

 in this red box。 You can see some string like A T phoneware reported an exception。😊。

Here's another crush log indicating issues with the PC phoneware in the screenshot。

 we also provide the code stack for the crush。 And you can see that the crush data is passed between DC CP and A P through this R T body mechanism。

 So D CP and and A P different processor。 So the way they communicate with each other is through is through this R body mechanism。

 And that's also how we can collected detailed bug report from the phoneware。 we directly you know。

 read memory data from the debugger to collect all these informations。

 this really helps us to better understand。The root cause of the bug。

Our father also discovered several interesting new interfaces。

 including a video interface that has received little attention in previous security research using these interfaces。

User mode application can directly communicate with DP phoneware without any permissions or entitlements。

All those crashes marked a breakthrough in our research by further analyzing these interfaces。

 We discovered additional vulnerabilities， including this one。 you can see it's pretty recently。

 which was confirmed by Apple that this one actually affects the latest versions of both I O S and microwavewise。

This is the crash log of the vulnerabilities during fing。

 And we can see that D CP transmitted a large amount of information through this antibody body mechanism。

 However， because the attack surface still contain issues。

 we cannot disclose cause the details about it at this moment。 What we can share now is that。

Many registers in the DC CP display co processor fromware can be directly controlled by user such as R2 R 8 and even this L R link register。

 This vulnerabilities and is a text surface pose significant risk。And to conclude。

 we summarize a few takeaways。Let's recall this slide。

 Our research has discovered vulnerabilities in almost every graph graphic subsistence。

 spanning like the legacy， AM M D， Intel， GPU architecture。

AG X GPU design Apples on kernel modules like this popular I O mobile frame buffer and even the phoneware。

Like the DCP 1， while significant security progress has been made。

 we believe that there is still room for improvement。

We will summarize the presentation from three perspectives， security， engineering， vulnerability。

 hunting， offensives and defenses。 First， from the perspective of security engineering。

New feature always means new attacks surface。 Reckless refactoring can sometimes cause significant。

 you know， security implications to the system。 Even， you know， in the era of AI and web coding。

 There's， you know， no guarantee， They will always produce correct code。

 That's why we should have policies or enforce people to use us either static or dynamic analysis tool。

 to make our code more secure。 And regular review of burning message is necessary。 For example， the。

Comp between sign and un sign integer， that should be easily detected by a， you know。

 regular static analyzer。 but still it was missed。 So we would recommend people to， you know。

 to regularly review all the warnings instead of ignoring them。Second。

 from the perspective of vulnerability hunting， certain complex kernel functions are repeatedly found to contain vulnerabilities severe vulnerabilities by the security community。

 We have identified numerous cases within Apples， Bluetooth， Wiifi and graphics subsistence。

In terms of the number of bugs in the graphic subsystem。

 although we only find fewer bugs compared to other modules， we analyze in the past。

 we don't believe that this is because this particular module graphic subsystem is more secure Instead we feel it's just because Apple do not provide enough thebug information to the modules related modules and they they do have some device in depth。

 like I mentioned earlier。 they moved some major components from one layer to another there so that the the attack surface is not directly exposed to the user。

 but the code is still there。 they just hide them you know， from the layer above。

 But if you know how they。Communicate between each layer。

 You can still find a way to trigger those buggy code。嗯。Like I said。

 there is a significant knowledge gaps， you know， between multiple domains。

 the system itself is already complex enough and make it worse。 Apple， you know。

 do not give enough information。 So all these modules from where they make the security research harder。

We believe that， you know， bridge the gap would really help security。 You know。

 we have a great expert from the security community， you know， constantly working on that。

 So that would the you know， make the whole system more secure。And finally。

 we would like to highlight this particular module。 I O mobile frame buffer。 As I mentioned earlier。

 this one can be directly accessed from browser and from web content。 That's why the competition。

 you know， between the offensive and defensive sides in this field is。😊，Has once reached， you know。

 a fever pitch。An analysis of the modules of。As I mentioned from the timeline。

 during the period of 2020 and 2022， there was a lag in the vulnerability research compared to。

 you know， when the code refactoring happened that reflects that， you know。

 we have a roughly half a year lag， you know， in terms of， when the bug was found。So， the。

Practice of transferring functions。 So that's the practice I all did。

 They transfer some call functions of vulnerable modules。

 such as this I mobile free buffer to the display co processor fromware。

 They move the code to one layer below this I would say its effective to some extent because they make it harder for people to you know。

 analyze and understand the code。 but this mitigation only you know， raise the bar a little bit。

 but still， if you didn't fix the root cause， people will eventually still find bugs there。嗯。

That's all for the talks。 Thanks， everyone， for joining me today。 Unfortunately。

 I wouldn't be able to have this Q And A section because I I'm afraid I don't know too much about the low level details。

 but I'm happy to talk in the break room。 and feel free to reach out to me if you wna talk anything。

 Thank you so much。😊。