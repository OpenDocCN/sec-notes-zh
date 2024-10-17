# P54：55 - Compromising the macOS Kernel through Safari by Chaining Six Vulnerabilitie - 坤坤武特 - BV1g5411K7fe

 Hello everyone， we will present about compromising the megasconer through a subparty by changing。



![](img/5e831e58f50b00d5eee362c11da3d471_1.png)

![](img/5e831e58f50b00d5eee362c11da3d471_2.png)

 six vulnerabilities。 Before jumping to the talk， let me introduce ourselves first。

 We are PhD students at Georgia Tech working with Professor Tazucaim。

 We are in SSL app at GATEK and we believe is one of the best in Promagion security labs。

 in the world。 We also place DTF as team rudimentary。

 You remember that DEP-QR-UT1 DEP-QR-UT1 is 2018。 Actually DEP-QR-UT1 is the union team of DEP-QR-UT1 which is our team。

 As you know， we won in PON2 2020 by demonstrating the power exploit。

 We got RCE and also escalated pre-release to corner after escaping from sandbox。

 We used six unique vulnerabilities in our exploit chain and 170K USD。

 Also we got second place in master of PON。 We want to emphasize two things。

 Our submission was the only browser categories of mission in PON2 on this year。

 Also it was the largest payout for single targeting this year's PON2 on。

 I think many people are interested in how we prepared PON2 on。 We prepared it for months。

 To find the vulnerabilities we considered the three methods。 Folding， CodeQR and manual analysis。

 We found several bugs by Folding but they were not exploitable。

 CodeQR looked great but we didn't have the time to run。

 So most of our findings come from manual analysis。

 We had frequent yet quick meetings to share information among members to fully utilize。

 short preparation time。 We selected the party as a target because we were interested in broader exploitation。

 and more familiar with Nix like OS。 Also previously experienced about the party exploitation helped us to select the party。

 as a target。 This is high level of what we hope our full chain exploit using six vulnerabilities。

 First we exploit a jip bug in a JavaScript engine to get RCE in web process of the party by。

 visiting our web page。 Then using our third bug， Hebrew plugin Z-brim server。

 we get our trade code excursion in， Z-brim server。 And then we execute our app using our second bug。

 a logical bug in the party support process。 However。

 the excursion is blocked by first time app excursion protection in the Meg OS。

 So we used our fourth bug to bypass this。 Now we have code excursion as an unprivileged user without sandbox。

 After that we use our Vips bug or less condition bug in Z-app preference demo to escalate。

 probability to root。 Finally， we exploit our sixth bug in Connor extension load binary to get Connor code excursion。

 and disable SIP。 Let me explain first bug in our exploit chain。

 RCE in the party via incorrect side-depacked modeling in JSCG compiler。 For your background。

 there is an in-operatory in JavaScript。 It will return to a specific property in the object or in its prototype chain。

 And this operation is usually side-depacked free， since it's just checking whether a specific。

 property exists。 The compilation eliminates redundant checks for optimization。

 Here is the function to be decompiled。 Note that all A2's type is already established。

 which means that all elements are double。 In the first line of this function。

 it says array2's value is double。 This will introduce type check to ensure that type is already established。

 And then there is an in operation against array1。 At the end of function。

 array2's type should be checked again to access array2's element。

 But the check is considered as redundant if the in-operatory's model is side-depacked， free。

 Then this check will be eliminated by optimization。 The problem is。

 if such side-depacked modeling was incorrect， we can trigger unexpected side-depacked。

 and cause type confusion。 For example， we can change array2's type to something else than access ID as an old。

 speculated type。 In-operatory's normal is side-depacked free。

 but WebKit failed to consider exceptional， cases which can cause dummy bans。

 WebKit uses PDF plugin to support an embedded PDF file。 For efficiency。

 plugin will be initialized lazily when its internal data is used。

 This includes uses of in-operatory。 And this initial ideation triggers a dummy ban。

 So we can register a handler for this dummy ban and invoke arbitrary JavaScript code to。

 cause side-depacked。 The interesting thing about this bug is that it can't be discovered by forging the JavaScript。

 engine itself。 So there is phone calls， fuzzily， code arguments。

 and the period can't find this bug since， PDF plugin is not part of JavaScript engine。

 How do you define this manually？ So how can you trigger this bug？ First， load PDF using embed tag。

 Then install an event handler that triggers side-depacked。 In-operatory。

 we consider that side-depacked free in G compilation even though it's not。

 Actually it will cause side-depacked for example printing hello word in this case。

 We can obviously debug to build a primitive for arbitrary code execution which are get。

 address open and fake object。 For address primitive， we G compile the file line function， opt。

 In the first line of the function， it says array to spell with double。

 This will introduce type check to ensure that type is array with double。 And then。

 there is in operation on array 1， this will be considered side-depacked free。 However。

 it will trigger initialization of the PDF plugin and invoke our event handler， for DOM3 modified。

 We then remove our object to the array and change the type of the array to into array。

 with contiguous。 Type check for array 2 is eliminated because of the incorrect modeling。 Therefore。

 compile code will sync array 2 as array with double。

 So we can return our objects otherwise interpreted as double in G compile function。

 Fake object primitive is almost the same。 But in this case。

 we put address of our fake object to array after triggering side-depacked。

 which changes type of the array to array with contiguous。 From address open fake object primitive。

 we just use the existing technique to achieve， arbitrary code execution。 First。

 we leak the structural ID to bypass randomized structural ID protection and corrupt。

 buried object to using one's technique which is presented in Blackhead Europe 2019。

 Then we achieved arbitrary read-write by abusing volaprized structure in GSC， similar to Eclass。

 exploit。 Finally， we overall did region 2 execute their code。 How this is fixed？

 WebKit started to consider that ino-proter has side-effects if an object's prototype is， modified。

 So now， side-depacked modeling operator is correct。 Next。

 step in our exploit chain is launching arbitrary app by exploiting our logical bug。

 in the practice broker process。 As you know， there is a fire scheme in the browser。

 If we use it in the Chrome， it will just show the contents of the directory in the browser。

 But in the exploit， somehow it pops Finder to show the directory。 We had a question。

 How does it happen？ So we checked what is happening under the hood。

 The party uses the direct function to launch Finder。

 There is a code to enter fire scheme in the party。

 It gets a fire pass from the given URL and launches Finder using Z-， Fire。 In this function。

 a URL will be checked using its FirePacket as pass function。

 Its FirePacket pass function takes the URL points to an app。 So in this case。

 the Elect Fire function is called with URL and nothing interesting happens。

 But its FirePacket pass returns first， then it will call Elect Fire with argument in Fire。

 if you are looking at pass。 After our kick experiment。

 we discovered that a pass is considered as an app by its FirePacket， pass。

 It is the directory whose name ends with that app and we can also bypass this check using。

 simple link。 Also， if select Fire's second argument points on an app。

 it will launch the app even if it， is the simple link。 And most importantly。

 the render can make broker to call this function using the party， IPC。 In short。

 we send the IPC after making a simple link for an arbitrary app。 We can launch the app。

 But two problems still exist to launch the arbitrary app。 First。

 we can't create simple link from the renderer since it is restricted by sandbox。 To resolve this。

 we use the bug3， arbitrary code execution in GBM server。 Second。

 I press mitigation code first time app protection。 In particular。

 if an app is not signed by trusted developers， its execution will be blocked。

 until users are proper。 But we were able to bypass this protection using the bug4。

 So how did that pre-pick this？ They just removed the application launching pass。 Now。

 YUNI will continue to let stop the presentation。 I'll continue with the arbitrary code execution vulnerability in GBM service by using Hebrew。

 overflow bug。 CBM server is an XPC service accessible from web process。

 These services use it to support openGL rendering。

 And we can see it is exposed in the sandbox for file code。 It is also sandboxed。

 but it has more capabilities than web process。 For example。

 we can create a simple link and send a signal to other process from here。

 And there is heap overflow bug in CBM server。 As you can see here。

 if we send a message with message field set4， then it calls the。

 function named CBM's server service。 All of these arguments are controllable from our XPC request。

 As indicated here， let's focus on framework name。 In this function。

 it opens a file name in the form of framework name， type of CPU， user， ID。

 and that map is concatenated。 Since framework name is controllable and there is no filter。

 we can make it open a file in， any directory。 In our case。

 we can create a file in the sandbox directory and make the CBM server open， this file。

 Since CBM's server reads the map's file by calculating its size based on its data， let's。

 look at the detail of the process。 Here is a shield code for the binary code above。

 Counter and offset are read from the file， so it is controllable。

 Then size is computed and it'll resize the buffer。

 Then it'll also read the remaining part of the file。 As you can see here。

 size can be smaller than 80， like both counter and offset can be 0。 Then size will be 0 too。

 But the actual size to read underflows in this case， making it possible to overflow， the buffer。

 Note that every other stuff at the end of the file， so we can also control the size to， overwrite。

 To exploit this， we used another handler from SS7， which returns from Macport to the client。

 On Macport is an IPC mechanism in macOS。 And there is a kind of Macport named Taskport。

 which can be used to control a task。 Well Taskport should not be exposed to other processes。

 because it allows reading and writing， memory and controlling the registers。

 So it basically means that arbitrary colleagues' kitchen is possible。 Also I like it here。

 Let's focus on the VM port variable that is loaded and copied to client。 As you can see here。

 the VM port is loaded from an array located at heap， so we can override， the port value。

 So we first locate the valid port value using other handler。

 Then we overwrite the port into the task port and send a message 7。

 Then the client will receive the task port of CBM Server， and in this case， the client。

 is web process。 Then we can execute arbitrary code in CBM Server by allocating memory and modifying stress。

 registers。 I will patch it this by checking if the real pass of the Mac file matches to the given pass。

 So arbitrary file open using pass travel so it becomes impossible。 Also。

 they edit or check for side on the flow。 Then we use another block to bypass the first time app protection。

 Here is a reminder that there is a first time app protection which requires user interaction。

 to open an application launching for the first time。

 It wastes the user's confirmation to click open。 The question is， how is it implemented？

 Then let's see the process list。 It turns out that the protection starts the application in the suspended state。

 What if it receives a signal which removes the process？

 It will bypass it so we could open our application bundle。 Applesate day won't fix the block。

 Here are some guess about the reasons。 First， to send a signal to an application we usually need to get code execution first。

 Second， kernel modification might be needed to support secure UI and it is not trivial， to change。

 So if you have similar types of vulnerabilities you can bypass the first time app protection。

 with this method。 Let me briefly summarize our sandbox escape chain。

 First we achieved code execution in web process using the first block and then we got code。

 execution in CBM server as well using Box3。 In that process we created a simple thing pointing to an arbitrary application and send。

 the iPC call to Safari to launch the application。 Then we sent 6 contuit to bypass the first time app protection。

 Now we get code execution outside sandbox。 To get root privilege we used the race condition block in CF-PREF-STEMON。

 So what is CF-PREF-STEMON？ It is an XPC service located at Core Foundation framework。

 We can read or write preference files by sending a request。

 There were severe security issues including the famous block found by code colorist。

 And there is a wrapper function which sends an XPC request to the demo。

 If a client calls this function with key value pair and destination pass set then demo。

 will process the request。 First， it will check if the client process can write to the file。

 Then it will try to create the directory recursively if it does not exist。 In this case it is pass2。

 Finally， it will write a new content to a 。plist file with the new key and value pair， added。

 But how exactly it creates the directory？ First， it will split the pass into parts using /s/dellimeter。

 Then for each part it will create a directory using an KDIR。

 Then it will change the permission and the owner of the folder。

 All of these three functions accepts the pass as their arguments。

 But what if the created directory are replaced between MKDIR and GH mode？

 Like we can replace the folder with a symbolic link instead。

 Then it will change the permission and owner of the pointed file instead。

 In this way we can make any files to be writeable。

 At this point let's look at the comment with ZUITP's named user bin login。

 The login comment is a user based on policy specified at the file named etcpam。d and login。

 This file specifies pam model for authentication。 For example， a module named pam permit。

so always permit access without any authentication。

 By overwriting this file we can change all pam models into pam permit。so。

 After that just running login root comment will give us a root privileged shell without asking。

 any password。 For the patch now they use file descriptor instead to change the owner of the created。

 folder。 And this is the last block in user bin our chain。

 We use it to execute code in kernel and it is a raised condition in kext。

 Before diving into the block let me briefly talk about system integrated protection。

 In mega west having a root privilege does not mean we can execute code in kernel。

 Because even the root user cannot write two folders with the attribute com。apple。rootless。

 and there is other protections that prevents them operation。

 Only specially entitled binaries can write to these folders。 For example。

 there is a comment named kext。lot and br2 legacy which is the mega west installer。

 And there are other programs with special entitlements。

 To have this entitlement it needs to be signed by apple。 This mitigation is added from OS X 10。

11 and it is called rootless。 Then let me briefly introduce about the kernel extension in mega west。

 Mega west uses many kernel models。 For example there is psd， sandbox and quarantine。kext。

 And it contains binaries and configuration files。 All folders related are protected in syp。

 So the root user cannot directly write to the kernel modules。

 Also we can only load signed kernel extensions using kext。lot command。

 Then let's look at this command。 kext。lot command has a special entitlement to write a directory that is protected by。

 IP。 So it can write to kext directories。 This command loads the kernel extension after verifying code signature and the signature。

 check happens only in user space。 The problem is both the signature checking function and the function that loads the kext。

 accepts the path of the folder。 Therefore， a raised condition could possibly happen。

 To prevent this it uses a mitigation called staging。

 Like it will use read all the copy in syp protected folder instead for verifying and loading kext。

 First it will copy kext to syp protected directory。

 Then it will verify and load the copy instead of using the original one。

 On attack I cannot modify kext file between verifying and loading because of syp。

 So it means that we fail to exploit the raised condition。

 But there are two problems in staging mechanism。 The actual steps are first it copies our kext file into syp protected folder and name the。

 library's tasty extensions。 Note that the path is preserved except the base file name which is replaced into a temporary。

 name with uuid。 Then it verifies the code signature。 If it fails it deletes them。

 If it succeeds it moves them into library's tasty extensions tmpa。kext。

 Then it'll load the kernel extension。 The first problem is it copies all of the files including the symbol link files。

 Second problem is we can actually avoid kext load from deleting them by killing the process。

 before step 3。 It is possible because both of the attacker process and kext load process are running in。

 the same account which is root。 So our plan is we place the symbol link under the kext bundle then run the command。

 Let first copy them like this and re-kill the process before it deletes them。

 Now we have syp protected symbol link file。 Then we run the commando game。

 In this time our kext under the symbol link。 It'll copy b。

kext under the folder named sym link to the destination pass which is under， the symbol link file。

 By doing this we can get a staged kext folder inside the right foot folder and it is no。

 longer protected by sip。 Then we use another technique to improve the chance to win the race。

 We abuse the sandbox feature to intercept some activity of process。 First。

 we can prevent deleting staged files by terminating kext load。

 Like we can do that by denying only the commando。 Second。

 we can stop after every file with to replace files after code sign check。

 We can send stop signal after reading specific file by using this rule。

 This technique is inspired by code colorist as a HITB presentation in 2019。

 So by attacking race condition we could load on signed kernel extension。

 For example we could use on root list。kext from Linux Sans which would disable syp by modifying。

 the kernel memory。 And the patch was， they made another read-only copy in a syp protected folder in different。

 form。 In this time they replaced leftmost pass to random one to prevent this attack。

 And here is our demo video。 We first check the version of Mega West and Safari。

 Then we navigate to the attacker server。 It first gets code execution and re-escape sandbox by attacking Safari and CVM。

 Then the calculator is parked outside sandbox。 In the background it tries to escalate its privilege to root then finally kernel。

 And it shows a terminal showing that syp became disabled by running code in kernel。

 So our conclusion is we discussed 6 vulnerabilities and their exploitation using the 0。12020 to。

 compromise Safari with escalation of kernel privilege。

 They show difficulties in protecting a large and complicated system。

 We open source our exploit chain to foster further research。



![](img/5e831e58f50b00d5eee362c11da3d471_4.png)

 Thank you for listening。 >> Okay， seems like we don't have any questions yet。



![](img/5e831e58f50b00d5eee362c11da3d471_6.png)

 (chuckles)， (silence)， (silence)， (silence)， (silence)， (silence)， (silence)， (silence)， >> Okay。

 Okay， we got one question。 >> Can you drop your link in here？ Okay。 I'll paste the link to the chat。

 >> Got another question from chat。 >> This is what was the model for this research？ Actually。

 participating is one of the dream for a hacker。 So I think that's the motivation。

 Another question is I've looked at the GitHub link provided and it seems the reference not， again。

 I'm talking with teammates to publish the exploit chain。

 So I think it will be public after about some minutes later， I guess。

 I think it will be public today。 Yes。 Okay。 Do you think that move by Apple to AM 64 CPU will make exploitation of this kind of chain。

 or challenging？ Yes， I think so。 Because we're needed to make some pointer differences work when exploiting CVM server。

 So if some mitigations like pointer or centricate code is applied， then I think the exploitation。

 will be harder。 And another question is what prompted you to look at the CVM server？ Actually。

 we looked at many debuts and CVM server is one of them。

 And we found the vulnerability in CVM server and we promptly exploit it and it will success。

 So at first we try to look at XPC and MIG demos because there were some existing research。

 around that。 So yeah。 I think this can be answered。 Okay。 I missed a question。

 So can you talk about how you go through manual finding about like what you do to get， started？ Yes。

 So first we listed all of the open the demos that is exposed to vendor process。

 And then we actually made a further to nightly just send some requests to that。

 But it does not work well。 So I try to examine each demo to figure out how a further is running correctly and it。

 related to me to a robust engineer each demo in more detail。 Okay。 We got another question。

 I have to see this box and I found to date work。 Actually we couldn't because we don't have a debugging environment for iPhone yet。

 But I have so some advisory on iOS acknowledging our book。

 So I think something would work but I think the exploit chain would be completely different。

 I guess。 And thank you for the appreciation。 I'm glad that you enjoyed our talk。 [ Silence ]。

 [ Silence ]， [ Silence ]， [ Silence ]， And for the link。

 I'm actually finding the location of our site。 So。

 I think the web pool will be also public in a few minutes。 So。

 I think we can put the link after making it public just for a moment。 Thank you。 [ Silence ]。

 [ Silence ]， [ Silence ]， [ Silence ]， Okay， I found a link。 So， I'll post it into the chat。

 It's not that this is not made in public。 We're still cleaning up some。 [ Silence ]， Okay。

 the question is， what resources do you recommend for research？

 It's getting started with Megawess on。 Actually， for all areas of research， I'd recommend。

 that we're doing a lot of research。 So， I think we're doing a lot of research。

 We're doing a lot of research。 We're doing a lot of research。 We're doing a lot of research。

 And there's many other resources。 So， I think you can read that。 [ Silence ]。

