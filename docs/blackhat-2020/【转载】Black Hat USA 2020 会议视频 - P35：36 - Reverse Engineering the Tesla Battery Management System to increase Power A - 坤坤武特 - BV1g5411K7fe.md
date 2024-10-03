# 【转载】Black Hat USA 2020 会议视频 - P35：36 - Reverse Engineering the Tesla Battery Management System to increase Power A - 坤坤武特 - BV1g5411K7fe

 Hi there。

![](img/9345a7e7dac034a10438e5588fb2a101_1.png)

![](img/9345a7e7dac034a10438e5588fb2a101_2.png)

 Welcome to my talk， Reverse Engineering the Tesla Battery Management System to Increase。

 Power Available。 This was a culmination of some research that I did over the last year。

 It was definitely one of the most complicated reverse engineering things I've ever done。

 But it was also pretty rewarding because it actually uncovered a real world effect。

 I was able to make a Tesla Model S faster in the end。

 And coming from a car guy who's been tuning cars pretty much his entire adult life， it。

 was a fun combination of actually hacking and then wrenching on cars to get the desired。

 effect out of this work。 So I hope you enjoyed as much as I enjoyed doing the work and the research。

 So a little bit about me。 My name is Patrick Kylie。 I'm Principal Security Consultant at Rapid7。

 a member of the Penetration Testing Team。 I've done previous research in Adionic Security。

 Obviously， the Tesla I've researched some Internet， Chemical and Transportation platforms also。

 have some experience in hardware hacking， CAN bus， etc。

 Some topics that we're going to cover on this is just a little outline。

 First the architecture of the Model S and the battery management system。

 The battery management system is the first gatekeeper for how fast the vehicle can go。

 the other units of the drive and burgers。 So performance in ludicrous timeline。

 you'll see what's important in a minute。 The hardware changes that actually had to happen to the car to make it go faster。

 some， components that had to be swapped out。 Some data that's actually stored in Toolbox。

 Tesla's Diagnostic Application and how I was， able to extract that data and use that to figure out the process。

 Some firmware changes that had to occur to the car in order to actually revise the battery。

 management system， upgrade the amount of current that it would allow going through it。

 Modification to the shunt。 The shunt is the component in the battery management and the battery itself that actually。

 measures the amount of current flowing。 It's a unique component and it actually had to be modified in order to allow this upgrade。

 to go through。 The upgrade process itself， what the actual steps are。 An expensive bricking。

 I managed to actually brick the car and I had to tow it across state。

 lines to order to finish the process。 But in the end I actually learned something cool and I'll share what that was。

 Then some next steps if someone wants to help me work on this and take it further， I'm continuing。

 to work on this and I would appreciate any help。 So here's a brief overview of the architecture of the Model S。

 The central point that when， someone's hacking the car that they connect to is the CID。

 the Central Information Display。 On the vehicle that was the donor car for this research is an NVIDIA Tegra base。

 the newer， ones are Intel based platforms including the Model 3 and the newer ones。

 The gateway which is a security component that actually sits between the CID and the。

 various CAN buses of the car。 Then specifically focusing in on a powertrain CAN bus。

 the powertrain CAN bus contains the， BMS， contains the drive inverters。

 charging units and other elements that actually control， the powertrain。

 It's a standardised CAN bus so it runs at 500 kilobits a second， uses differential signaling。

 two wires， 11-bit R/D's pretty much like any other CAN bus that you would see out there。

 And then that CAN bus actually supports the EDS standard we'll actually see later why that's。

 important。 So drilling in on the battery management system or BMS itself。

 this is a picture of the BMS， that you see over here on the right。

 The main microprocessor is a TMS 320C 2809。 It's a digital analog converter microprocessor made by Texas Instruments。

 There's also an Altara CPLD that's basically a hardware backup for the TMS 320 in case。

 there are any software flaws。 It looks like it was engineered enough such that the TMS 320 would have a backup in the。

 Altara in case there were any kind of software flaws that prevented any hazardous conditions。

 from existing from the battery。 There's also a current shunt that has an STM8 that measures current coming from the battery。

 and feeds it directly into the BMS board。 There's a pre-charge resistor which when the contactors from the BMS close and allow current。

 to the rest of the car， the pre-charge resistor acts as a stopgap before both current's， before。

 both contactors actually close and prevent inrush current damage from all that high voltage。

 going into the other components of the vehicle。 And then there are B&B boards on each battery pack。

 The 85， 90 and 100 kilowatt hour battery packs all have 16 of them and those all connect。

 back through a bus into the BMS board。 They actually have bleed resistors and they're a very important component in actually maintaining。

 a level state of charge across the entire battery。

 All the firmware component changes that we actually did to the car occurred in the TMS， 320。

 Then there are a few changes to the shunt so we'll go over that。

 This next picture actually shows all the individual components kind of hooked up except for the。

 B&B's。 I just have one of those。 We have the pre-charge resistor， the shunt itself。

 two large pieces of copper with another， piece of metal welded between the two。

 The contactors themselves， voltage sense wires would actually connect to the four different。

 terminals， the contactors to measure that the contactors are open or closed and there。

 isn't a short situation going on there and also measures the amount of voltage currently。

 provided by the battery。 So a little bit of history on this。 The P85D。

 the subject of this actual research， it was announced on October 10th of 2014。

 But then almost a year later in July of 2015， LudaCrisis announced。

 So the dual motor performance vehicle was released before that and that's important。

 because there was a retrofit available。 For new vehicles purchased after LudaCrisis announced it was a $10。

000 upgrade。 But as a loyalty or for some other reason they decide to actually offer for a limited time。

 $5，000 upgrade for existing P85D owners but if you didn't take advantage of it during。

 that limited time， you couldn't take advantage of it later。

 So you couldn't buy a used P85D today and actually pay Tesla to do the upgrade。

 They don't offer that anymore。 The hardware part of the upgrade involves new contactors and a pyrofuse。

 Those are very important components that actually allow additional current to flow through the。

 battery。 And then many of the battery packs that came in。

 the Tesla's after that were actually already， upgraded with the new components and they were just LudaCris capable。

 So what that meant is there was just a configuration change that had to happen on the car。

 It was just a software change。 It wasn't where you actually had to reprogram firmware。

 You just had to make a single setting change on a single file on the gateway。

 So LudaCris capable basically means if you add performance add on one to the internal。

 dot dot file that's existing on the gateway， it would enable the car for LudaCris assuming。

 it had the battery pack and everything else capable。 So what we did。

 I had a donor vehicle and we actually did the upgrade。 I drove that to California。

 had the car out there because that's where I actually had， access to a friend that has a lift。

 There's a guy that goes by the name of Bitbusters of BMW Hacker。

 He has a body shop offered me use of his lift。 He was actually the first guy that was able to retrofit autopilot 2 to an autopilot 1 vehicle。

 I mean he was actually doing that work while I was working on this。 He was successful。

 He actually was able to add all the cameras and everything and make an AP1 car， AP2 very， cool work。

 But he helped me with this and so thank you Bitbuster。 So here's the vehicle up on a lift。

 Here's a picture of the vehicle with the pack dropped out of it。

 So we basically did all the components， removed the central bolts and then dropped it onto。

 this giant heavy weight chassis that could support the entire weight of the vehicle itself。

 Remove the side bolts and then raise the car back up so we can do the work。

 Here's a close up of where the actual pyrofuse sits。

 The pyrofuse actually sits between two sides of the battery packs。

 So basically midway through if you think of one side is being the most negative and one。

 side to be the most positive it sits halfway between those two。

 So between eight of the packs is where this fuse sits。

 And we had to go from a conventional fuse which is what you see in this picture to pyrofuse。

 Picture on the right is a picture of the contactor bay with the old contactors removed。

 And then in the next slide you can actually see a close up of the new contactors in and。

 a close up of the current shunt。 You can see that the current shunt exists right between the contactors and then the rest。

 of the battery and that's what actually measures the amount of current going through it。

 So those are the hardware changes。 What about the firmware？

 For this we need to dig into some Python。 So Tesla makes a diagnostic tool called Cool Toolbox。

 It runs on Windows but it's mostly written in Python。

 So it's an executable that's written in Python。 And it uses all these what you see are Scramble files in my little screenshot here。

 These Scramble files are encrypted and then they're actually compiled and then encrypted。

 but to reverse that we had to decrypt them and then decompile them。

 And these files are contained as individual plug-ins that when a technician is actually。

 using Toolbox these files are decrypted on the fly even if they don't have a connection。

 to the internet。 So basically what tells us is that everything that we need to decrypt those files you can。

 actually get from a laptop that's actually running the software。 So someone else， not me actually。

 if you're how to do that decryption， shared that information。

 with me and then I started to dig through these Scramble files to find out what we could。

 find out for the process。 So once you decrypt them you can actually just use Uncompile 6。

 It's a standard Python module and it'll give us the source code。

 And then what's really cool is that the source code actually had all the comments left in。

 place so next few slides are going to show some of the data we're able to get。

 So this is a little header of one of the files that was uncompiled and you can actually see。

 it has all the information about who wrote it， information on it and then just regular。

 Python source code。 All right， helpful comments。 So here's one specific to ludicrous。

 It tells us that you know some of the things that it actually has to have and some of the。

 things that it's actually checking so comments like these actually help point us in the correct。

 direction。 And then we actually had a lot of data structures。 So if you see these QT resource data。

 QT resource name and QT resource structure， that's， all binary data。 I wrote a really。

 really ugly Python extraction file to iterate through every single Python。

 module and extract it if those variables were there。 And then we just ran Benwalk against it。

 Once Benwalk was ran， we got all sorts of very useful things that were critical to actually。

 figuring out this process。 So once we did that and we got that， we were able to。

 these are some other artifacts that， we actually found inside the files。

 figured out that the PD5D that was our loaner vehicle had a battery pack hardware idea 57。

 And here are the steps that actually identify how to change that battery pack ID from 57， to 70。

 It specifically identifies three hex files that are TMS 320 hex files that are loaded via。

 UDS onto the BMS to actually perform the upgrade。 And the first one was the updater file。

 So basically that one's needed for shunt calibration， which we'll get to in a minute。

 And then the next one was actually an application updater file that allows you to update the。

 bootloader。 So the second file after the shunt calibration is done is a necessary one that's used to allow。

 the bootloader to be updated。 And then when the bootloader is updated。

 it also updates the battery pack ID， which has， all the configuration values to allow the pack to operate at ludicrous speed。

 So all the instructions and the files needed for the upgrade process are actually stored。

 in those toolbox files， both in the source code and in the various extracted files that。

 we're able to get out of it。 So what we also had in those firmware upgrade files were DBC files。

 DBC files used in Canvas， I'll straight that in just a little bit。

 ODX files that actually defined how to calibrate the shunt， grant security access to the module。

 over UDS and upgrade the firmware。 All of that data was actually stored in these basic Python source codes。

 So even if I couldn't run toolbox because I had access to the source code now， it was， decompiled。

 could actually figure out this process and use a third party software to make it happen。

 There were also some files that stored some interesting calibration data。

 You can see that the image of the screenshot says I。pickle。

 So this is the DBC file for a specific version of the Model S。 It's the DBC file that once。

 you actually extract out of the pickle file， it's just a straight DBC file you can import。

 it into files that can actually read DBC。 But there was also a pickle file that stored a shunt calibration data。

 That turned out to be critical。 So what that actually did is that pickle file had the serial number of every single。

 ludicrous capable shunt across the entire inventory of Model S vehicles。

 So that pickle file actually turned out to be critical when we had to figure out how。

 to upgrade the specific vehicle。 I'll show that in a bit as well。

 So just a little background on Canon UDS because it'll be critical to understand the。

 next steps of the process。 So sitting on top of the Canon network stack。

 there's this protocol called UDS which is， a unified diagnostic service。

 This protocol can be used to help to condition diagnose problems， you know， reads， values。

 and sensors in addition， it can be used to update firmware。

 Can networks use a descriptor file called a DBC？ I think that means database can。

 I've never actually seen a definition that defines what it actually is。

 But then for UDS for the diagnostics， there are files called ODX or GMD。

 I think GM is General Motors Diagnostics， ODX。 I'm not quite sure again what that one means。

 but those are stored in what kind of more， of a XML style as you kind of see over here on the right。

 And then the ODX routine that you actually see a little snippet of here is the one specifically。

 that covers the calibration of the shunt。 And you can see in the source code that there are values for the hardware idea of the shunt。

 CGI-1， CAU-1。 So those are two values that are actually updated。

 There are other values for CRC and the actual serial number。

 And then what I actually did is I used a commercial tool called VehicleSpy。

 We had a license for that for work。 We imported the DBC files into that and then also the ODX files and we're actually able。

 to run everything from VehicleSpy。 That's a tool made by Intrepid Control Systems。

 And then the other thing that we actually found out from the DBC file is that the RBI-D7 E2。

 7 Echo 2 and 2-0-2 from the BMS actually identified a max current available from that particular。

 battery pack。 And then the next 3， 232， 266 and 2E5 from the BMS and the drive-in-rotors actually identify。

 the amount of max power in watts。 These values vary and they vary based on the state of charge of the vehicle。

 temperature， of the battery pack and possibly even the drive-in-rotors themselves。

 And then it also varies based on power recently used。

 Tesla uses a thing called a leaky bucket theory which basically means if you have used a lot。

 of power recently it's going to limit the amount of power you can have until it has a。

 chance for that kind of bucket to refill。 That's kind of the concept behind that。

 So if you've been honing it for quite a bit the car is going to back off the amount of。

 max power that you can have right now so you can't run max power continuously。

 But what a DBC file does， this is a screenshot from VehicleSpy。

 It can see all you can see are the CAN RBI-Ds and a bunch of data bytes in data。

 You'd actually have to reverse all engineer what each of these individual signals means。

 But with a DBC file it looks just like this。 So now we actually have all of the signals identified and broken out。

 So now we have BMS power available。 This is actually a screenshot from when the vehicle is not engaged so it's just a limited。

 amount。 18 kilowatts that are available on RBI-D232。

 Once you actually put the vehicle into drive or even in park that goes way up again based。

 on the temperature and state of charge。 And then also we took those ODX routines that we extracted out of a toolbox and imported。

 those into VehicleSpy。 You could use other open source ODX tools to do this。

 Just VehicleSpy was easy。 I had access to it so we used it for this upgrade。

 And you can see that first of all what we actually have is security access。

 So we were able to extract the security access routine from ODX which actually uses a static。

 seed and key that's something that was previously disclosed in another talk on Tesla。

 But then we also have the ODX routines for shunt calibration。

 So for reading the data out of the shunt and then rewriting it all of that was in those。

 ODX routines we were able to import those into VehicleSpy and recalibrate the shunt。

 And you can actually see a valid value for a valid shunt in this screenshot。

 The serial number was and the way it works is that actually looks up in that Pickle file。

 looks up the serial number and then looks up what the new CAU-1， CGI-1 and CRC value should。

 be and then writes those to the shunt。 That's how the shunt calibration is actually done。

 So it turns out that that wasn't everything that was needed to be done by the shunt and。

 thankfully I actually decided to be safe and do all of this research on a test bench first。

 I was able to actually get a shunt that was ludicrous capable but that wasn't actually。

 done in the research。 In the upgrade it was just one that was separately provided as part of an upgrade kit that I was。

 able to find on the secondary market。 But what I figured out is that the shunt actually needed a hardware modification in addition。

 to all these recalibration things that we had to do to it。

 Here is a single wire that connected the shunt to the CPLD。

 If you remember earlier I was talking about how the CPLD is a backup to the BMS and I assume。

 this is some type of emergency signal that the shunt would use to tell the CPLD to do。

 something based on whatever was going on。 So basically after I did the firmware upgrade and upgraded a BMS from 57 to 70 if I had the。

 shunt plugged in it would generate an alert， an active alert that actually identified that。

 there was a current sense error。 So what I did is I actually made a breakout board。

 I separated all the wires from the shunt going into the BMS and then just ran a logic。

 analyzer on them。 I figured out that if this one wire was disconnected the error actually went away。

 So what this actually meant is that when the battery pack was opened up this shunt actually。

 had to have this wire disconnected from the shunt itself otherwise it would generate an， error。

 And it also meant that the entire process had to be done all the one time。

 So once the battery pack was dropped and the wire removed from the shunt it would generate。

 an active alert if the firmware was not upgraded。 So it meant that we had to do the hardware modification and the firmware modification all。

 at the same place all while I was out there in California during the work and it's what。

 led to the next very difficult steps of the process。 So here's the actual process itself。

 So I had access to the garage and left in Southern California。 I drove there to do the upgrade。

 We had to make sure that the vehicle had a very low state of charge for safety reasons。

 We had to drop the pack had to get some what are called linemen's gloves which are involved。

 in doing high voltage work。 I do all the hardware stuff， replace the contactors， replace the fuse。

 modify the shunt and then， reinstall the pack。 That was very tricky。

 I didn't want to have a rich rebuilds moment where I put the pack back in and had it start。

 leaking and then had to go and drown on my own tears about how much I actually broke。

 a very expensive vehicle。 So I used a bore scope very carefully lowered the vehicle and the lift made sure that all。

 the various connectors were lined back up and then put the battery pack back in the car。

 and it connected back up。 And then updated the internal dot dot， added the ludicrous command。

 changed the pack ID， and then had to redeploy the firmware which is basically just reinstalling the same firmware。

 that's already on the car to tell it the rest of the components of the car that actually。

 had a new pack ID。 So then the next up was just drive away and enjoy the ridiculous amount of torque。

 right？ Nope。 That's not what happened。 So we used known techniques that we've used before。

 tried to redeploy the firmware， tried， to upgrade， tried putting new software on it。

 Everything we tried for about a day and a half failed。 So now I was stuck with a very。

 very expensive brick。 We couldn't even get it out of the garage because the parking brick was engaged。

 Had to figure out how to do that too。 So finally， I decided to punt， I called。

 found a towing referral service online and have， the vehicle towed from Rancho Cucamonga to my home in Las Vegas so I could continue to。

 work on it。 Talked about a stressful weekend。 But it only cost $360 or $3。600。 So not great。

 not terrible。 It turns out it was actually worth it because what I did is I learned something cool。

 The gateway uses a file called firmware。rc。 During the upgrade or during a redeploy。

 this file is generated。 And basically what it does is it looks in a index file for the firmware that's on the。

 vehicle and then it generates a number of CRC values for the various components。

 You can see in this image you have the gateway having its own CRC value， the BMS having its。

 own CRC value and the BMS when the software has changed， the CRC value changed。

 And that is what prevented the vehicle from engaging the contractors and being able to， drive。

 So I saw a single reference done I think by the 10 cent guys where they actually talked。

 about firmware。rc when they were doing some reverse engineering of the gateway。

 And then I also saw the firmware。rc file mentioned in the error logs that I gathered。

 when I was trying to do the software redeploy myself。

 So I went onto my bench unit before the car was actually towed out to me and actually。

 requested that file from the gateway。 GW transfer， firmware。

rc and then looked at it and sure enough， here's this file with all。

 these various CRC values and then a new CRC value at the end。

 So what I tried to do is I did the firmware upgrade on the bench one more time。

 I was always having errors on this because it was the bench and I didn't really think。

 more of it beyond that。 I actually looked up the correct CRC value from the BMS。

 It also happens that it's broadcasting that across the canvas。

 So I could have grabbed it from that as well。 But instead I grabbed it from the index file。

 put that value in it， made sure that that， firmware was actually what was running on the BMS。

 And then I had to recalculate the file CRC at the very end。

 So what I figured out with the help of some people online who are actually very grateful。

 to that that's actually a jam CRC。 So if you take the entire file。

 figure out the jam CRC value for it， minus what you see， in this picture is line 12。

 but it's much further in the file because I've trimmed it， down for clarity。

 And then add that single line file CRC with the additional value at the end。 The gate wheel。

 except that CRC value and the car was able to wake up。 So after I got the car back。

 pulled the file from the gateway， did the recalculation， calculated， the new CRC value。

 and as soon as I pushed it back to the gateway and rebooted everything， the car woke up。

 So that was actually kind of the cool thing that came out of the failure and the breaking。

 of the vehicle。 I figured out how this firmware that our C file actually worked。 And then with that。

 someone should be able to actually deploy whatever firmware across， the vehicle they want。

 deploy their custom firmware， or do something else that's pretty。

 cool that wasn't intentionally designed。 Of course， if you do this wrong。

 you could cause some pretty bad things， so I don't， recommend actually doing this in this。

 So we're really careful， but it was actually a pretty cool thing that we figured out。

 So here's my kind of POC slide。 This is a Canvas。 These are the static values that I talked about earlier。

 Before the upgrade， it would have a max current of 1305。 After the upgrade， it was a 1516。

 And then there's actually a separate CR value under CanID 202 that's actually reduced a， little bit。

 I don't know why。 If someone from Tesla actually wants to share that with me and they tell me I got to keep。

 it secret， I'd be happy with that， or they tell me I can talk about it。 That's why I do。

 But I would really like to know why the actual max discard current value is less than what's。

 actually being put out by the 72 Canvas ID value of 1516。

 By that discrepancy is there is there some type of attenuation or some type of de-rating。

 that's going on。 It doesn't actually look like there's any de-rating going on because it doesn't say。

 there's a de-rating active， but it could be that just CanID is not being used。



![](img/9345a7e7dac034a10438e5588fb2a101_4.png)

 So beyond this， where can we take this further？ So we've actually managed ludicrous a P85D。

 It turns out the TMS 320 is actually supported 9-0 pro。

 I've actually done a little bit of research on that using the firmware。

 I've found some interesting things。 So there are those RV IDs already。 We talk about 7-0。

02 and 202 that define the max current。 It seems that we should be able to increase the speed beyond ludicrous。

 but I can't say， that I actually recommend anyone doing this unless they're really。

 really careful because， these limits are put in place for a reason。

 Just like anything with any vehicle， there are limits and then there are limits that。

 are dangerous to push yourself past。 But it's possible that someone could actually figure out and someone could work with me。

 and actually figure out to go past ludicrous because it seems reasonable that if we have。

 access to all the firmware， we should be able to bump up these limits past what it would。

 be able to go and then actually find out where the limits of the vehicle are。

 So hit me up if you'd like to assist。 My information is easily out there。

 You can find me on LinkedIn。 The other big thing that I'm really curious about are what are those shunt parameters？

 CAU-1， CGI-1。 I noticed that before the ludicrous upgrade， they were different。

 but after they were in， that pickle file， they were all the same。 And then just some references。

 Obviously， the space ball movies， the various announcements from Tesla。

 A definition of a current shunt if you'd like to go up there and actually figure out。

 how one of those works and how they use a known resistance value to a calculator amperage。

 A data sheet in the TMS 320 and then of course， in truck and control systems， the maker of。

 the software that I use to actually do the upgrade。 Thank you。 Okay。

 so going over some of the questions。 The shunt resistor tolerance isn't something I check to assume that since all the shunt。

 resistors are actually stored with values in a table， those probably have to be pretty， accurate。

 but it's something I can look into。 Over the air firmware updates， the way those processes work。

 this is a question from Michael， Bowman， over the air firmware updates actually completely rewrite that file。

 So no， they wouldn't。 It should be just fine because when I've actually done over the air firmware updates on the。

 donor vehicle， it actually completely rewrites the file。

 So it would probably fail if there's something in the config that it doesn't like。 In fact。

 that's what I did。 So no， it shouldn't cause the CRC to fail。

 It should just completely rewrite that file。 To answer， I'm going to try and get these in order。

 So I didn't actually try anything with reverse gear。

 I would have to actually put it in reverse and actually look at current limits for that。 So no。

 I haven't actually done any of that。 The Model 3 long range， all wheel drive stuff。

 I haven't done any Model 3 research， so I， couldn't ask you that。

 Although I have seen a lot of the research that basically said it's the same vehicle。

 but Model 3 has a completely different way of actually controlling the config on it。 And it's。

 again， it's not something I've researched at all。 I did work with Tesla on the subject of this talk and they were actually very supportive。

 of it also。 All the slides went through them。 We had some meetings and they were very supportive of me actually doing this reverse engineering。

 Matt's question， does Tesla apply specific access measures on the Canvas to frustrate。

 these type of approaches？ Not that I've seen。 It looks like all of the effort right now that Tesla is doing is actually on the main。

 MCU。 So they're actually doing a lot of firmware updates to actually frustrate people getting。

 access to the MCU itself。 The Canvas still uses the same static seed and key。

 And if you want specific details on that， check out the car hacking village talk。 But basically。

 the seed and key are static on every single Tesla module that I've tested。

 up to the most recent ones。 So that would be the biggest access measure that they could do on that to frustrate this。

 kind of work。 Right now， the only ECU that I focused on is the BMS。

 So there's not really any other ECU that I'm confident I can modify the firmware for。

 The other recovery procedure was basically not to make the config change that actually。

 caused the firmware flash， the firmware redeploy failure in the first place。 Alan。

 that's a great question about the parking rate。 So if you open up the trunk of the vehicle。

 actually I had some pictures of this just for， time。 I didn't really get into it too much。

 The way we got the car out of the garage was we looked at where the module was。

 I had access to all the wiring diagrams。 So we knew that the module for the electronic parking brake was in the rear of the vehicle。

 behind kind of the side panel in the trunk。 So basically you pull off that little。

 I don't know what to call it， kind of like a， fibrous material covering the metal skin of the vehicle and the edge and you have actually。

 access to where the parking brake module is。 So what we did is we actually disconnected the leads going down to the actual electronic。

 parking brakes themselves and applied 12 volts to those with a correct polarity。

 I had access to a wiring diagram so we can actually see the wire colors and that actually。

 unwound the parking brakes enough that we could freely push the vehicle。 Newer Intel stuff。

 I know some people are actually hacking on it but only work has been。

 on the Nvidia so I haven't done any stuff on the newer Intel ones but the newer vehicles。

 actually all are P100Ds and may actually have the batteries upgraded automatically。

 I know some people have done some stuff on the Intel units but these different EMMC and。

 it's really completely new hardware。 There was some cool stuff some people did on the gateway but I think Tesla closed those。

 avenues off。 The only authentication I saw on the firmware itself was the CRC value but the overall firmware。

 is signed so when the overall firmware is loaded onto a vehicle there's a long signature。

 check that occurs。 Lexi or question， the work could be used to increase mileage。

 So range on the vehicle that's a really tricky one because a lot of that's based on resistance。

 I don't think any of the work I could do to actually increase that other than basically。

 preventing people from stomping on their vehicle。 Again I didn't try any of the UDS stuff because the car was。

 because the contactors were forced， being closed I was having trouble even maintaining enough 12 volts to even do anything at all。

 So I figured the best bet was to actually physically attach to the parking brake modules。

 and unwind them。 William Redding， I don't quite understand your question。

 If you could clarify that a little bit， what keys are you referring to？

 Keys for all these things stored。 So what keys do you mean by that？

 But I'm not quite sure what you mean by that and I think that brings us up to date on the。

 questions。 Oh now we have a question one。 So I think we covered all of this。

 Yeah I think that covers everything in the main chat。 Oh okay so how do we obtain the sign keys。

 So that was actually based on some research some other people did。

 I'm not going to comment any further on that because they've asked that I keep their facts。

 kind of to themselves。 They showed me how to do it but then they asked me not to comment on it。

 But basically if you think about it like this you have a technicians laptop。

 That laptop has these encrypted Python modules on it yet that laptop is designed to operate。

 entirely independently of being connected to the internet。

 So whoever's logged into that laptop needs to be able to run toolbox completely independently。

 So therefore it would only logically assume that somewhere on that machine were the keys。

 or the encryption keys for actually decrypting all of those modules。

 And that's about all I can say on that。 But pretty much everything uses standard Python library so if you're a Python guru it's probably。

 not going to be that difficult for you。 And see if we have any other questions。

 Got a couple more minutes。 So any more questions？ Alright， thank you Jerry for the feedback。

 I appreciate that。 So the config itself if you kind of remember the beginning of talk there's a talk where。

 we talk about the file internal dot dot。 That internal dot dot file the contents of that get sent up to Tesla on a regular basis。

 So I'm assuming if there's some diffing going on on their end if anyone tampers with their。

 internal dot dot file they would know right away。 Yeah。

 so there's actually been quite a bit of reverse engineering work done to do all。

 sorts of stuff like adding the heated rear seats adding the heated steering wheel adding。

 dual chargers adding heated washer nozzles to vehicles didn't otherwise have that。

 The guy that loan me his garage actually figured out how to reverse engineer the entire autopilot。

 two systems so that he could basically shoehorn autopilot two into an autopilot one vehicle。

 For those of you aren't aware autopilot two uses eight cameras autopilot one is one autopilot。

 two was created by Tesla autopilot one was the mobile system so it's completely different。

 system he even had to replace the steering rack replace the brake modules as well as add。

 all of the cameras including in the the B-pillar where there wasn't really a hole for it he。

 actually had to cut the hole for the B-pillar but considering on the body shop that was relatively。

 easy work for him he actually got a functioning autopilot two system running in a vehicle that。

 was originally configured with autopilot one。 So the other there aren't really any other Tesla ECUs that I've focused on I'm just aware。

 that several of the other ECUs actually have a TMS 320 so those techniques could be very， similar。

 So I think that's about it again check out the car hacking village talk or the DEF CON talk。

 later this week there's no charge for either of those and hopefully we'll have some other， Q&A。

 [No audio]。