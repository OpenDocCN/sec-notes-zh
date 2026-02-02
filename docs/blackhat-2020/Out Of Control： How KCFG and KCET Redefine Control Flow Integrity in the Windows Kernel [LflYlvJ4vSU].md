# Out Of Control： How KCFG and KCET Redefine Control Flow Integrity in the Windows Kernel [LflYlvJ4vSU]

Hello and thank you for coming to my talk and thank you to Blackca for having me and allowing me to speak。

 My talk today is going to be about the control flow integrity mitigations on Windows。

 specifically why they're important， what they do， but most importantly。

 taking a look at the implementations of these mitigations and kernel mode as opposed to their user mode counterparts。

😊，So a little bit about myself。 My name is Conor Mcgar。

 I am a software engineer at a security startup called Prelude。

 and I'm just primarily interested in anything related to Windows internals， exploits， mitigations。

 hypervisors， all the typical fun stuff。So before talking about what CFI actually is。

 it's probably worth taking a look at what is CFI aiming to address？Well。

 most exploits today that want to execute some sort of unsigned code typically need two things。

That is firstly， the ability to coerce a given exploit target to exercise a code path that it might not otherwise exploit。

 and then using that same primitive， the ability to load data into an exploit target and have the OS or application treat that data as code。

And so CFI is attempting to address this first tenet of exploitation。

 which is verifying and mitigating any attempts to alter the legitimate control flow of an exploit target。

So the canonical example of this in Windows is control flowguard， it's been around since Windows 8。

1 in user mode， and this is Microsoft's implementation of a forwards edge CFI mitigation。

 so this is protecting against indirect calls and jumps。

And the way that CFG works is at at compileile time。

 all of the known indirect call targets are stored in a bitmap， and that bitmap is read only。

 and it is a per process bitmap。 And any time an indirect call or jump happens。

 we first go to the bitmap to get more information about the call target we're about to invoke。

So for our intents and purposes， really CFG， the bitmap states are this is allowed or this is not allowed。

 the way that manifests itself is a little bit different because there are some memory manager optimizations that are made to the bitmap to compress its size and depending on how the compiler generates call target boundaries。

 we need two bits really to denote this， but for our intensive purposes， as I mentioned。

 it's allowed or it's not allowed。And so here's a kind of before and after of the CFG days before。

 So we have an exploit in this case， let's say it's a C plus plus or some kind of browser as an example。

 there's a virtual function table associated with an object and there's a list of functions。

 Well I's tacker with a readwrite primitive can locate that virtual function table。

 overwrite one of the entries， and then coerce the application to actually call that function。

 Well in this case obviously instead of calling the legitimate call target。

 we call into ro gadget or stack pivot or some other deviation of control flow。

And so in the after CFG days， everything about the exploitation remains。

 but instead of just arbitrarily calling whatever we were told to call， we first go， hey。

 let's go check with CFG， let's use the call targetss address as an index into the bitmap。

 get the associated bit state and see if we should actually make this call or if we should terminate the process。

So we can see C， FG it does have an an effect on exploitation， but attackers do what attackers do。

 And that is， they take the path of least resistance。

 Why would I mess with CG and try to find every way to bypass its implementation。

 Instead of doing that， look for some other way to transfer control flow that is not protected by C。

 FG。 Well， one obvious one comes to mind that's return addresses on the stack。

 You have a list of return addresses on the stack。 A function unwinds。

 And then eventually that return address is picked up， and we deviate somewhere in memory。Well。

 Microsoft knew about this obviously since the beginning。

 even when CFG was being developed and they produced a softwarebased implementation of this called of return flowguard。

 but it was deprecated because of some discoveries by their internal red team。

 which effectively the source of truth for this mitigation could more or less be deterministically leaked and Microsoft did the correct thing in my opinion。

 again not that anyone cares， and they said hey， we know we can't do this in software。

 let's do the right thing and wait for a hardwarebased mitigation to come to fruition。

And Intel's implementation of this is called control flow enforcement technology。

 specifically the shadowstack。But Windows also has support for the AM MD Shat feature。

 and it's been in user mode since Windows 101903。And with CET， again， this is a hardware mitigation。

 so we need newer processors and these processors have a new architectural register。

 the shadowstack pointer or SSP， and the shadowstack pointer cannot be encoded in a memory operation as the source or destination and for specifically user mode CET。

 there's a special right user mode， shadowt instruction which can only be executed when the processor is running at current privilege level zero or kernel mode。

 so it's undefined in user mode， So for all intents and purposes。

 the shadowst is completely immutable to a user mode attacker。And anytime a call occurs。

 instead of just updating the instruction pointer and pushing a return address onto the stack。

 we now push a return address onto this immutable shadow stack， and any time a return occurs。

 we pop the in-s scopecope return addresses of both the shadow stack and the regular stack and we compare them if they mismatch the CPU issues a special interrupt。

So， obviously， these mitigations are doing a lot to thwart exploitation。

 but you may have noticed a few things so far。And that is both of these rely on a given source of truth。

 So as an example， user mode， C， E T and user mode， C。

 F G are protected by the user kernel security boundary。 If you want to write to the shadowst。

 you either need some way to ask the kernel to make it writeable or have some primitive where you can execute that special op code while the processor is running in kernel mode and same with the bitmap。

 you need some way to actually make it rightable， This is usually done through a system call。 Well。

 if you have some way to execute code to do this in the first place， obviously。

 these mitigations are nonstarters for you。 So we have the user kernel security boundary。

But the problem is， if you try to implement this mitigation against a kernelron mode attacker。

 but the mitigation itself is in the same privilege boundary as the attacker you're defending against。

 obviously you're not going to have a good time with your mitigation。

An example of this is a kernel mode exploit。 We run in user mode。

 but we have an arbitrary readr in kernel mode， and everything about the exploitation remains the same。

We locate some list of functions and memory， we corrupt it with our own call target。

 and then we coerce the kernel to actually execute that in scope corrupted call target。

We just have to add two more steps。 and that is， we just simply use our primitive to locate the bitmap in kernel mode。

 We then locate its associated page table entry， which is a special structure that is used to map physical to virtual memory。

 and it contains special control bit such as is this a copy onright page， Is it dirty。

 has it been accessed before， I it kernel， I it valid， Is it ridable。

 is it executable And one of those things is obviously， as I mentioned， ridriable。

 So what's stopping us from locating the PTE， which is stored in kernel mode。

 marking the bitmap as ridable， then just corrupting the bitmap to mark our ro gadget as valid call target。

 Our exploitation only requires two additional steps。

 If we implement this mitigation in the same privilege boundary as the attacker。Well， luckily for us。

 there actually is a higher security boundary that we have on Windows and that is the Windows hypervissor。

 hyperV。And so it's been a while now， but Microsoft implemented a suite of features under the umbrella term virtualization based security。

 and this provides a mechanism to guarantee the sources of truth for things like CFG and CET。

So the way that VBS works is it leverages second level or second layer address translation or nested page tables。

And what that actually does is VMs are in an isolated region of physical memory。

 So VM1 and VM2 they access what they think is physical address 1000。

 but there's only one physical address 1000 on the actual machine。

 So VM just access memory in context of VM and there's a secondary mechanism which performs a final level of translation to say。

 hey， that memory you think you're accessing it's actually over here at this system physical memory。

 So using this， we can actually break out the operating system into two what's called virtual trust levels or VTLs。

Which are effectively V Ms in the sense that these are isolated regions of physical memory。

So we have VTL 0， this is our normal world， this is typical anti kernel page tables。

 the windows that we all know and love， and now we have secure world or VTL1。

 and it's called secure world， not because it's inherently secure。

 but because it is allowed to impose its will on the security policies of VTL 0。

So an example that we'll walk through here is something called kernel data protection。

 which is a newer mitigation under virtualizationbased security。

 which effectively guarantees a region of memory in VTL 0， which is where normal Windows operates。

 always remains read only， regardless of an attacker has a kernel mode readwrite primitive。So again。

 we have a kernel exploit here。 We're running in user mode and we leak the associated memory of a KdP protected region of memory。

 We locate its associated page table and we mark it as writeriable。

 So now we actually go to do the right operation。 But in this case。

 where is the right operation happening in Vtl0， which is a guest like a VM。

 And so that memory access is then delegated to the hypervisor。

 which performs the additional level of translation。 Well。

 what KDP does is it sets a read only S entry for Vtl0 saying actually this page is read only。

 and those are manifest through the extended page table entries that only the hypervisor has access to。

 So in this case， even though a kernel mode attacker marks the PTE。

 which is managed by the regular kernel as writeable。

 the extended page table entry is the final source of truth， which says read only。

 And so this is effectively the same as writing to a read only region of memory and what's called a Ept violation or an extended。

Page table violation will occur and the machine will crash。So as you can see。

 we actually have ways to guarantee the integrity of the memory in V T L 0。

 the traditional kernel and what a normal user interacts with。But it's not as simple as just going。

 okay， take everything we have in the user mode implementations and just shove them in kernel mode and add some hypervisor stuff in there。

Engineers have to think about things。An example of this is VM exit。

 So your processor is running in context of a guest。

 It then the guest then invokes the services of the hypervisor， kind of like a system call。 Well。

 the processor then goes to run in VMM mode or hypervisor mode。

And that incurs a context switch where a processor has to update its state and a bunch of other things。

 Well， that's not a free operation by any means。 So just the sheer fact that requesting the services of the hypervisor can invoke a VM exit。

 That's something we have to keep in mind。 So with that。

 let's take a look now at the kernel mode implementations of these mitigations。So right off the bat。

 we can see that kernel mode CFfG， it came around in Windows 10 Redstone 2， which I believe was 2017。

 a couple additions into Windows 10。 So user mode user mode CFG comes in Windows 8。

1 as an optional update。 We take a lot longer to get the kernel mode implementation。And K C。

 F G is only fully enabled when another feature of virtualization based security。

 which probably most of us have at least heard before， Hyvisor protected code integrity。

Or HVCI is enabled。 So only when that is enabled is kernel CFG fully enabled。

 And HVC I works very similarly to KDP in the sense that it tries to guarantee the integrity of memory in V Tl 0。

 But what it is looking at is pages which are designated as code becoming data pages or data pages becoming code。

 Those are two prerequisites for unsigned code。 If you have a code page that's probably readable。

 executable。 If you want to manipulate the contents， it needs to be ridable。

 If you have a page that's ridable。 and you want to execute it。 It needs to become code。

 That's what H VC I is looking at。So what happens here is the N kernel is still responsible for designating where the kernel mode CFG bitmap is going to reside。

 and there is only one kernel mode CFG bitmap， so every process has its own CFG bitmap。

 but the kernel mode address space is shared and so there's one two terabyte region of memory which is the kernel mode CFG bitmap that all of the images share。

The anti kernel is responsible on kernel initialization for determining where this is going to live。

 and it enlightens the secure kernel through a mechanism called a secure system call， because again。

 these are isolated regions of physical memory， the regular kernel and the secure kernel。

 they don't know about the layout of each other。So this secure system call occurs over the hyperca interface。

 So V T L 0 or N T asks the hypervisor， hey， send this request to the secure kernel as an example。

 And so on initialization， the secure kernel tracks that region of memory associated with a bitmap through an N T address range or normal address range or NR structure。

And this NR structure is what allows the secure kernel to denote， hey。

 I know this region of memory is important and it resides in the regular kernel and I need to manage it。

Typically this comes through every kernel mode load image results in a NR of postboloaded drivers being created which contains the range of memory associated with all of the executable code of that image。

 but we also have special items which are not directly tied to kernel mode image such as the CFG bitmap or the shadow stacks。

 so those are regions of memory the secure kernel needs to know about and they get what they get what is called static NArs。

So after the secure kernel receives where the bitmap is going to reside。

 the following steps happen on every subsequent image load。

 So every kernel mode load image that happens that has， that is compiled into CFG support。

A new allocation is going to occur from the bitmap range。 Now， obviously。

 we could not commit two  TBabytes of memory at once that would exhaust the commit limit。

 and most pieces don't support this。 So one of the optimizations that's made is two terabytes are reserved and then on demand allocations are made as images are subsequently loaded。

 And what happens is that allocation is then marked as read only in the S table for Vtl 0。 So again。

 even attacker with kernel mode readr privileges in Vtl 0。

 They can mark the page table entry ridriable， but it's not actually writeriable because the hypervisor is responsible for setting the read only S entry。

And then additionally， the associated bit states are updated in the new allocation to say。

 here are the indirect call targets that I care about tracking。

And in this case I'm using the source point JTag debugger in order to debug the secure kernel and we can see a hyperca is happening in the screenshot here with the mask of GPA。

 which is guest physical address readable， thus marking the read only St entry。

That's not the only case we need to care about the bitmap。 So consider， for example。

 and a call to getproc address and user mode。 you provide a string and the exported function address is returned back to you。

 Well， presumably you're going to call that memory。 Well。

 that's going to be invoked since it's through a function pointer as an indirect call。

 and indirect calls are checked by CFG。 Well that call。

 that call target which you resolved through getproc address was not known at compile time。

 So it's not a valid call target。 So the point here is that the kernel mode implementation of getproc address is called M gett system routine。

 And if you call this， the call target is automatically marked as valid in the bitmap。

 which is something interesting。I haven't talked much about exploitation at this point。

 but one of the known weaknesses of CFG is the fact that it is a coar grainined CFI mitigation。

 What that means is CFG does not actually validate the call target you're invoking is the developer intended call target。

 What it does is it takes the address of the call target looks it up in the bitmap。

 And if it's valid， it jumps for execution。Well， since the entire CFG bitmap is shared by all the kernel mode images。

 nothing is stopping us， from example， overriding a call target in win 32K as part of an indirect call with another valid call target from NT or N TFS or somewhere else。

 So obviously you can still call into other functions。Well。

 Microsoft knew about this from the beginning， and they eventually came to the solution of extended control flowguard or X。

 FG。And XFG works at compile time， it creates a hash of the functions prototype。

 so a number of parameters parameter types， return value and return type。

That information should theoretically be unique to a given function。

 and that is placed above every call target， and then when an indirect call happens。

 we take the incope XFG hash and we compare that with the expected hash， and if they mismatch。

 we have a CFG violation。Well， unfortunately， although this takes everything being a valid call target to only developer intended targets。

 this was deprecated although it did have user mode and kernel mode support。

 and so it's no longer in use。So where does this leave Colonel CFG？Well， there's no X FG。

 and it works just like normal CF FG。 Some of the mechanics are slightly different because of the fact that there is a new feature in windows called hot patching。

 And what this means is instead of invoking the dispatch function through an indirect call。

 It's actually fixed up to a new dispatch function， which is highlighted in red here。

 and it's made through a direct call。 there's a little more nuance to it。 But for our purposes。

 this is worth calling out。😊，Another interesting note is that kernel CFG acts as a software SM or supervisor mode execution prevention。

 so back in the day what kernel mode attackers would do is allocate some user mode memory。

 use a readwrite primitive in the kernel to corrupt a function pointer with that user mode memory。

 and then coerce the kernel to actually call that corrupted function pointer。

 which redirects execution into user mode。The only problem here is the processor is still running in context of the kernel。

 so the user mode memory is executed with chron mode privileges。

This was a situation of the mitigation itself was implemented in the same privilege boundary as an attacker。

 The page table entry has a special user supervisor bit， which lets us know what's a user page。

 What's a supervisor page。 So K C of G， even with HVC I disabled， will be a full no operation。

 except for one single case。 and that is a bit wise test on the address。

 And if it's a user mode address， we still crash the machine， even without HVC I being enabled。😊。

And one of the other things that attackers know is that the imports address table is explicitly called out in the documentation for CFG as not being protected。

So in this case， we have a kernel mode exploit again， and we're running a user mode。

 We have a kernel mode readrite primitive。 We locate the imports table associated with driver dot cis。

 And in this case， we corrupt the PTE to market as ridable。 Now you may be thinking。

 well HVC I should be there。 How can you corrupt these page tables Again。

 HVC I is looking for code becoming data or data becoming code。

 The imports table is a data of region of memory that is simply becoming ridriable。

 So HVC I does not care about this。 we corrupt the imports entry with ro gadget。

 And then we coerce the kernel to invoke this import。 So in this case， we call X allocate pool 2。

 which will do a memory fetch to the I and pick up what it thinks is x allocate pool 2。

 but is actually ro gadget。 And thus we invoke ro gadget， even with CG enabled。😊。

So you can actually combine kernel CFG with a lesser known mitigation that's not directly related to it called Repoline and Repoline aim to mitigate a CPU branch prediction vulnerability called Specter type 2。

 Well newer CPUs can defend against this。 And so reppaline it isn't really used or talked about much anymore。

 But one of the things that reppoline did for our intents and purposes is obviously it does a lot。

 but it actually patches or fixes up， I should say all of the indirect calls to the imports table and makes them direct calls。

 and it makes those direct calls to a special reppoline dispatch function。But in 99% of cases。

 an additional feature called imports optimization actually results in this just calling the imports table directly。

 We can see this in the screenshot here。 In this case， the call from N TFS is going to N T Z W close。

 but it's not being invoked indirectly。 it's made through a relative call or excuse me。

 a direct call And even though newer Cs don't use Re pullinging。

 all of the Windows images are still compiled for Repoline support。

 So you get the imports being fixed up to direct。 And one of the interesting things that I like to call out is how this was implemented。

 there's a feature called dynamic value relocation table or DV Rt back in the day。

 I think it was Windows 101607， the static addresses for the paging structures like the PFN database。

 the P TE database were finally randomized And the way that this was achieved was through this special feature that was created with support from security and compiler teams that you can actually relocate a。

😊，static value to some other value， which allowed the randomization to occur。

 Red pullingll used an extension of DVRT， which allowed all of this fixing up to occur。

 which I think is pretty interesting。So what this means for us is that we no longer have to read from the imports table。

 So in this case， I wrote a driver for the purposes of this conference talk。

 and it's called not V driver。 And there's one single call that's made to ex allocatecate pool 2 through the imports table。

But you can see in the debugger in the R 10 register。

 I've already corrupted this entry with an arbitrary read write primitive to 41，41，41，41。

If we did not have reppoline support， we would have simply called 41，41，41。

41 because this is what was in the imports entry and we would have crashed。 But in this case。

 you can notice there is no crash， and we have a valid pool chunk in the return value。

 This is because， again， we did not have to do a memory fetch to the imports table。 Thus。

 whatever is in the imports table at the time the image is loaded is what remains。

 no matter if an attacker corrupts it。 So the point in calling this out is one of the known limitations of kernel CFG is protected by repplline。

Obviously， attackers， they can do things like overr return addresses。 So in this case。

 I have a thread。 This thread is in a suspended state。

 which effectively means there's an asynchronous procedure caller A PC queued to it。

 telling this thread， do nothing。I then use a kernel mode readwrite primitive to corrupt one of the return addresses on the stack with a simple ro gadget。

 In this case， it just breaks into the debugger for demo purposes。When this thread resumes。

 the stack will unwind， and my ro gadget is obviously executed because in the red box here。

 we've broken into the debugger。 This is outlined first by a technique called Colonel Forge。

 which kind of pioneered this。 so we can obviously still overwrite return addresses on the stack。

Well， this is exactly where kernel mode C E T comes in， as I'm sure you could have guessed。

 And so return addresses are now protected。And kernel CET is a little bit different than kernel mode CFG。

 and that it's not binary。 CFG is binary， It's on or it's off。 You need to compile， obviously。

 for support。But with C， E T， there is something called audit mode where you can effectively put your machine in audit mode。

 And when a control flow protection fault occurs instead of just the interrupt handler bug checking。

 it fixes up the shadow stack or other things that's needed。 And most importantly。

 it emits an event tracing for Windows or E T W event that you can get more information about why this C E T violation occurred。

Again， as I mentioned in the last slide， we also need HVCI。

So the region of memory associated where all the kernel mode shadow stacks will reside is still managed by N T。

 So there are situations where we have kernel mode shadow stacks like interrupt service routines or DPCs。

 But the most canonical example， I guess you could say。

 is a thread being created Anytime you have a thread being created， you have an associated stack。

 Well guess what， if you have an associated stack， you now have an associated kernel mode shadow stack。

So the common example is a threat is created， Newtack is created， then we need to create the shadowt。

 we make an inline call to the secure kernel through the secure system call interface that enlightens the secure kernel。

 hey， here's where we want this new shadowowt allocation to Re and please mark as read only。

 but then additionally a special bit which we'll talk about later called supervisorvisor shadowdowst bit。

 which is part of an Intel feature called Supervisor shadowdowt control。As I mentioned。

 every thread creation needs a shadow stack effectively。Well。

 if every thread creation needs a shadow stack， that means every thread creation now results in a call to the secure kernel。

 which is going to do what， do a hyper call and incur in VM exit。

 We don't really want to do that all the time。 if we don't have to。

 So there are two caches that are managed by N T and rest assured all of the members of this cache have gone through the slow path once。

 So all they are all read only in the slack table for V T L 0。

And we have two kinds of caches a per processor cache and a pernemenode cache。

 Aneumminode is effectively a grouping of processors associated with a range of memory is a very gross oversimplification。

And so what happens is if the conditions are right， we send old shadow stacks。

 which we are done with to one of these caches。 And the reason for this being is threads typically have an ideal processor or。

Idealneumminode。 Well， allocations typically will be cached on the processor。

 which they were issued from or executed from。 Well。

 the the idea here is that if we can cache these shadowts on a processor orneumminode。

 we may be able to gain performance in the sense of when we go to get a new shadow stack。

 that memory may already be cached on a given processor depending on the conditions。

 So we have two caches， but there is a slow path and that goes to the secure kernel。

So no matter what happens if we go through the cache or we go through the slow path。

 the K threadread object， which is managed by the kernel for a thread。

 is updated with a new field called the kernel shadowowt。

 It contains the address and also some metadata， as we can see here， documented types。

 We have user thread and kernel thread are the most notable。 you may be thinking。

 why does a user thread need a kernel mode shadow stack。

 Well the architecture on Windows when you do something like a system call as an example and you transition an execution into kernel mode。

 It's issued on the same thread which issued the system call。

 And so we don't want to use the stack in user mode to store all this local kernel data。

 So we switch into a kernel stack。 if you have a kernel stack， you now have a kernel shadow stack。

And additionally， just for completeness sake， there is something called shadowst owner data。

 it's really used for debugging purposes， and the MMPFN。

 the PFN structure associated with the region of memory of the shadowst。

 contains effectively a masked off address of the Kth object which owns the shadowt。

But before we get to that， that's the final stage。 the secure kernel has to do a few things。 One。

 as we know market is read only and two， it configures what's called a shadowst token。

 So a token is used in certain circumstances to validate that a shadowt is effectively legitimate。

 And there's also an example of how to switch shadowsts for supervisor shadowsts documented by Intel。

 You have what's called a restore token。 The secure kernels responsible for configuring this。

 And remember， the shadowst is read only in Vtl0。 so it cannot be modified。

 the secure kernels delegated the permission to do this。And on a context switch。

 special CPU instructions are used when the thread of the new shadowd stackack is scheduled on the CPU to update the new shadowdst pointer register value and also save the old value for the old thread leaving the processor。

Thus， when this old thread is scheduled again， we know where to pull the old shadowst value from。

 so that's kind of the canonical example of shadowt switching for supervisor shadowts。

And it's also responsible for initializing the return address。What I think is one of the， I said。

 main engineering hurdles。 This is just my opinion， is rights to the shadowt。

 So this is an interesting thing because。Putting Slat and hypervisor aside for a second。

 It's very easy to disallow ordinary data rights。 So a move operation to a shadowt。

 All we have to do is know where the shadowsts are。 So there's four paging structures。

 typically on Windows。 There's something called on Intel processors There something called LA 57。

 which is coming in the future to make that 5。 But for now， we basically have four。

 And these are made up of page table entries。 Intel documents special PTE bits set for all of the paging structures used to translate the shadowst marking all of them is ridable。

 except the final PE that maps the physical page。 It's marked as read only and marked as dirty。

 That's a special set of bits that we should not find anywhere else。 And this we know， okay。

 this is a shadowst page。😊，But what about with Slat because we're using CET with the hypervisor and the hypervisor means we're not using the PTs。

 We're using the extended PTs。 So number one， how does the hypervisor know what the shadowst pages are。

 But number two， if a call pushes a return address onto the stack and a return address onto the stack goes on a shadow stack that's a read only that's going to cause an Ept violation because that's a read only region of memory。

 What's that going to do， that's going to do a Vm exit。

 we don't want to do a Vm exit every single time a call happens in kernel mode。

 So we use what's called the supervisoror shadowt control feature to help with this。

So there's a special supervisor Shadowtack bit which I talked about set in the EPTEs for VTL0。

 so NT lets Secure Colel know， hey， this is a shadowowst region of memory。

 and then that is passed on to the hypervisor and that's where the read only entry。

 the read only permissions and supervisoror shadowowt permissions are set。So same thing here。

 the extended page tables all have page table entries with control bits。

 All of them are marked as readwrite， except the final extended page table entry is marked as read only and supervisor shadowst that。

 This is how we know this is a supervisor shadowt， Thus we can disallow ordinary data rights。

 but not have to incur the cost of a VM exit every single time a call happens because we know， hey。

 this is a shadowt region of memory。And another security benefit we have here is that shadowst accesses are not allowed to occur to a nonshadst page。

 so an attacker could theoretically take a shadowst region of memory。

 remap it to a non-shadst that's ridable and forge their own malicious shadowst。

 Well shadowst accesses cannot happen like that because this supervisor shadowst control feature says accesses can only happen to pages marked as supervisor shadowst。

So we also gain that。 And as far as I'm aware， Windows is the only platform that actually leverages us today。

So there are other things， though， that kernel C ET needs to be aware of an exception is an example。

 So we're executing code and kernel mode and exception happens。 Well。

 we have to go dispatch the exception handlers。 The exception handlers do their thing。

 and then we have to jump all the way back to where execution was before。 Well。

 the way that that works is there's a special what's called context record and exception records that are used to figure out。

 okay， here's where we need to go back in execution。 Well。

 now part of the context structure is the shadow stack。 So if this was handled by V T L 0 exceptions。

 What's stopping an attacker from being able to do coerce。😊，The exception handlers to say。

 actually update the shadowt pointer to this value。

 So what we actually do is we delegate updating the shadow stack value from exception to the secure kernel。

 So the way that this works is there special Mrs or model specific registers that are used for context switch。

 privilege context switch， context privilege switch。

 So when a processor is running in current privilege level0， which is kernel mode and goes to 3。

 which is user mode， There's a special MSR where it can pick up。

 Here's the intended shadowt value for the new user shadow stack that I need to maintain。 Well。

 we also have that same functionality for guest to hypervisor。 So when an exception happens。

 we go in line to the secure kernel through a mechanism called。😊，Shadowt assist。

 And we basically say we need to update the shadow stack to this value。

 What happens is there's a special virtual processor register associated with the virtual machine control structure for V T L 0。

 And the VMC S is configured in a way that when VM entry occurs。

 that's the context switch back for the processor executing in context of V T L 0。

 it will pull the new shadowt value from this entry in VMC S。 And so for all intents and purposes。

 only the secure kernel can supply the new shadow stack value for an exception is one of the things that we have to be aware of。

😊，But also， we have opportunistic checks。 So the secure kernel knows about all the executable code in V T。

 L 0 through those naR structures I mentioned earlier。

And exceptions are associated with a new context record。 So the secure kernel goes。

 We're already here。 We're already updating the shadow stackg value。

 Why not take the instruction pointer from the context record and validate It resides within a known executable region of memory。

 So that's another opportunistic check， which is done。😊。

Another interesting note here is that the CPU is going to issue a special interrupt when the control flow protection fault is incurred。

 but that is' not all she wrote， because what happens here is that on this interrupt handler being invoked。

 which is a thing which is handled by V Tl0， the the stack value which caused the C ET violation。

 is preserved and we actually loop over the entire shadowt。 And if the stack value。

 which caused the violation is found anywhere on the shadowt。

 we use our ci functionality to fix up the shadowt and execution can continue。

 So you can still overr return address is on the stack as long as the address you're overriding。

 is also found on the shadow stack still， which is a very interesting note。

So what are some of the conclusions here。 Well， kernel C E T。

 it's only been around since Windows 1122 H2。 and it is not enabled by default。

 And so what that means is research surrounding this mitigation。

 first of all requires specific hardware， but also requires you know later versions of Windows。

 So most of the research which exists today surrounds actually taking advantage of known weaknesses in forwards edge CFI。

 So taking advantage of kernel CG。 again， we have the entire bitmap of all valid call targets that we can choose as an example。

 We also have other things like counterfeit object oriented programming， jump oriented programming。

 where we can still get around kernel CG。 So that's where most of the research exists。

 and that is because Windows does not leverage indirect branch tracking。

 which is the other feature of CET， which validates indirect calls and jumps。

 IBT would mitigate against this， but Windows does not use it。Obviously。

 kernel C ET seems to be the stronger of the two mitigations。 It's hardware enforced as well。

 I think that out of context returns could be something that's interesting as well。

 I've seen it maybe talked about before， but no real public POCs and whatnot。

 So I think that could be an interesting vector for research。

 but you can obviously still do remapping as I talked about earlier。

 we have the supervisoror shadowst control feature which can mitigate against this for shadowsts but what's stopping an attacker as an example for taking a region of memory that's KdP protected and remapping it to a writeriable page and writing to it。

 Well we actually have a mitigation now called hypervisor linear address translation or Hla。

 which is used through the hypervisor and special CPUs that is available now in 24 H2 of Windows that can prevent this remapping。

 which I've just mentioned。And so I just think that the presence of all these mitigations。

 when they're all enabled by default， obviously raises the bars for attackers。

 Rob is completely mitigated effectively with CET。 The interrupt Tr stuff is obviously a Windows specific implementation thing。

 so it'll be fun to see the cat and mouseuse game which ensues in the future。

 once more companies adopt the newer hardware and CET is fully enabled。 HVCI is fully enabled。

 all of these mitigations enabled in tandem。And so with that， I thank you for coming to my talk。

 Thank you to the folks in this slide here for answering a lot of my questions。

 Here are some additional resources。 I don't think I'm going to have more than a minute for questions。

 but I will be in the wrap room afterwards if folks would like to ask anything there。

 So thank you for coming to my talk。 and thank you to Black Ha for having me。

