# BitUnlocker： Leveraging Windows Recovery to Extract BitLocker Secrets [2CJl6mTtgws]

Right， hello， everyone。 thank you for joining us today and welcome to our talk， Bea and locker。

 leveragingveraging Windows recoverycovery to extract Bea lockcker secrets。😊。

Today will take you inside journey of attacking and securing beat locker。We'll show you how we found。

 exported and fixed new vulnerabilities in the Windows recovery environment that allowed bypassing beat locker and extracting all the beat locker protected data。

 But just before we dive in a quick introduction。😊。

So my name is Aone and here with me on stages Nathantanel。

 Were both security researchers working with a security testing and offensive research team at Microsoft。

 also known as Storm。😊，Our expertise in vulnerability research focus on everything that happens before the operating system fully loads。

 covering the various boot stages and its security features。

 including beat lock or secure Bo and others。😊。

![](img/6965ae58a1114310549c9e9863ca8823_1.png)

But enough about us。 And let's dive into our agenda。

So we'll start with a research background and give an overview of both beat locker and Winery。

 Then we'll dive into the vulnerabilities that we that we discovered， how we exploited them。

 And finally， we'll wrap up with our research results。

 the fixes and actionable beat beat locker countermeasure that you can apply today。😊。

So starting with a research background。Today， we'll explore a security feature that is designed to protect your sensitive data at。

 It aims to defend against the scenarios where an attacker steals your laptop。

 aim to extract your sensitive information， compromiseise your machine and even back to it。😊。

You're here at Black Hat， surrounded by some of the brightest minds in cybersecurity。

 And let's be honest， some of the juiciest targets for hackers。😊，Now， picture this。

 You step away for a quick coffee break。 And when you come back， your laptop is gone。

 St by a thief that is after your research， internal tools， credentials and personal data。

Without data trust protection， the thief can directly access the contents of the hard disk。

 extract sensitive data， or even modify system fast or install the back dooror and then return the laptop without detection。

Data trust protection ensures that even if your laptop is stolen。

 your data remains encrypted and inaccessible， and your system remains untempered。Now。

 I know what you're thinking。 The odds of your device getting stolen are so low that you're probably wondering if you should even bother worrying。

So consider this。 St show that a laptop is stolen every 53 seconds。

The odds of a laptop being stolen are one in 10， and the average cost of a stolen laptop are around $49000。

This figure reflects replacement costs， productivity losss， legal expenses。 But most importantly。

 the loss of sensitive data and intellectual property。

These numbers emphasize the criticality of data trust protection features。 And with that。

 let's dive into our journey of attacking and securing bit locker。

 which is Windows's data trust protection feature。😊。

So Biockcker is a full volume encrypt technology that is designed to protect individual disk volumes。

Once enabled， it encrypts the target disk， the target disk volume。

 protecting all data that is stored on this volume。 Now。

 users have the flexibility to choose which volumes to encrypt based on their specific needs。

 And by default， beat locker targets the O S volume。

 ensuring that all data you stored within the OS S environment is protected。😊。

With beloer encryption in place， even if your laptop is stolen。

 the protected data remains unreadable because it's encrypted。

The Blocker Web model assumes an attacker similar to a common thief。

 which is someone with full physical access to the device， but without advanced credentials。

 such as usernames or passwords。

![](img/6965ae58a1114310549c9e9863ca8823_3.png)

Natally， middlero attack surfaces are shaped by the interfaces that are accessible to such attack of feeding within this threat model。

 So as we examined， the different attack surfaces， one stood out a surface with significant vulnerability potential that surprisingly has received very little attention in per research。

😊，This surface is the Windows recovery environment， also known as Win or E。

Any bit locker attacker can directly boot into Winery by holding the shift key while selecting restart from the log screen。

 We render it a potential candidate for deeper inspection。😊。

Given winry feet within the Blocers web model， the lack of per research and its high vulnerability potential。

 we decided to conduct a secure security review that is dedicated dedicated towards finding new vulnerabilities。

 exploiting them， fixing them and then hardening winry and reducing its exposed attack surfaces。😊。

The goal of of our security review was to make both Winery and beatlocker more secure and resilient。

So let's take a closer look at Winery， what it is， how it functions and which areas present the most promising the most promising opportunities for vulnerability research。

😊，So Winery is Windows's recovery platform。 It's designed to address critical system issues such as startup failures。

 resistance crashes， beat locker errors and more。For instance， if your machine crashes。

 Winery is responsible for analyzing the issue， identifying corruptions and resolving it by using its recovery tools。

If you use Windows， the chances are that you have encountered Winery at least a few times in your lifetime。

Architecturally， Wiery operates with its own standalone OS， known as the Rey O。

The recovery West is a lean version of Windows， with recovery specific customizations。

These customizations include a unique set of recovery tools that are all integrated into the known blue screen recovery U I。

Among the exposed recovery tools are the familiar options such as startup prepare， system reset。

 system Re and other familiar options。For storage， the entire recovery O。

 including all the executables， D L L S and drivers。

 All the system files are basically compressed into a single we file named Winery Wiim。😊。

This wave file is then stored on disk。Then when weary boots。

 the entire w file is decompressed into Ram， creating a temporary Ramm disc that hosts the recovery of runtime。

😊，Any changes made within this environment are not saved back to the original winery wind file。

 making the the recovery of runtime inherently volatile。

 Moifications to the run disc are discarded upon reboot。😊，When Bilocker was first introduced。

 Winery had to evolve to support recovery from Bilocker related failures。

And this LED to several architectural and design changes to enable that beloer recovery functionality。

So we wondered what impact these changes had。 Let's take a closer look at them。

The first design change focused on the location of the winery wem file。

 It was moved from the from residing in the O S volume。

 which with beat locker is now encrypted by beat locker to reside in a dedicated recovery volume。😊。

This change was necessary because Winery must be able to recover from beat locker related failures。

 if the O S volume is encrypted and becomes inaccessible because of a decryption failure。

 for example， Storing wineery there would prevent recovery。😊。

Since Win is a critical critical component that must always be available。

 it was moved to a separate recovery volume。 And this ensures that Winry can function。

 even if the O S volume is not accessible。😊，The second design change focused on the integrity of the Winery Wiim file。

 So to support integrity validation of Win Wi， a feature called trusted Wboot was introduced。

It verifies winner's integrity by comparing its hash to a known， trusted hash。

 So a simple hash comparison of winner we win。😊，If the hash is match。

 the o volume is automatically unlocked。If Daes do not match， the O S remains lockeded。Now。

 these two statess auto unlocked， unlocked， defined the level of access that we already has to the O S volume。

In the auto lock state， Winry has full access to the West。

 It can perform recovery operations without any user intervention， whereas in the lock state。

 Winry has no access at all to the O S volume。Now， this change， the introduction of trusted windbo。

 was necessary because Winery now resides on an unprotected recovery volume。

 and it can no longer be blindly trusted。We trusted Wibo any unauthorized modificationification to win or we win。

 for example， by a physical attacker， breaks distrust， effectively blocking any tampering。

The third design change focused on limiting recovery tools that are inherently risky to beat locker。

 For example， among the recovery tools is the command prompt。

So to prevent a scenario where an attacker abuses the command prompt to access the bitlocker protected data。

 A volume re functionality was added into Winery。And this functionality is triggered every time that a risky recovery tool is selected。

 and it res the O S volume。So then to regain access to the West volume。

 the user must manually insert the bit locker recovery key。 and otherwise。

 all contents remain completely inaccessible。

![](img/6965ae58a1114310549c9e9863ca8823_5.png)

And this is how it looks like。 If an attacker launches the command prompt the command prompt recovery tool without inserting the bit locker recovery key。

 access to bit locker encrypted volume will be denied， resulting in the following lock error。



![](img/6965ae58a1114310549c9e9863ca8823_7.png)

So to summarize the impact of the design changes， as long as Win Wim is trusted and no risky recovery tools are triggered。

 the O S volume is automatically unlocked， allowing Wiery to recover it without user intervention。

In contrast， if winter wind is modified without authorization or if a risky recovery to the trigger。

 the OS volume is reed， preventing Winery from accessing it。So now， the critical question arises。

Any attack surface is exposed because of these design adjustments。As it turned out。

 during the auto state， winner reports files from unprotected volumes， specifically。

 the E5 volume and their recovery volume。This parsing presents a very interesting attack surface。

 which wasn't valuable to explore before beatlocker's introduction since before beatlocker。

 there was no security boundary to cross。Before a beatlocker。

 even full court execution within Winery didn't grant a physical attacker any new capabilities。

In contrast， with beat locker， any code execution within Winery during Da lock State can be utilized to bypass beat locker and extract all the protected secrets。

We decided to dig deeper into these external files and see and examine the pricinging。 specifically。

 the files that we will be focusing on today are the R agent X Ml and boot S D I files residing in the recovery volume and the boot configuration data store。

 the BD store residing in the E5 volume。😊，So without further ado， I'm gonna pass it on to Nall。

 We will walk us through the research process of the arts external files。Thanks a along。

Let's begin with the Buddhist Di。Now， this SDI stands for system deployment， image format。

 and it is optional used for booting from virtual disk into Rams。

This format consists of multiple binary blobs， with certain lobs being essential for the Ramdi boot process。

Below， you can see the layout of of the Ramdi skin memory， which contained the O disk skin image。

When S DI file is specified， it gets propented to the virtual disk within the allocated Ramm disk memory region。

In our scenario， we are trying to boot to Windows recovery from a windf。

The Buddhist D I will contain a windimblob， which contains an offset to the apped trusted Winery wim。

Additionally， the Buddhist D must contain an empty T N TF face volume。

This empty volume enables the in memory wem to be presented as NF S volume。

 ensuring compatibility with other Windows components。😊，Now。

 let's briefly examine a pseudo code that demonstrate this process。

The code retrieves the sizes both of the SI and wind files。

 then allocates buffer large enough of both。Next， the DI data is copied to the beginning of the Ramdi image buffer。

Then the Wiim data is loaded and positioned immediately after the SI data。During this process。

 the loading API computes the Wi hsh for validation。 If the hash validation fails。

 the min volume gets locked。And finally， the Wiim address is determined by adding the Wim offset stored in the SDI to the Ram diskk buffer address。

😊，Notably， there is no verification linking between the wind being used to the one that was harshed earlier。

 allowing the offset to be said arbitrarily。With that in mind。

 let's dive into our first vulnerability。We know that weery Wi must pass harsh validation when loaded immediately after the SI。

 However， the Buddist D I itself undergoes no validation whatsoever since we can manipulate the Wim offset to point to an arbitrary location。

We could append an untrusted whim to the SI and adjust the offset to point to it。

The validation for for the trusted weim will succeed。

 but the untrusted wingim will be executed in its place。

This allows us to load and execute any wing we choose while keeping the menu volume still unlocked。



![](img/6965ae58a1114310549c9e9863ca8823_9.png)

Now， let's see a demo for the first vulnerability。Here's a lock machine。

 which we don't have the password for it。So now we press on the shift restart to reboot into Winery。

And inside Win， we will run the exp we prepared beforehand。 Now， during boot。

 the main noise volume gets automatically unlocked by beat locker。



![](img/6965ae58a1114310549c9e9863ca8823_11.png)

We reach win U actions。 We select troubleshoot。Then， we select advanced options。Command prompt。

 Now the main S volume got lockedgged。 It requires a key。 We don't have it。 So we ski this drive。



![](img/6965ae58a1114310549c9e9863ca8823_13.png)

Now， we try to access C drive， but we can't access it。 It says log by bit locker。

If we check the B locker status， we're gonna see it as locked。Now， we change path to the D drive。

 which is the mounted the recovery volume， the unprotected volume。

 We execute the expert we prepared beforehand。Now， we added， weend the custom Wim to the Buddh D I。

 We are just the offset to point to it。 And at this point。

 we have only to re the machine back to Winery。

![](img/6965ae58a1114310549c9e9863ca8823_15.png)

Now， instead of going to win a re U I actions， we expect the， the custom boot to load。

 which we set CMD to launch。 Here is the CMD。 we can change path to C drive。



![](img/6965ae58a1114310549c9e9863ca8823_17.png)

If we check the B locker status now。You can see that it says unlocked and above it。

 the protection is still on。And if we list all the files under the C drive。

 we can view all the secrets。 There it is。

![](img/6965ae58a1114310549c9e9863ca8823_19.png)

![](img/6965ae58a1114310549c9e9863ca8823_20.png)

After we completed our review for Budd S DI， we shifted our attention to our agent and X M L to search for vulnerabilities。

The R of TM L file contains the current recovery state and other configuration settings for wineery。

Upon when is start up， it immediately consumes and passes this X M， L file。

When Win East state is configured to execute a scheduled operation。

 it will run the designated recovery operations， such as startup prepare or other tasks。

And all of these occur prior to reaching the UI actions you've seen before。Today。

 we will talk about two scheduled recovery operations that can get up that can get arbitrary executed by modifying the state in our agent X M。

 L。 First， offline scanning and then we applications。Starting with offline scanning operation。

Disopation allows running A V scan from within Min against the main volume。

It is mainly used against power that does not run inside greenery。

We farm we can control which Iper the scanning， though there are couple limitations。First。

 often scanning is performed only by executing applications from within the main noise volume。

 Second， this application must be digitally designed by Microsoft or W HQL。And third。

 the signatures must be embedded within the binary itself。

We search for all default applications that satisfy these requirements。

 and we discover approximately 30 applications。 No， CD is not among them。

And we could not execute all applications， on that least， mainly due to compatibility reasons。



![](img/6965ae58a1114310549c9e9863ca8823_22.png)

We reviewed them one by one until we got the application named T， T tracer。

This application is a time travel debugger utility that allows tracing an arbitrary executable。 Okay。

 so if we can trace anything we want， let's trace CM D， then right。Now。

 let's go over the full expposition steps of the second vulnerability。Before starting the machine。

 we set recovery state in our region and X M L to schedule the offline scanning operation。

The X M L is then far， par by Winery。Then according to the state， we just said。

 Winner re performforms the scheduled operation， which is offline screening。

It will execute each deor， which will trace any binary you set。We specified CMD。 So in our case。

 it will launch enter A CD。And at this specific point， while still in O to unlock state。

 we have a shell access with the menu volume fully unencrypted。



![](img/6965ae58a1114310549c9e9863ca8823_24.png)

Let's see a quick demo for the second vulnerability。So again， is a lock machine。

 with a password protector， which you don't have the password。

 We shift restart again to boot into Winite to execute or exploit。

Now the main US volume got automatically unlocked。 We to win a review action。 select troubleshoot。

 then advanced options and command prompts。

![](img/6965ae58a1114310549c9e9863ca8823_26.png)

![](img/6965ae58a1114310549c9e9863ca8823_27.png)

Now， again， the Man S got locked。 We don't have the key。 So we ski this drive。If we try to access C。

 you can say it says the this drive is logged by B locker。If we check the billlogger status。

 you can see its as locked。Now， we will change path to D drive， which is the recovery volume。

 the unprotected volume。 We will run the expert we prepared beforehand。

 We set T T tracer to get scheduled in the offline scanning operation。

 And we also set it to to run the CMD to trace and run CMD。 Now with the export completed。 we。

 we would the machine back to Winery。😊。

![](img/6965ae58a1114310549c9e9863ca8823_29.png)

Only this time， instead of seeing the UI actions you've seen before， we will。

 we expect to see T T tracer。

![](img/6965ae58a1114310549c9e9863ca8823_31.png)

The application we scheduled。Here is T T tracer。 It ask， It asked us to accept the E O A。

 We accept it。And immediately after that， we can see the CMD is spawning。 It launched。 Now。

 we can change the drive to see if we check the billlogger status。

Here it says unlocked and protection is still on。And， again。

 if we list all the files under the C drive， we can give， we can view the secrets。 Here they are。



![](img/6965ae58a1114310549c9e9863ca8823_33.png)

![](img/6965ae58a1114310549c9e9863ca8823_34.png)

So we completed going over the offline scanning。 So let's proceed to win our applications。

Another scheduled operation we can configure is winery apps。

 which allow us to run applications within Winery。If this application are trusted。

 they get executed while the main remains in auto un lockstead。Otherwise。

 they run after the manise volume has been reloced。We went looking for the harsh validation process。

 and we found a trusted application are registered in the Winery registry using their executable name and the file hash。

 This registry resides within the Wiim file， and the physical attacker cannot modify it。

 Any alterations to the Wiim would change its harshush。

 causing the main noise volume to be relocked during boot harsh validation。

And when a winnerery is scheduled to run trusted application。

 it first calculate the has of the target application and then searches for a match in the registry。

 If a match is found， application is marked as trusted。 If no match exists。

 the application is marked as untrusted。 The Min volume gets relocked。 And later。

 that application gets executed。

![](img/6965ae58a1114310549c9e9863ca8823_36.png)

Overall， that sounds as binaryary trusted applications Val is solid。 It workss properly right。 Well。

 we wondered if an already registered application can expose an attack surface。

 So we went hunting for registered applications。😊。

![](img/6965ae58a1114310549c9e9863ca8823_38.png)

We found a legitimate usage of this feature by a program called Se Plaform。

This application is registered as trusted up and and used during the Windows upgrade。

Only once the upgrade completes， the trusted app entry remains in the registry。

 and it is not removed。So we can execute set a platform。 But what else can we do with it。

We found that the register shift was F1 hotkey to launch C D。We tried that。

 but it fails to execute due to missing configuration on the main S volume。

 Since this configuration file resides on the bit locker protected Min S volume， we cannot。

 It cannot be created or edited。This configuration requirement cause set a platform to terminate early when the file is absent。

Now， the time window between the hotkey registration and the process exit is very short。

 It's quite impossible to trigger CM D using this hotkey manually， so。

We still wanted to find another way to figure CM D。 So we went back to analyze a platform。Now。

 this is when we discovered something really interesting。Immediately。

 after registering the CMD hotki， this application。

 it search for a set platform and I file on the recovery volume volume。

 which is the unprotected volume。Now， by configuring this file properly。

 set platform will trigger a message box。Which prevents application from continuing the execution。

 essentially， blocking the application for， from early determination。

That will open an infinite time window to spawn CM D and type anything we want in it。

Let's view the complete flow。 So we modify our agent X M L to set to set for scheduleched for scheduling enrus it up。

 more specifically， we said set a platform to get executed。

Whenner even validate that this app can be trusted， the validation succeed。

 and the minus volume remains unlocked。Next， how a platform gets executed。

It registered the hotkey for CM D， which is shift plus of1。

It then looked for setup half of my an iPhone， and loads it。

Which will trigger a message box This way， the process gets blocked and won't continue until we press。

 O。Now， we can simply press on ship us F 10 to trigger CMD。And we get a shell open and running。

 while the mens volume still unlocked。

![](img/6965ae58a1114310549c9e9863ca8823_40.png)

Let's see a demo for the third vulnerability。So again， its a lock machine。

 which you don't have the password for it。 We shift restart to reboott into Winery。



![](img/6965ae58a1114310549c9e9863ca8823_42.png)

We reached Win E in the， we select troubleshoot and then advanced options， command prompt。Now， again。

 the manuals got locked。 We don't have the key。 So we ski this drive。



![](img/6965ae58a1114310549c9e9863ca8823_44.png)

We tried to access C drive again。 It has locked。 We'll check the bit longer status。Again。

 it says locked on both。Now， well run the exp the exploit resides on the un the recovery volume。

 the unpro volume。 we set set a platform to as a trusted app。

 We also set the set platform I N I file and。

![](img/6965ae58a1114310549c9e9863ca8823_46.png)

Once the expo is completed， we re the machine。Now， instead of seeing the actions we've seen before。

 we expect a message box to appear， right。From setup platform。Here is the message box。 Now。

 we can simply press on shift of 10， we spawn the CMD。



![](img/6965ae58a1114310549c9e9863ca8823_48.png)

We can change path to C drive。 if we check the B log status。

You can see it says protection is still on。And the volume is unlocked。

And if we list the files under the C drive。We can view all the secrets again。 Here it is。



![](img/6965ae58a1114310549c9e9863ca8823_50.png)

![](img/6965ae58a1114310549c9e9863ca8823_51.png)

Now， Mal alone， we'll continue to talk about the B par in the attack surface。Thanks， something now。

 So let's move on to attacking the B D parsing in Winore。

So for those of you who are unfamiliar with the BD and what it stands for。

 BD stands for boot configuration data。 and it's the file that defines how Windows Bo。

 It stores the boot entries， controls the boot parameters。

 recovery settings and many more boot related configurations。😊，Now， in the context of Winery。

 the usage of BD is considerably minimal。 Winery uses BD mainly to determine the location of the target S volume on disk so that Winery knows which volume to end the recovery operations to。

 right， because we have multiple volumes on the disk。

 Winry must know which volume to end the recovery to。 So when Winry starts。

 it reads the BD store coreries the target S volume from the BD and then adds the recovery operations to this volume that the B specified。

😊，So the first question that came into our minds when。

 when we investigated greeneno obesity usage is， what can we potentially gain from targeting it。

As it turns out， Win array places full trust in the target West。

 assuming that it's out of attacker's reach。Now， this assumption is actually solid from a design perspective because with bitlocker enabled。

 the target S volume is encrypted， encrypted and an attacker has no access to it whatsoever。

 So it's okay to place their configurations， query configurations。

 assuming it's out of attacker switch。😊，Having said that。

Since the target S location is defined in the B D， which we can modify。

 we began to question this trust assumption。Could BC manipulations allow us to undermine it。

So what if we could trick winery into thinking that an attacker control volume is trust is a trusted beat locker encrypted one。

 sort of confusing winery here。So our focus is shifted towards identifying a primitive that allows impersonating the target West location。

The goal is to manipulate the target US volume， to point to an attacker controlled volume。

 such as the recovery volume。 instead of pointing to the trusted beetle concrete volume。

 which we can't modify。But the recovery we can modify。

 So this primitive we've gained would break the trust assumption。Now。

 we already know that the target S location is controlled in the BD store。

 which we can directly modify。 So what happens if we change the location directly in the BD store。

Well， theoretically， it's possible， but it isn't valuable。 Let me explain。

The target S location isn't used only by Winery， but also used by the boot manager。

 which uses it to know which volume it should auto unlock for Winery。

Any change to the shirt store affects both the boot and the West faces。In this specific scenario。

 modifying the target location to point to the recovery volume prevents the boot manager from auto unlocking the bele curve grid to the west volume that contains the secrets。

 And these are the secrets that we want to extract。😊，So this breaks the trust assumption。

 But even with full code execution withinery， bit like her secrets remain inaccessible and locked。

 That's why it's not valuable。 So a primitive must not interfere with a book procedure and not interfere with auto lock functionality。

 So we must go deeper。We decided to dig deeper into the way that winner search for the BD store。

 Any potential flaws at this stage wouldn't interfere with auto lock functionality because it executes afterwards after the O S volume has already been unlocked。

😊，So to locate at the BD store， Wiery basically iterates overdi volumes and searches each volume for a BD store。

😊，The first order that is found is the one used by Winery to further calculate the target West location。

Now， this volume iteration logic is a very interesting point to further inspect。

 And the reason is that only the5 volume contains the BD store。

 So any attempt to locate this store on other volumes could potentially be abused。

 especially if the other volumes are controlled by an attacker。😊。



![](img/6965ae58a1114310549c9e9863ca8823_53.png)

Eternally， the functions that are used to iterate over your volumes are two standard Windows APIs。

 find first volume and fine X volume。😊，And notably。

 the re section of these two APIs highlight a very interesting point。

 It highlights that one you not assume any correlation between the order of the volumes that are returned by these functions and the order of the volumes that are on the computer。

😊，The reason being is that under the hood， these two functions perform further filtering and sorting。

 which causes an inconsistency in the return volume order。



![](img/6965ae58a1114310549c9e9863ca8823_55.png)

On the monitor， you can see the typical volume order returned by this part to this volume command。

The order is first to West， then EF I， and only then the recovery volume。



![](img/6965ae58a1114310549c9e9863ca8823_57.png)

However， the volume iteration functions would return a slightly different order。 First， D O S。

 than the recovery and only then the Eified。 Can you spot the inconsistency here。

With the volume iteration functions， the recovery volume is iterated over and checked for a BD store before the E5 volume。

😊，By placing a custom attacker control store in the recovery volume。

 we can trick Winery into using it instead of using the EF5 BD store。 And with this setup。

 Winery doesn't even reach the point of querying the EFfi volume for a BD store。

 It just uses the first store that he finds in the recovery volume。😊。

So any modificationification or corruption we make to that attacker store will only impact betweenery and will not interfere with auto lock functionality at all。

This flow allows us to achieve the desired remedymed during the boot phase。

 the boot manager locates and uses the legitimate VD store from the E5 volume to unlock the bit locker encrypted to West volume。

 So now this S volume， if we get code execution， we can extract the secrets from。

 and we are after these secrets。😊，However， during the O S phase。

 Winery instead locates and uses the attacker control BD store in the recovery volume。

 This is due to， to the lookup logic， which we just explored。

 And this leads Winery to consider an attacker control volume as the bitlocker encrypted volume。😊。

So we gained the confusion primitive。 And with this primitive。

 any configuration that Winery retrieves from the target us previously assumed to be beyond an attacker reach is now entirely under the attacker's control。

😊，To further exploit this far primitive。We need to find a winry flow with three characteristics。

 first。A flow that can be triggered either from Wiy U I or R H and XO scheduled operations so that we are able to trigger this flow freely。

😊，Second， a flow that doesn't trigger the realloc functionality so that we won't lose access to the secret。

 right， Because we need to access the secrets。 And if we trigger a flow that triggers the reallock。

 we won't have access to the secrets。The third characteristic is the flow that cores configuration from the target West to perform sensitive operations。

😊，And this is important。 The idea here is to change， change the winery flow that we。

 we will potentially find with again， the impersonation primitive to trigger sensitive operations in the auto lock state operations that we wouldn't have the possibility to control beforehand or to trigger beforehand。

😊。

![](img/6965ae58a1114310549c9e9863ca8823_59.png)

After exploring the different wineery behaviors， we found a potential candidate。

 the Bush button reset feature。Or P，BR， for sure。So PBR is Windows system reset tool。

 and it provides various risk functionalities。

![](img/6965ae58a1114310549c9e9863ca8823_61.png)

It supports multiple trigger modes with one of its modes， known as online PBR。

 meeting the characteristics that we defined for you exploit。😊。

Only PR can be triggered truly as a scheduled operation in our age in X MO。

 It doesn't trigger the wheel functionality。 therebyby it benefits auton lock。

And it course configuration from the target us to perform sensitive operations。

 therebyby these configurations， although wouldn't be。

 we wouldn't be able to control them before the impersonation。

 We cannot now control this configurations with the impersonation primitive。Specifically。

 the sensitive configurations that we identified was an X M L file named reset session that X M L。

 which defines all operations executed by PBR。 Notably， it includes the dec volume directive。

 which instructs BBR to decrypt any arbitrary beat locker encrypted volume。😊。

So this is very interesting。We now have all the pieces that are required for the full beat locker bypass。

The expert required modifying few filess in the recovery volume。 First， the X， M L， R and X。

M L with online PBR operation。Second， to modify the BD store with the OS S device pointing to the recovery volume instead of pointing to the O S volume。

 and then to modify the PBR configuration， specifically with brief succession and the dec volume operation specifying the beat locker encrypted S volume as the target volume to decrypt as part of PBR。

😊，With this setup， the next winary boot will go through online PBR flow during the BD store lookup。

 PBR will locate the attacker BD store and mistakenly identify its target volume as a recovery volume。

😊，Consequently， PBR will load its configuration from the recovery volume。

 which instructs it to decrypt the bele encrypted or s volume。😊，Once BBR completes。😡。

Bit locker be secrets become freely accessible。 as beat locker was essentially disabled by PBR。

 So we went from limited a Western impersonation primitive to an arbitrary volume decryption primitive。

😊。

![](img/6965ae58a1114310549c9e9863ca8823_63.png)

With that， let's see the demo of the full exploitation chain。So we have here a locked machine。

 We don't have the password to it as usual。 So we select restart。

was taking the shift key to reboot into Winery。While inside Winery， we select troubleshoot。



![](img/6965ae58a1114310549c9e9863ca8823_65.png)

Advanced options。 and then command prompt。 at this point， the recovery key will be requested。

 which we don't have it。 So we skip this drive。 And as a result of skipping the drive。

 You'll see that the sea drive is locked。 The main O S is locked。

 We don't have access to the secrets whatsoever。 As you can see， the says of beat lock curve。



![](img/6965ae58a1114310549c9e9863ca8823_67.png)

I's fully locked。So we'll now change drive to the D drive。 This is the mountain recovery volume。

There are Xy files directory where we place the exploit。 We execute the exploit。

And now all that we need to do is to restart intoery in order for the exploited trigger。



![](img/6965ae58a1114310549c9e9863ca8823_69.png)

So， now we'll restart。Interinery and the PBR will， will start。It will take a few。

 it will take a few seconds。 So we fast it forward a bit。 So you'll see on the screen now。

 PBR is sitting this PC。 But let me tell you what the XO did。

 It basically is scheduled only PBR placed the BD store under recovery volume。

 when PBR looks BD store。 It locates the attacker B store。

 This store pointed to the O S volume to store pointed the recovery volume as the O device。

 we place the PBR configuration in that device as well。

 And this PR configuration tells PBR to decrypt the O volume， the main volume with the secrets。

 So under the hood that happens here is that the manual volume that was encrypted is now being decrypted by PBR。

 This is what happens under the hood。😊。

![](img/6965ae58a1114310549c9e9863ca8823_71.png)

So now， BBR finishes。

![](img/6965ae58a1114310549c9e9863ca8823_73.png)

And we continue into the main West。We'll go back to the main US。 Now， you will see the logan screen。

 At this point， of course， we still don't have the password。 Nothing has changed。

 but let's reboot into Winery to check what is the coin state of beat locker。



![](img/6965ae58a1114310549c9e9863ca8823_75.png)

So。We are back in Winery with like troubleshoot events options， command prompt。



![](img/6965ae58a1114310549c9e9863ca8823_77.png)

But the recovery key is not requested here。 If we change they to the sea drive to the main US。

 you can see that we have access to the secrets。And if you wonder why we have such access without reing the volume without requesting the recovery key。

 let's say， what is the coin state of beat locker。You'll see that the protection is off。

And the drive is fully decrypted。 So essentially， disabled bit locker able to extract all the bit locker secrets。

😊。

![](img/6965ae58a1114310549c9e9863ca8823_79.png)

![](img/6965ae58a1114310549c9e9863ca8823_80.png)

Thank you。So closing remarks， starting with the vulnerability fixes。

 So all the discovered vulnerabilities and their exploitation techniques were fixed in July  which Tuesday。

 The Cs vulnerabilities that we presented today are displayed on the screen。😊。

To further enhance the security of beat locker， we recommend enabling T PMM plus spin for pre authentic。

 This significantly reduces the beat locker tax surfaces by limiting exposure only to the T PMM。😊。

So if TM plus spin is already enabled on your machine。 or if you enable it， you're not。

You're protected from， you're protected， fully protected from Blocker byes that originate from winnerry。

 specifically from boot components from logogo screen and other attack services。



![](img/6965ae58a1114310549c9e9863ca8823_82.png)

To mitigate bit locker downgrade attacks， we advise enabling the revised mitigation。

 This mechanism enforces secure versioning across critical boot components。

 preventing downgrades that could reintroduce non vulnerabilities in beat locker and secure boot。😊。

For the full article， belowlocker counterner measuresa， please scan the QR code on the screen。Lastly。

 we want to emphasize that our journey never ends。 Well continue to proactively research beatlocker。

 Winery and the related components with the never ending goal of increasing their security and resiliency。

😊，It was a pleasure to share with you our research project and the security work that we've been doing on Winery and beatlocker。

 If you have any questions， please catch us in the wrapper room and feel free to reach out on social media as well。

 And with that， we would like to thank you all for joining us today。

 and we wish you a great rest of your day。 Thank you。😊。

