# Dark Corners： How a Failed Patch Left VMware ESXi VM Escapes Open for Two Years [MhQmaK8Zsfw]

Hello， everyone。 I'm Yu Haojiiang。 It's an honor for me to be presenting here at Bhead。 Today。

 my colleague Zmin and I will be presenting dark corners。

 how a failed patch left being where S X I V N escape open for two years。

 This research was a collabo effort between myself， Xinlei and Zmin。😊，First。

 let me introduce who we are。 We are security researchers and and group year security lab。

 and we have successfully exc on virtual machines many times。 And also。

 we won the pony award in 2023。😊，Next， here's our road map。

 We will start with a brief introduction to E， S X I， and our E。

 S X I escape consists of two main components。 First， escaping from the virtual machine。😊，And second。

 escaping the E SX I Sbox。Finally， we will conclude with a demo video。

So let's get started with the introduction。Before diving into the technical details。

 let me first share what brought us here today。 Earl this year。

 V Nware disclosed several vulnerabilities that respond represents a complete exploit and were confirmed to have occurred in the while。

This really highlighted the potential impact and danger that E， S。

 X I vulnerabilities can pose in real world scenarios。

Since we successfully demonstrated a VN E S X I VN escape at the Tianfu Cup 2023。

 we thought it would be valuable to come here and share something interesting things behind that story。

😊，Now， let's take a look at the E S X I's architecture。As a virtualization platform。

 it's pretty much the same as whenware workstation。However。

 the underlying host host O S is replaced with VN kernel， which is a specialized hypervisor kernel。😊。

Additionally， it has a sample system that provides fine grain permission control of each process。

So now let's move to the virtual machine part。Let me examine the text surface of E， S。

 X I hypervisor， first。I've put together a table here。

 which is an update version from my presentation last year with some new information added。

It provides a comprehensive record of all public disclosed critical vulabilities and their affected models over the past few years。

As we can see， the current attack surface of V Nwares hypervisor primary consists of virtual devices and guest R PCC。

Most vulnerabilities prior to 2024 were USB related。

 These include the virus USB controllers and Bluetooth device。

 V Bluetooth device is connected through U T UH CI controller。However。

 I saw that V Nware has since moved the re removed the Bluetooth device。

 So this attack surface may no longer exist。And this year。

 we've seen vulnerabilities emerge in other models such as C S I， VM， X， net 3 and VMC I。Essentially。

 what we are looking for are models that can be triggered from within the more virtual machine。

 but won't be interfered with by the gas operating system is itself。And then。

 let's talk about a story。Our story begins with an ancient vulnerability。In 2023。

 when we decided to participate in Tianfu Cup， we started looking into V where。

We examined the most recently patched vulability at that time。

 which was the one reported during 2021 Tian Fu Co。

 This vulnerability was discovered by way from Kun La。 And as we can see。

 it's a use after to free vulnerability in the actual C I USB controller。

Before we continue sharing our analysis work， let me briefly introduce some extra C I objects。

 The blue parts are from the extra C I specification， and the purple parts are V and well specific。

First， the slot object represents a USB device。 Each device has 31 endpoints for configuration out data transfers and in data transfers。

Each endpoint can have string contacts that correspond to T R rings。

 which store the data data packet。Now， the， the purple parts， these are VNware S object。

 The pipe manage the endpoint data transmission， and U RB represents the data packet being processed。

And all U R B are manage managed by the pipe。And the key change of the。

 this orientation vulnerability were located at two H C I command ring handleer functions。

And the changes were reordering the execution sequence of slot context rewriting and evoking extra C I clear string contexts。

We can see this from the red boxes in the， in the string course， swing source below。

This means that in the older version， we can modify the slot context before executing actual C I string。

 clear string context。So what can we do with it。Let's take a look at actual C I clear screen con context。

It executes X C I， delete string contacts。And it's sequentially excuse X C I fetch pipe and extra C I clean pipe。

In exercise clean pipe， we can see it first checks whether a pipe pipe exists in the temporary endpoint variable。

 And if it exists， it will execute cancel pipe， which free all U RB is managed by the pipe。

But this also means if extra C I fetch pipe fails， cancel pipe won't X be executed at all。

So what can we do in X C F H pipe。We can see that it reads content from the stored content。

 then performs a check。If the check fails， it directly returns 0。Then there won't be any pipe in the。

 in this endpoint， temporary variable。So the flow is like this。

Normally when we delete string contact， it executes cancel pipe， free U R Bs。

 and finally free the string contacts。But if we can modify the stored context。

 it will skip the pipe processing and directly execute the following code。Now。

 we can leave the pipe not freed after screen contact has been freed。 What can we do next。

We find that the U RB have a pointer to the string contact。 And when a U RB finished。

 it structure the U RB packet size from the string context。

Since we can skip pipe processing when deleting string contacts， the U RB and pipe remain。

But the string context is already free。So if we can manually clear the pipe。

 it will free the U R B and causing it to subtract value in the already free string contexts。

This is the UAF vability。After understanding this old vulev。

 I decided to look at VNware's latest version at that time。

And I discovered that V Nware had added new code to the X C I fetch pipe function。Essentially。

 it introduced a new way to fetch pipes。 So there are now two approaches based on the slow state。

This allows me to modify this specific slot member to achieve the same effect as the original vability。

To making it unable to find pipe after being modified。 So it doesn't process。

 It will doesn't process the pipe at all。So the flow works like this。 But to to the time constraints。

 I won't go into details here。Yes， so we find a new vulnerability， right， This looks pretty exciting。

😊，But wait， the story is not that simple。 Actually， this Asian vulnerability holds even more secrets。

Actually， we don't need to need new code to make the H C I fetchpay function fail。

We find that the patch never succeed。Let's take another look at the X C I clear screen context function。

It， it actually has an E P I D parameter to subs the endpoint。

From our earlier introduction to S C I object， we know that each endpoint has its corresponding string contact array。

This means that the X C I clear string context only deletes the string context of a specific endpoint。

But the content of the entire slot has been already， has already been modified by us。

So we can modify the slot content， then clear the string contact of an endpoint。 And next。

 we can execute a dis slot command to clear all endpoint string contacts， and we can trigger the UF。

😊，So it's never been successfully patched。Now， let's go to the exploit part。

Let's take a closer look at this UF vulability。This vulability actually presents enormous challenge for our exploitation ex efforts。

This is a very constrained UF。 First， it only affects the plus 0 x 20，5 c offset of the free trunk。

 and the use operation is subtracting a value from the4 by value at that address。

This brings several difficulties。 First plus 0 x 2，0，5 C is not a 64 by B aligned address offset。

This means if we want to use this UF to modify a pointer in the hip trunk。

 we can only modify the high4 bys of that pointer。But modifying the high for byte of a pointer is complete。

 completely meaningless for exportation。Because even our plus one change would make the pointer span across 4 GB of memory。

Second， plus 0 x 52，0，5 C is also a very large offset value。

 which means we need to do much better when he grooming。

So we need some approaches to solve this problem。 This is also quite challenging for a close source target。

Fortunately， I found a perfect primitive。😊，This perfect primitive is hash map。 I'm sure you。

 you all know what hash map is。 And V Nware's hash map works like this。

 Each element has a value and a key。 They are stored in a hip trunk。

 And when storage exceeds the capacity， the hash map will double in size。Even better。

 the X C I string context hashmap has exactly what we need。Each element is 1。

8 by pointer to the string contact， plus 4 by I D。That's exactly 12 B total。So this is perfect。

 Our UF now can modify the second element where the pointer starts at offset plus C。😊，Now。

 we can modify the low four bys of the， of this string context pointer。With this primitive。

 our constraint， UF suddenly has enormous potential。

 We can make the this pointer point to a fake string context that we can control。😊，Let's。

 let's look at what other primitives we have at our disposal。The first one is U R B。

 This is an object we shared about last year， and we use it to develop a develop a completely new set of exploitation primitives。

😊，It's very powerful。 We can arbitrary control its size， allocate or free it。

 And it has a data array with the lens member that manage the array lens。

 And it also has many pointers， making it extremely useful in various aspectss。😊，Here。

 I'd also like to share something we didn't discover last year。

We find that the VNware USB arbitrator program in VNwares Linux version hasn't had it symbol stripped。

So we can obtain many USB related symbols， such as the symbol of U R B。

This has been very helpful for our ongoing research。

The second set of primitives are M O B surface and GMMR。 They are all belonging to V N S VGA model。

 which is the graphics model。 And for detailed information。

 you can check out the blog post published by VDI in the 2018。Through these objects。

 we can easily perform hip spraying and hip roing。At this time。

 we actually have enough primitives available。 It's time to combine them together。

 to complete our exploitation。To better share this with you， I've splified the relevant content。

 The actual heat layout and operations are more ex complex than this。So first。

 we need to perform an information leak to obtain hip addresses。

We start by allocating a string contacts， then trigger the v。It will get freed。

 and we can subsequently subtract a value as， as at its0 x 2 or，5 c offset。Next。

 we allocate a GMMR with a 0 x 2，0，5，0 sized hip trunk， which will occupy the front part。

 At this point， our capability becomes modifying the value at offset plus 0 x C of the subsequent hip trunk。

We can then allocate a U RB right after GMMR， and the U RB's actual lens field happens to be allocate。

 located exactly at the plus 0 X C position， perfect。😊，Now。

 when we trigger the use operation in our UF， we can sub the U RBs actual lens downward to a large value。

 All us to perform auto auto bound read of the content after U RB。

If we prearrange content that contains hip addresses in the memory region behind this setup。

 we can successfully leak those addresses。😊，After obtaining hip dresses。

 we can start forging various objects in this exploitation。

 since we are using the string context hashm as our primitive， we need to forge string context。

We can place a string contacts in the subsequent memory， then trigger the vulnerability。Similarly。

 we only free the string contacts and then use the hash map to occupy the appropriate position。

When we trigger the use operation of UF， the original pointer will get subject by a value。Ultimately。

 pointing to our fake screen context。Although we can't directly do much with this fake string context。

We can use actual C， I clear screen context function to free it。At this point。

 it's still part of U RB's hip trunk。So we can achieve hipap overlapping capability。

Since E S S I use uses G C to manage hip， this hip overlapping capability is quite powerful。😊。

Finally， we can move on to the control for hijacking step。

We now have hip dresses and hip overlapping capability。 The next step is to find a place。

Where we can call function pointers。We discovered that the function that sub the U RB size from the screen contact is actually a function pointer in the V USB device object that exists in the U RB。

Therefore， we can for the U RBs V USB device object to achieve arbitrary address call。At this point。

 we've actually complete the virtual machine escape。 However。

 to execute the subsequent sandbook escape， we need to。

 we need the ability to execute arbitrary shell code。

The method to execute shell code is to find gadget to perform stack migration。

 then use R P to executable， to create executable memory， copying the shell code and execute it。

So I've concluded our story about E， S， X， I V and escape。 Now。

 let's review the security behind all of this。We discovered that C 2021，2，2，0，5。

0 was never correctly patched。We reported this issue in 2023， and it received a new CVE， CVE 2024，2。

2，2，5，2。However， nobody has pointed out that these two vulnerabilities are actually the same。

We also V Nwares announcement about thevolilities being exploited in the wild early this year。

Which demonstrates the real value of E， S X I escapes。But， on the other hand。

A vulnerability from 2021 wasn't truly fixed until 2024。This phenomenon deserved reflection。

I think there are several reasons。 First， and most importantly。

 when we sponsoring program is significantly lower than the actual value of these vulnerabilities。

This leads to fewer people being willing to report critical vulnerabilities to V and well。

And more security researchers choose to use these vulnerabilities for security competitions like Tian Fu Carb and Ponttu own。

 But these once a year competitions inevitably extend the lifespan of vulnerabilities。😊。

Alternatively， more exploits might be used in the wild rather than being disclosed to secret competitions。

Second， V NY is the code source software with higher barriers to entry。

Resulting in very few researchers studying V and where。This leads to our third point。

 The community lacks VNware related technical sharing。

 This is also our motivation for sharing VNware escapes at Blackhead for these two years。

The next part is how to escape E S X I's sandbox。 I will leave the following time to my colleague。

 Z Ming。Hello， everyone。 Today， I will talk about Yes X， I sandbox escape。 After a Ram escape。

 we can run cold on the host system， but the V X process is still is still inside a sandbox。

 It means we can do many things like turn on S S H。

 turn off the firewall to get full control of the host。 We must escape the sandbox。😊，First。

 let me explain how the sandbox work。A yes success system has only one user， root user。

 It use security domains to limit process access to files， networks。In the shell。

 we can use this command to see the sandbox roof。E， X L has over 100 sandbox domains。

 Each process rests in one。When a， when a virtual mention start， yes S， X。

 I creates a new soundbox domain for the VM X process。 We focus on it。😊，Looking at the roof。

We can see restrictions on this call。 Some rules are easy to understand， like L C T L。Open says。

 but some like Wem says， we need to know what they mean。

In order to know which this course can be used， we must look at the wind kernel。

The women with symbols can be extracted from this field in the system。We can locate this file。

 using the find command。After checking the kernel code， we learn。Wmconnell has three Cis tables。

The table used depends on the Ci number。If the number is less than 400， it use in Ciical table。

 for example， open read， right， same as Linux。And we can add over。700 new ci calls。

 just like in the picture。It seems like a larger take surface。

But not all ciscos are allowed in the sandbox。Sundbox restrictions on Cisco are mainly in this function。

It does two checks， first。Check enforcement level， lets like say Linux issue domain has a level。

 enforcing， permissive， disabled。If the level is0， disabled， the function directly returns success。

And if the level is enforcing， then check says mask。Each domain has an access mask。

 a number that shows what it can do。Wmconll defines many assess class types like generic where Mac says each bit in assess mask means one class。

And each cico belongs to a access class。 For example。

 if a domain has both generic size and Web max permissions。A mask would be would be 3。

And this function is a Ci function。 It belongs to generic says。啊，so一次loud。

If we can list every cisco says class， we know which cisco we can use。😊，Next。

 I will talk about how to switch to another sandbox domain。There are two ways。First。

By adding security do to the parameters in this cisco。This parameter can be an int。

 It domain index or string domain name。If you want to test your program in a sandbox domain。

There's an easy way， just adding this。To the command line。Can we escape the sandbox this way。However。

 only pre domains and transition domains can use this method。The list as shown in the figure。

Another way to transition to a sandbox domain is when the binary has the way more security attribute。

Here is get X， A T， T， R function。The label of domain is used to find the domain object。

Fund and transition。the same question， is it possible to escape the sand voice by settinging the X X A T TR of a binary file。

The answer is no。 First， this， this coin is blocked in the sandboxs。 We need VM K says permission。

And second， issue domain has a list of load targets， for example。

V Max domain can only switch to TPM domain， as shown in the picture。

A linked list is used to store all domains allowed for transition。Now。

 we can fully understand the sandbox rules。The command shows rules in four parts， so。

Fail ci call and transition。 Each part can be a way to take the system。While checking the rules。

 I saw something interesting。 The Web max process has read and read access to CBT device。What is CBT。

Changed block tracking is used to track which parts of Vs disk have changed。嗯。

Let's look at这个 CBBT driver。The CPT driver is a few device service driver is added to the kernel with this function。

The main function is CB，T L， C， T， L。 We call， We can call it in two ways， first。

Open the default device， control， control device， then use LC C T L。Second。

 use CBT make Dev to create a new device。For example， create power1 device， open and LC C。

These two ways go to different code passes。 We focus on the second way。

CBT make make a de with the first function called this function。Create a CBT Dev object。

Sttors the fill handle entered by the user。And use this function to get the few sets。Finally。

 create a bit map object based on the few sites。 The beat map is used to track changed blocks。

The vulnerability in this function， update beatm。Which cause an O B rate based on the offset and size bad user。

That doesn't check well。So if we can enter this function， we can right past the end of the beat map。

 Got O W。Primitive。But there is no problem before this。The code calls this function。

It will check the set and size can't be larger than the few size。But， here is the trick。

The beat map says is set when the file is first checked。But if we make the file bigger later。

 the bit map size doesn't change。 So let's write more content to the file make the feel bigger。 Now。

 our of set and size may be less than the new file size， but the bit map is still small。

 So we can write past it。 Now we have an O B right prim。Now。

 we can trigger an O B write on a help object。 and which object can we over write。 First。

 I thought of looking for objects in the ri kernel。 However。

 the help space between the driver and the kernel is isolated。

 It means that we can only override object in the CBC driver。The C。

 the CBT driver has only 15 functions and is really， really， very small。

There are few objects we can use。But there is a good thing。

 The wind kernel will don't will not do other memory task to disturb our happy layout。

 It makes the exploit very stable。😊，We still have a good target。 The bit map object。

This object has two fields。Beta map pointer， bit map sets。

 We can read or write the memory at bit map pointer using L C， T， L。

By modifying the bit map size field with an O B right， we can get an O B read。

 then click the kernel address。Use this copy copy out。By modifying bit map pointer。

 we can get A W prime。But we can't modify a pointer address to another pointer address because it's an all operation。

 We can only turn 0 bits into  one， not one into 0。 For example， we can modify 41 to F F。

But we can't modify 41 to 42。So if we can set bit level pointer to 0。We can change it to any address。

Now， let's take the women help。The women can help is like hip is like dies hip。

Each memory block has a small header， size and flexs our plan。First。

You also be right to change the size of the next chunk。Make it very big。

 big enough to cover the AW bit map。Theneng free the victim chunk。

The helper may merge it with free space。Now， when we relocate。It can clear the AW beatman。

Including a bitmb pointer， set it to0。Now， because map pointer0， we have A R or AW prime。Step  one。

 overr victim bit mapap sets to get O O B Ray read and leak corner address。 step 2。

 Overrite victim chunk sets and then release the chunk to control the bit map pointer to get a W parameter。

Step 3， use A W primitive to modify Cis mask table and call this function。

 It's a Cis function to close the sandboxs。K。I explained how the E， S， X。

 I sandbox work and found a bug in the CB driver， C E2024，2，2，2，5，4。

 and use O B right and hip tricks to escape the sandbox and got full control。

it shows that even small drivers can be dangerous。Hope more researcher will focus on Wikind security。

Next is demo tab。Go。Okay， so we will conclude our presentation with a demo video。

 And this video is recorded before the Tian Fu Cup，2023。And this is the victim target。

 And we run the E S X I on it。 And this is the Windows get gas operating system in the in this virtual machine。

 And we assume that we have already control this gas virtual machine。

 And so we downloaded the P P exploit。 and then we will execute。😊，This is our exploitation。So。

 it is now executing。It will download some files， and execute them。And says some environment。

Now it's worked。 And then we use the cable to connect to our attack attack target。

 We use it to connect the E S X I。 and we reserved our shell。 And yes。

 we point so we can control the E S X I with the root primitive we we root privilege。Yeah。

 so this is our demo。And thank you for listening。 And do you have any questions。

And if you have questions， you can also later the to the web room。 Yes， sure can you。い。1%。Sorry。Yes。

 no， no， they haven't changed their bonddy program and。For my info。

 from what I know is really low compared to other vendors like Microsoft or Google is really low。

 low bunty program。Do have human name likes Do you want to step up to themic。对几啲音。

VM escapes usually require privilege elevation inside the guests。

 or is there sufficient a attack service。我 the查。Yes， actually。

 most of the VM escape needs a root privilege of the in the gas system because the attack surface of VN A VM escape is。

the virtual device。 And we also more normally， we need rude privilege to communicate with these hardware devices。

 although it is a virtual device。 So， yeah， so but。

Maybe they will have be have some attack surface with without we don't need a real privilege privilege。

 But I think most part， we need a real privilege， thanks。Is there any other questions。Okay。

 thank you for listening。 And if you have more questions， you can ask me later。

