# 【转载】Black Hat USA 2022 会议视频 - P20：021 - Blasting Event-Driven Cornucopia： WMI-based User-Space Attacks Blind SIEMs - 坤坤武特 - BV1WK41167dt

 [MUSIC]。

![](img/1a017bc136190fbb202c3486a49d0dc4_1.png)

 >> Good morning， Black Hat。 My name is Kao Yutoda-resco and I'll be presenting。

 the blasting event driven cornucopia representing the binary team。

 Windows management instrumentation provides， the ability to manage enterprise devices both locally and remotely。



![](img/1a017bc136190fbb202c3486a49d0dc4_3.png)

 Various tools， EDRs and seeing solutions leverage。

 this functionality for performance monitoring and device telemetry gathering。

 Attacking this functionality can blind all these solutions that rely on this telemetry。 Also。

 this is an important topic for， defenders for experts in Windows security and malware detection and incident response。

 A little bit about the binary team。 So binary is a device security startup based in LA that focuses on。

 threats that originate below the operating system in the former and。

 understand how they move up the stack into the operating system。

 And we use our land to deploy their next level， their next stages in 2021 at Black Hat Europe。

 The same team presented a deep dive on attacks on ETW。 For this talk。

 we chose to attack WMI and as we'll see through the presentation， we have a lot to talk about。

 Again， my name is Kao Yutoda-resco。 I'm the CTO and co-founder of Binerly。 Unfortunately， Andrei。

 Red Plate and Igor couldn't make the trip to be with us today。

 but their contribution to this research was essential。

 So it was very important to understand how we can attack WMI because it is used by a lot of solutions in the field。

 And these solutions can be blind or it can be tricked in terms of the telemetry that they are getting。

 Also， more important is to find ways how to detect if the WMI or any other solution that is used by security solutions is tempered with。

 And then act upon that by notifying the proper audience。 So as for the agenda。

 we'll start with presenting some information about the Windows management information in terms of architecture and features。

 Also， we'll look at how the hackers and the threat actors are using WMI in the world。 Next。

 we'll go to the core of the presentation by introducing different types of attacks on WMI and also introduce eight new brand attacks on WMI。

 At the end， we'll introduce some solutions that can。

 some solutions like WMI check and memory ranger。 Those solutions can detect and prevent some of the attacks that will be presented in this presentation。

 Next， let's go a little bit into the Windows management instrumentation architecture。 For short。

 WMI actually is the Windows implementation of two standards。

 the WBEM or web-based enterprise management and CIM common information model。

 It has very good advantages。 It's available system-wide in all the Windows operating systems starting with Windows NT 4。

0。 Also， it provides a standardized framework to talk with consumers and providers via common interfaces。

 going into the specifics of the architecture。 So we have the WMI providers that produce device telemetry as WMI objects。



![](img/1a017bc136190fbb202c3486a49d0dc4_5.png)

 We have the clients that consume those events。 We have the CIM standard that is structuring。

 querying and transmitting the WMI objects。 For reference。

 WMI objects are actually instances of WMI classes。 Next， so as part of the CIM standard。

 we have the WMI repository that stores the class definitions， the namesfinity definitions。

 and their associated properties， as well as the persistent WMI objects。

 Then we have the MOF or managed object format。 It's an object-oriented language that is used to specify different WMI artifacts to extend the format。



![](img/1a017bc136190fbb202c3486a49d0dc4_7.png)

 Then we have the query language that is a SQL-like type language， WQL， to filter events。

 To remotely connect and transmit or receive data， Windows remote management or DCOM can be leveraged。

 Last but not the list， we have the WMI service， which is implemented at the SVC host service DLL in the NetSVCS group。

 which is called WinM GMT。 Let's talk a little bit about the WMI providers。 In user mode。

 they are implemented as code-comb-based DLLs or as kernel drivers for the kernel component。

 As you can see in the MOF definition of a provider。

 a provider is an instance of the underscore underscore Windows 32 provider standard class。

 It is identified by a class ID and as any comm interface， it has its own registry configuration。

 As you can see in Windows 11， we have more than 4。

000 built-in WMI providers that can cover a lot of different device components。

 Some of them are listed in the table。 WMI events offers a great ability for both attackers and defenders to act on pretty much every WMI event。

 which covers mostly any of the operating system events。 Also。

 another great ability offered by WMI is to register permanent event subscriptions。 This is very。

 which obviously will survive very good， which is very used in the field by attackers to tamper systems on those systems。

 There are two types of events， intrinsic events and extrinsic events。

 I'll leave the definition to this on reference。 Next， we move to filters。

 It's an instance of underscore underscore event filter WMI class that specify which events are delivered to the bound consumer。



![](img/1a017bc136190fbb202c3486a49d0dc4_9.png)

 We'll see what bounding means。 The most important properties of this class are the event namespace。

 the query language to filter the events， and also the query itself expresses a WQL query。

 These are the examples of the general syntax of a WQL query。 And also we have two examples。

 one for intrinsic event query， which actually triggers every time a notepad。exe is launched。

 On the other one， it's an example of extrinsic event filter。

 which detects persistence in the run registry key。 Next， the consumer， the WMI consumer。

 defies the action to be carried out once a bound filter event filter has triggered。 In the standard。

 there are available five event consumers to perform actions such as logging。

 executing scripts or command lines， or even sending notifications。 And in terms of。

 we talked about the permanent event subscription， how is that performed？

 It's the way persistence and code execution happens in the WMI repository。 It's in three steps。

 Create a filter to describe the event to trigger on。

 Create a consumer that describes the action once the filter has triggered。



![](img/1a017bc136190fbb202c3486a49d0dc4_11.png)

 and then bind them using a filter to consumer binding instance to link pretty much the trigger to the action to be performed。

 So this is a simple example how it can detect when a new service is installed。

 You create an event filter that monitors for instance creation events at the polling interval。

 for which the target instance is of type 132 service。

 Then you create a consumer that will trigger when the new service is created and write a notification into the event log using the event viewer consumer。

 And last but not least， you bind the filter with the consumer to link pretty much the trigger to the action to be performed。

 Let's talk a little bit about the CIM repository。 It's the database for the WMI that stores the class definitions for the class definition。



![](img/1a017bc136190fbb202c3486a49d0dc4_13.png)

 and the enhancement definitions together with their qualifiers and properties as well as the persistent objects。

 WMI objects consists of three types of files。 It's the BTR object data and the 3 mapping files。

 The index that BTR contains the index for the WMI， which is implemented as a B3。



![](img/1a017bc136190fbb202c3486a49d0dc4_15.png)

 organized in index records stored in pages。 The object that data contains the actual class definitions and the persistent objects。

 In the same way it is organized in records and those records are stored in pages。

 And then the mapping has only one purpose to translate the logical page numbers to their physical page number equivalent for both the object data and index that BTR。

 When I was part of the FLIR team in FireEye， I reversed the format of WMI and wrote a paper and released several forensic tools with my good friends Willie and Matt。

 Let's go deeper into the forensics， how it works in simple terms。

 Pretty much the search path is constructed by using the identifier of the namespace。

 class and instance。

![](img/1a017bc136190fbb202c3486a49d0dc4_17.png)

 The index that BTR is searched and then the corresponding index record is found。

 which contains the logical page number in the object data for the record that you are looking for。

 The record and identifier are in its size。 Then the mapping file is used to translate the logical page number into its physical equivalent。

 Then now we have the physical page where the data you are looking for is in object data。

 You go through that page record by record and identify the one that you are looking for by looking at the record ID。

 Of course the size in the index record should match the actual size in the objects that data。

 Also the same approach can be used for index that BTR because it's a bit between implementation and the pointers to the next nodes in the B3 are specified as logical page numbers。



![](img/1a017bc136190fbb202c3486a49d0dc4_19.png)

 Now that we know how to parse the repository， we did a quick exercise to figure out what it's in actual database for a Lenovo machine。

 So we saw that in the root WMI namespace we have very interesting classes starting with Lenovo Underscore as prefix。

 One of them is a BIOSetting， another one is Lenovo set BIOS password and in the right we have the complete definition with properties in it。

 For which current setting pick our attention is a string and if we are trying to look in the repository for instances of this class。

 we have empty results which means this class is dynamically created on the fly by a custom provider that is implemented by the OEM vendor。



![](img/1a017bc136190fbb202c3486a49d0dc4_21.png)

 So we can use WMI to actually look for non-empty instances of this class and then printing out the current settings we have these results。

 If we magnify a little bit we see some information about the trusted execution。

 the TPM and also about updating the BIOS。 Which means that having those classes you can alter the settings of the BIOS from the user land。



![](img/1a017bc136190fbb202c3486a49d0dc4_23.png)

 Remember that there is a Lenovo Underscore set BIOSetting。

 So which means Lenovo provides an interface in the in the firmware so the WMI provider can communicate to read or alter the settings。

 Which from my point of view as a device security startup is pretty scary。

 Also the standard provides a class with 32 BIOS to give us information about the current BIOS version。

 It's serial the device serial number and also the OEM vendor。

 So now let's look at how WMI is leveraged by both defenders and attackers。

 For defenders it's a great tool to use to gather telemetry。

 For attackers it's a great living of the land infrastructure to perform malicious activities。

 So how the threat actors leverage WMI， reconnaissance AV detection and so on。

 In the next slides I'll present an example of how in the world WMI is leveraged for。

 wireless persistence called execution and data storage。

 So pretty much we have a trigger or event filter that triggers several seconds after the system power up to allow the device to boot up。



![](img/1a017bc136190fbb202c3486a49d0dc4_25.png)

 Which will trigger this trigger will make the command line event consumer to execute。

 What it will execute is a PowerShell script located in the description property of the option class。

 So we have the definition of the option class which has a name and a description both of them strings。

 In the description that is called by the consumer we have the script。

 If we look at the script very quick we see that it extracts the。

 basically the code it payloads from the name property and the code it drops it to disk。

 the path specified in the configuration and then it executes the second stage。

 So the tools that we use in our research the WMI test to showcase the WMI interaction scripting via VB script。

 and PowerShell to play with filters and consumers also developed our own WMI client to look for running processes。



![](img/1a017bc136190fbb202c3486a49d0dc4_27.png)

 So WMI attacks now let's switch to the main topic。

 The Tread model of WMI consists of the WMI service that connects and interacts that。

 communicates with consumers clients and providers via advanced LPC ports LPC channels。



![](img/1a017bc136190fbb202c3486a49d0dc4_29.png)

 Then we have the WMI files the repository and the provider DLS and so on。

 We have the configuration and registry and also the data in the WMI service。

 And then we split each one in each of the attacks in five buckets depending。

 which are the targets the WMI data process the LAPC connections the files and。

 registries then the sandboxing of a WMI using user mode attack and then sandboxing of WMI using a DCOM attack。

 So why the WMI attacks are so dangerous so these attacks have existed in the。



![](img/1a017bc136190fbb202c3486a49d0dc4_31.png)

 WMI from the beginning and the reason for that is WMI wasn't built for to be used for a security。

 solution。 WMI was built just to gather calamity on the device also the WMI service is not considered a。

 critical app it doesn't have PPL or a trust label and the solution the security solutions。

 don't have a way to check if the WMI has been disabled or has been tampered with。

 So that on top of that all these attacks are architectural flaws which cannot be just fixed with a。

 simple patch。 So let's start with the first bucket which is attacks on the files and。

 registry configuration so you can modify content you can remove files you can restrict access to。



![](img/1a017bc136190fbb202c3486a49d0dc4_33.png)

 the files on disks like the files that represent the database or the provider DLLs in the registry。

 we have the service configuration and the other WMI internal settings。

 So let's the first attack there is a registry key called enable events when a new event。

 WMI client comes in that's the following call stack on Eenit。es which stands for。

 initialize events of system is called inside the function the enable events registry value is read。

 if it's different from one then an pointer to the interface is obtained and success is returned。

 if so an attacker who basically can write number different from one in the in the registry if value。

 restart the WMI service and then everything is disabled。

 So the WMI infrastructure in the user space the most important is the service。

 the service the main service is WMI SVC is the WMI SVC DLL it's implemented as a SVC host service DLL。

 and the other the DLLs that are important for our discussion the WBM core the RBRVFS and WBMMS。

 So let's look at the template how this is attacked。



![](img/1a017bc136190fbb202c3486a49d0dc4_35.png)

 So we have the code of a certain DLL on the right we have the running memory so when the DLL is loaded。

 the global flag is initialized to it default value in this case zero and the initialized function。



![](img/1a017bc136190fbb202c3486a49d0dc4_37.png)

 this flag is turned to one when a new connection or registration of an event filter comes in。

 the global flag is checked if it's true or one then the event is processed the event is processed。

 and success is returned we have a new connection established and the registration successful。

 Then an attacker can clear the global flag so the error path in the dispatch routine is taken。

 the event is dropped and then the error is returned。

 So we'll talk about all these flags so different， flags in different DLLs can be attacked the only difference is the error returns after the attack。

 when you we disable WMI。 So we'll start with the global boolean variable do not allow new connections。

 when the WMA core DLL is loaded then it is set to false and when it is unloaded in the shutdown。

 method it is set to true when a new WMI connection comes in ensuring it initializes finally called。

 and when it's recalled if it's false then the initial the system is initialized and。

 everything works correctly if an attacker comes in and turn this value to true then the insurance。

 initializes will return an error which is called server stopping so now is the time for the first demo。

 You can call the people of the community and find out the name of your friend。



![](img/1a017bc136190fbb202c3486a49d0dc4_39.png)

 [inaudible]。

![](img/1a017bc136190fbb202c3486a49d0dc4_41.png)

 [inaudible]。

![](img/1a017bc136190fbb202c3486a49d0dc4_43.png)

![](img/1a017bc136190fbb202c3486a49d0dc4_44.png)

 [inaudible]。

![](img/1a017bc136190fbb202c3486a49d0dc4_46.png)

 [inaudible]。

![](img/1a017bc136190fbb202c3486a49d0dc4_48.png)

![](img/1a017bc136190fbb202c3486a49d0dc4_49.png)

 [inaudible]。

![](img/1a017bc136190fbb202c3486a49d0dc4_51.png)

![](img/1a017bc136190fbb202c3486a49d0dc4_52.png)

 [inaudible]。

![](img/1a017bc136190fbb202c3486a49d0dc4_54.png)

 [inaudible]， [inaudible]。

![](img/1a017bc136190fbb202c3486a49d0dc4_56.png)

 Thank you。 So let's introduce WMI check， which is a tool developed by Andre Redplate。



![](img/1a017bc136190fbb202c3486a49d0dc4_58.png)

 Consistre of a console app and a kernel driver。 It can scan the process and parse the WMI internal objects。

 You can do that for a specific process and also for the whole system。

 On the right side we have all the options that are supported。

 On the left side is a screenshot of how WMI check can detect WMI data patching。

 So you have a snapshot before and after the attack comparing the snapshot you see how the internal flags are modified between different instances。

 So next demo showing how to detect the previous attack using WMI check。



![](img/1a017bc136190fbb202c3486a49d0dc4_60.png)

![](img/1a017bc136190fbb202c3486a49d0dc4_61.png)

 [inaudible]。

![](img/1a017bc136190fbb202c3486a49d0dc4_63.png)

![](img/1a017bc136190fbb202c3486a49d0dc4_64.png)

 [inaudible]。

![](img/1a017bc136190fbb202c3486a49d0dc4_66.png)

![](img/1a017bc136190fbb202c3486a49d0dc4_67.png)

 [inaudible]， Next， let's start with the next attack on WMI event delivery。 This is an internal flag。

 The offset C in the EAC session field of C repository class。

 Pretty much when the DLL is loaded the event delivery is set to its default value， which is false。

 In start event delivery is set to true。 In stop event delivery it is sent to false。

 Same attack pattern。 You have an intrinsic event coming in。



![](img/1a017bc136190fbb202c3486a49d0dc4_69.png)

 You have the delivery intrinsic event function。 An attacker will come in and patch this flag to false so that the WMI is now error is sent。

 That's actually an error if you use failed on this error it returns true。

 So what happened all the intrinsic events are disabled。 Let's see another demo about how this works。



![](img/1a017bc136190fbb202c3486a49d0dc4_71.png)

![](img/1a017bc136190fbb202c3486a49d0dc4_72.png)

![](img/1a017bc136190fbb202c3486a49d0dc4_73.png)

 [inaudible]。

![](img/1a017bc136190fbb202c3486a49d0dc4_75.png)

![](img/1a017bc136190fbb202c3486a49d0dc4_76.png)

 [inaudible]。

![](img/1a017bc136190fbb202c3486a49d0dc4_78.png)

![](img/1a017bc136190fbb202c3486a49d0dc4_79.png)

 [inaudible]。

![](img/1a017bc136190fbb202c3486a49d0dc4_81.png)

![](img/1a017bc136190fbb202c3486a49d0dc4_82.png)

 [inaudible]。

![](img/1a017bc136190fbb202c3486a49d0dc4_84.png)

 [inaudible]， [inaudible]， [inaudible]。

![](img/1a017bc136190fbb202c3486a49d0dc4_86.png)

 [inaudible]， [inaudible]， [inaudible]， [inaudible]， [inaudible]， [inaudible]， [inaudible]。

 [inaudible]， [inaudible]， [inaudible]， [inaudible]， [inaudible]， [inaudible]， [inaudible]。

 [inaudible]， [inaudible]， [inaudible]， [inaudible]。



![](img/1a017bc136190fbb202c3486a49d0dc4_88.png)

 [inaudible]， [inaudible]， [inaudible]， [inaudible]， [inaudible]， [inaudible]。



![](img/1a017bc136190fbb202c3486a49d0dc4_90.png)

 (upbeat music)， (upbeat music)。