# P16：16 - Security Research on Mercedes-Benz - From Hardware to Car Control - 坤坤武特 - BV1g5411K7fe

 Hi and thank you very much for joining this session。



![](img/c31f5a3ad53007941634e093106b4428_1.png)

![](img/c31f5a3ad53007941634e093106b4428_2.png)

 My name is Guy Harpak。 I'm head of product security and the Mercedes-Benz R&D hobby in Tel Aviv。

 I'm very excited to be speaking here at Black Hat and I hope that we'll be able to meet each。

 other next time in a more normal or new normal format face-to-face。 In the next 40 minutes。

 I'm going to present， together with my partners from 360， more。

 information on their security research done from Mercedes-Benz that we presented together。

 at RSA earlier this year。 But before we dive into the technical details of the research。

 I want to talk about something， big happening in the world today。

 The last thing I want to talk about is the transformation going into the automotive industry。

 I'm sure most of you are aware of this transformation。 You probably even feel it in your day-to-day。

 We in the industry like to look at this transformation by following four major trends。 First。

 the connected car。 Cars becoming more and more connected。

 To the internet connected to other infrastructure， seamless and intuitive mobility。

 Enabling features ranging from entertainment into more unique and special， even safety， features。

 Second trend we follow are the autonomous cars。 I'm not going to talk about it too much today。

 We all know， we talk about it， we hear about it， we read about it in the news daily。

 I want to share more insights here， but we all know that this is a major trend going。

 through the industry。 Third， shared mobility， services around cars。

 shared mobility allowing new usage models， and services， new digital services for us。 Car drivers。

 car customers。 Last but not least， electric mobility。 Locally free emission。

 very exciting trend and again a shift for the entire industry。

 But we are a cyber security conference， so I will focus today on the connected cars。

 And I'll focus on securing a connected cars。 Securing a connected car is a challenge。

 A car is a system of systems。 It has a lot of complexity in it and securing it is exciting。

 But when you think about it， securing a connected car is not enough。

 Because we think of how to secure each car and how to defend the entire fleet。

 And in order to illustrate the challenge here， I like to think of a patch of road， a patch。

 of highway。 Imagine a highway in rush hour。 So just imagine。

 how many cars do you see in this patch of highway of highway doing rush， hour？

 How many types of cars？ How many different modelers？ Some of you probably know。

 each car can have between 70 to 150 electronic control units， issues。

 Those are little computers with a microcontroller or microprocessors。 Running code。

 connecting to the network， they have interfaces and they could have vulnerabilities。

 So if you multiply the amount of cars that you imagined by the amount of ECUs， electronic。

 control units， you can get a single patch of highway to thousands， tens of thousands。

 maybe even millions of cars。 Think about it around the world。

 This is one of the largest IoT network around the world， connected cars。 Now I always say。

 I have three little kids and I have a dog back home。 This is a very interesting daily challenge。

 But professionally， defending a fleet is one of the biggest challenges I've seen。

 And we are tackling this challenge。 In order to do that， we have an approach。

 We like to call it fleet secops， fleet security operations， defending the fleet of cars on。

 the road。 How would you do fleet secops？ How would you do fleet security operations？

 Pretty much like you would do for standard enterprise network， standard IoT devices， all。

 of the other systems that we need to defend。 First， you assess where you stand。

 You try to identify all of your vulnerabilities。 You try to know what are the threats。

 what are the risks。 Maybe you have a red team in place。

 maybe you have a threat intelligence team in place。

 building the threat landscape for you so you know where you stand。 You can assess。 Second。

 you have the technologies， the processes， the people in place to detect any attack。 So for example。

 use the head unit， the little computer that you use to maybe hear music， navigation。

 maybe control the air conditioning。 And this little computer in the car is suddenly sending a braking signal。

 then something is， probably wrong。 But take it to the higher level， take it to the fleet level。

 Look at all the anonymous data you get from a fleet。 And try to see。

 does my fleet today behave the same as it did in the same geography yesterday？

 If there's something different， potentially I've detected an attack。

 And then come to the third stage。 The ability to respond。 People， processes。

 again technologies in place to respond to any potential attack。 So you can investigate。

 so you can do any changes fixing any vulnerabilities that are， needed。

 So you can do the forensics to understand the scope of any potential attack。

 And then after you have the lessons learned， the fourth stage， last but really not least。

 because it's all part of a round signal。 It's happening all the time。 You need to protect further。

 protect your fleet。 It can be the cars that you're developing today。

 And it can be the cars that are already in the road。 So in essence， between any event and event。

 you have better security for the entire fleet。 If you want to learn more about fleet backups。

 then you can look at the presentation we gave， at RSA earlier this year where we talked about the key principles and now we tackle the key。

 challenges in fleet backups。 But since this is blackhead。

 I want to leave some time to the partners at 360 to detail。

 more about the great research they've done and now it helped us improve the security of， our fleet。

 So thank you very much。 I invite you to the virtual stage， the partners from 360。

 And I'll come back to the virtual stage afterwards to talk a little bit about what we did once。

 this year， those findings were responsibly disclosed。 Thank you。 Hi everyone， I'm Jaoli。

 Today I'm down to speak at Blackheads。 It's a dream come true for me。 Since Blackhead， real world。

 since four Skydoteans had worked and the Ming Ray will start his， talk。 Please enjoy。

 Skydotean is the security research team established in 2014。

 We've worked on connected car and industry security。 In China。

 we have about 75% market share on cybersecurity of connected car。 Notable research。

 About 60 years ago， we do the first cybersecurity case on Tesla and the BVID connectivity for。

 technology。 And we do the research on Tesla's operating system。 About three years ago。

 we developed an evaluation platform called Campic。 And in 2019。

 we developed a numerical active defense system for the campus。 The last four years。

 we have done the best advanced cases from hardware to control。

 This is the turn of our research on Mr。 Spence。 About two years ago， we started our research on Mr。

 Spence。 Then we spent about one year to find the one ability to achieve car control。

 Then we repulsed the findings to the Daimler。 Then they fixed their one ability very quickly。

 And we designed to publish our research today。 This is the result of our research。

 the impact of our research。 Our ability can impact our Mr。

 Spence connected car in China in about two minutes。

 And we can get access to remote service to control car like controls， lights， windows， engine。

 And we can do this since without physical access。 This is a agenda of our talk。 First of all。

 we will talk about how to build test bench。 Then we focus on hermers。

 The last thing is the way to car control。 This is the most important thing you care。

 Then we will give a summary from the Skaggle and can we introduce the instance response。

 in their decision and their summary about our research。 Okay， let's talk about our build test bench。

 In our research， we have two very important components。 The first one is Hermers。

 At the normal add TZU， the tZU， the second one is HAD unit。 As normal add， we are going for the TZU。

 Hermers。 Hermers is TZU， the TZU， the TZU， the TZU， the TZU， the TZU。

 This device provides communication for the vehicle。

 This device has too much attention from security research since it helps users to remote monitor。

 and control car。 We found several motions of the Hermers from the market。

 This Hermers has little differences between each artist or hardware design。 Ideally。

 it's a computer that provides infotainment functionalities。

 This device is also called the numerical infotainment system。 This provides Bluetooth。

 Wi-Fi and USB service。 It can interact with it to control the car and get more information about the car。

 We are done with HAD unit and we learned that this HAD unit is much more complicated with。

 the same device in other cars。 Whenever in hardware and software， we will talk more detail later。

 The first step for us is to view the CAT wrench。 It helps us either to do the research on the table。

 With all this image， you can see the left image according to the architectures of Mises。

 Bands connected car。 It helps us to make HAD unit and Hermers works。 When we built CAT wrench。

 we can't turn it on。 The safe of this expense was designed for NTG 2。5 since 1990。

 The NTG has caused much inconvenience to our research since it's reached our summertime。 After now。

 we've made three types of NTG。 The first one is warning you to restart HAD unit。

 You must ask your dealer or contact Mises service center to deactivate this NTG。

 With help of dealer， we saw this program temporarily， but it is easy to check such tools。

 and I will add some new research。 We will encounter this program。 For more convenient。

 we back up the SD card data to restore HAD unit。 Whenever we want。

 then we use the cameras to find out the check message。

 We made CAT wrench to meet two of which have two wing can't receive。

 It can divide the ECU network into two parts。 Therefore。

 we can filter or modify the message on cameras。 We have analyzed the message on KHMI and CATA。

 In this process， we found that once the IC or the country panel disconnected， the HAD unit。

 will restart quickly。 If we send abnormal CAT message to the HAD unit， for instance。

 we change the VN and， it will check the HAD level and save。 This time。

 we have only one way to do it。 We face the restore the HAD unit。

 We found out different HAD VN message for this two-way camera bus for the regular memory。 Moreover。

 we found out the same ID to prevent HAD level and save。 Finally。

 we built the test bench successfully。 Before talk about vulnerability。

 we should analyze the attack back turns first。 The HAD unit is the first device we researched。

 The version is NTG 5。5。 It's used Windows CE， automotive server operating system。

 And it has spent so much time to analyze it。 Because we must figure out how it goes and how it goes。

 the kernel， the first-center and， the kernel are together。

 And you can see there are so many fields with the crafted and extracted in this picture。

 But it doesn't mean that we clear this game。 It's just the beginning。

 It's very hard for us to do reverse engineering on these fields which have many structures。

 and objects for inter-process communication。 We can guess what they are。

 The most difficult is how to dynamic debug them。 We never started Windows CE and we have no knowledge about it。

 Moreover， it's too old and discarded。 But I pulled the sock of the HAD unit。 It's Renaissance。

 Our car H2 solution。 We don't have our car H2 debug environment because we didn't buy it。 You know。

 this is called our car H2 debug。 Even if we got it， we have no user manual。

 And the Windows CE automotive server is not an open source project。 We have no idea about that。

 We got a fully blown black and installed Windows CE 7。

 And then we tried to run this extra cupboard first for a long time。 But it has too much issues。

 So we gave up the research on NTG5。 The next attack vector is OVD2。

 I think car HAD can research cars OVD because it usually doesn't have a pro- protection。

 The OVD interface connects to the thermos and the EIS。

 The thermos is the target communication should contribute。 The EIS means electronic ignition system。

 Also the EIS is the gateway in the vehicle。 The OVD attacking its physical accesses is limited。

 And the severity is low for new with the space car because we can affect the power of human。

 safety functionality。 So that's their bag and the display from OVD2 interface。

 And you may ask why don't you pay a new key from OVD2 with X-H8。 If I turn to OVD。

 we can do that without FDS4 data because we don't have any authorization， account from the demeror。

 The last attack vector in the test bench is Hermes。

 The Hermes can change a 14 module that connects to the car with the internet。

 Hermes is designed by Hermann Harder。 That's used in the budget。 It makes it easier when to see it。

 HAD unit。 It access the internet through the thermos。 The thermos communicates with the HAD unit。

 It has many methods such as Bluetooth， Wi-Fi， USB， the protocol code WCC。

 Which is a demeror or protocol。 The traditional attack vector is useless for Hermes。

 But we still find some useful attack vector in hardware aspect。 Let's talk about Hermes。

 Hermes plays a very important role in communicating with the internet。

 This is the critical country in our talk。 In the beginning， when we got Hermes。

 we had no idea about this。 Because this device is much more secure than other devices we have seen。

 Hermes has multi-ship plan versions。 It's waiting for an 3G to 4G。

 The older models use Qualcomm salary module。 The new one is easily in-con salary module for the PCB design aspect。

 The USB interface was removed from the old one。 They intend to build with shorter masks in the PCB。

 which made it more challenging to， build the test bench。 In the latest Hermes 2。

 it doesn't have a new feature。 Only the GPS module and antenna has been optimized。 For older models。

 such as A-class， G-class， G-class， the Hermes connects to the HID unit， via your SBE。

 And for the old HID and MOD， the Hermes connects to the HID unit where Bluetooth for the new。

 models， the Hermes version is 1。5 and 2。1。 Bluetooth connects to the HID unit with W-C-C protocol。

 Then Java will take over here。 Tell more about the detail。 To find a different process in the PCB。

 we should prepare a 4G module in a P-7 and find， a appropriate P-con into the PCB wiring。

 For you can have this process with your tools。 There is a hard way to find a 4G module being out。

 Use the feature to work the station， hit it from the button and 400 Celsius for long。

 beneath and you carry both hands quickly。 Then you can use the flash-out and watch features to find out the wiring。

 It's common that an IoT device seems to be on it， but both of course， we find that you。

 want to debug more than a pervise with some ADL names and user names on the network and。

 visualization of。 Also， we find back new URLs for this launch。 At the US people。

 we find it's more than it can match。 We'll have more debug boards with the set mode command。

 At the 4G module dedicated， you can open a PCB service with set mode。 However， for Hermes。

 we didn't open the PCB service in this way。 We find more in the device list after sending set mode commands。

 We can send more AD command to the application interface。

 There is another way to obtain an application configuration that hits change to USB mode。

 and send this command to a fast-PDD-compass configuration。 The API name is there。 For example。

 you use the 8K upgrade to flash the system。 You should be packaged in a variable from where and bypass the signature check。

 Furthermore， you must keep the watchstock sleeve otherwise we'll upgrade the 4G module。

 The emcee will recess the power and you will get a upgrade。

 We don't have a JTAC back tool for the emcee。 But we've found JTAC back in the face at the 4G module。

 It has to test the break-upon and successfully。 We can rewrite the put-a-lola because it was loaded in memory。

 As a matter of view， you can modify the memory。 You can download the firmware。

 But we don't have the non-driver。 It's a little bit hard to read the non-flash from the JTAC because you have to re-engine。

 the current mode and find out the ranges to follow the non-controller。 On the other side。

 the break-months are unstable。 If you enter the break-months。

 the watchstock will start your 4G module。 To prevent it from restarting。

 you should connect the emcee with your device and feed。

 the watchstock while the commences are running。 This way， it will travel the way。

 So we have to find another way to get the firmware。 Let's see the storage memory。

 Open the shared-off cellular module。 You can find the emcee key， JTAC。

 which is composed of non-flash and DDR。 But it is all the new 4G module。

 Both use the same memory as flash。 The difference is just the packaging。

 We use the mid-year remote station to heat the tear down。 The left is the old non-flash。

 The right here is the new non-flash。 For the mid-year 1 or 37 non-flash， we don't have a perp rate。

 So we have to wire up into the T-soft for the NP-V adapter。

 Today we just need well 16 magnets wires， which includes 6 control pins and 8-bit eye openings。

 Rest pins are the WYSS-C and GLD。 This is not hard but with your time and your eyes。

 of course we've fed up in the eyes with， wiring up magnets wires。

 So we made some PCB in the dactress and some keys。

 The mid-year can wire out all the 4G pins because we wanted to support 8-bit and 16-bit。

 NAND flash by just changing the circuits only so that we can reuse the circuits。

 But for smaller size NAND flash， we just wire out the required pins because the dissonance。

 on the T-both is so small。 That PCB section doesn't support the width and the width and out。

 The next is 2-dumb data from the NAND flash program。 HVH-H happened in 2014 by St。

 Azon and 64 by St。 Spare area。 We can use to correct the parameters to redo the tape from the chip according to the user。

 manual。 The spare area is cut or be an area。 At first we created the NAND flash manual and we found some about the spare area。

 The officers just had the last 64 by some each page。

 But we found that the last 64 by some system data。

 It doesn't match the user manual because the spare area mapping depends on the NAND。

 controller in the 4G module rather than the NAND flash chip。

 Comparing the NAND-Dumb between the Qualcomm and the HVH-C。

 You can find the mapping of the spare areas。 Another thing。

 There are two ways to find the mapping of the spare area。 One is to check the source code。

 Also the NAND flash controller has the spare area mapping and this is the， the Qualcomm。

 NAND controller driver is open source。 We can find details at the code comments。

 Also the automotive NAND flash is single level cell。 The Qualcomm device is a 5。00 laptop。

 Each sub-page has one spare area but not only NAND spare area mapping is open to the public。

 For the NAND mapping you can pick out two pages with the spare area。 Then， compared to the new。

 you can find something special at the same offset as a page such， as the FFF or the random bias。

 This belongs to the spare area which includes the bad block flag and the E-50 data。

 The next is to make two or three motors spare area。

 There are three different types of spare area mapping。 In the current， including both the others。

 systems and models， and a weak health system， partition only。 In general。

 the spare area does not include data about the whole system。 However， there is not。

 Qualcomm system partition use YFFS which is a fast system for an in-depth device。

 The YFFS needs to use 16 bars in the spare area。 After removing the spare area。

 the firmware is still a single battery file。 There is no two-spotted extra data from it。

 so we need to find a partition table from the， 4G module to separate each partition。

 For the Qualcomm 4G module， you can find partition tables by searching these magics。

 The start of it bias if the primary matching and the sub-magic， the number of ways to search。

 for an NIBIV strings has a master funny， positive partition name of the table。

 We wrote a panel screen to pass the partition table and extra files from it。

 We found that the daughter is an Android image format。

 It uses multilateral controller such as the APPSBL-SBL1-SBL2。 Obviously。

 the 4G module has secure boots。 You can find a symptom which for upgrading the redundant。

 The high-city-count 4G module will process in similar to Qualcomm。

 The boot router in our high-city-count development kit always brings the partition table with。

 its boots。 The partition table is the same as the Hermes。

 Another way is to check the open source code of high-city-count。 In the NANDAMT。

 you can find the string card P-table hat。 If you Google it。

 you will find the boot router source code。 We can find the partition table and extra-trend partitions according to the source code。

 Finally， we pass the partition table of Hasekun。 The name is Balomissen Archuutile Telematy。

 It's the same as the 4G router which use the Balomissen Archuutile。

 You can find something just added for PDFR。 A small， the file system is YFFS2。

 The YFFS is designed for the NANDFASH to keep the flash more slumper。

 The YFFS uses the well-leveling so that block is non-signal。

 The block mappings are in the spare area。 We route the tool to operate the YFFS DANDLONG partition according to the Linux YFFS driver。

 We got other system files and we found a lot of OEM apps。

 So the next step is to analyze these applications。 During our reverse engineering works。

 we found that some of the references are missing or， wrong。

 And some executable files can run in the emulator。

 That makes me confused after comparing the firmware dumps from the same NANDFASH。

 We found a bit freepid problem。 After troubleshooting， the bitfaping is a phenomenon。 Therefore。

 non-control needs YFFS。 The other Qualcomm NANDFASH has bitfaping errors but at the NANDFASH on Chromys 1。

5 is perfect。 To fix the bitfaping errors we have two ways。

 One is to compare NANDFASH down repeatedly。 Repress the change bit with bitfaping which is most frequent。

 But that does not really solve the problem。 The other one is to check the source code of the YFFS DANDLONG。

 But it just shows the YFFS algorithm type。 The coding implementation and the polynomial is not the same。

 This usually not only to the public。 The Hermes Jordan port only plays out loud。

 doesn't have an interactive shell。 For dynamic debugging。

 the Hermes app will need to modify the same line to prevent the。

 YFFS sharing directed to the Hermes ports。 We can disable the firewall and over-advious sub-is。

 But we can write the change to NANDFASH directly。 Because non-control has hardware， it says it well。

 the NAND program doesn't。 So we have to generate correct uses in the emerging tool， the OBE area。

 the right thing， to the NANDFASH。 We got an ECC algorithm and the journey to correct use。

 The same as the data from NANDFASH。 We can take the data area， ECC area。

 reverse area and that block flag to get on each page。

 Because the secure code only checks the boot procedure， we can change the system files， free。

 then flash the neural data to NANDFASH。 The last job is NANDFASH。

 This optics BGA workstation can have you align the balls。

 We can modify the system to build Hermes DVC。 After we got the road shell。

 we found application cut engineer modes， which can manage the Hermes， sub-is。

 It has a constant constant signal but it's not a real constant signal。

 It just comes traffic PEU and sends it to the SH2A/MCU。

 The answer is that the WEDL will transfer the tool to campus。 If you need to get access to campus。

 you need to modify the SH2A from WEDL。 But I think this is not my network for me。

 Because even the ways you need to learn the SH2A instruction， then we got the SH2A/ID and。

 the back tool and we spend a lot of time to reverse and patch it。

 So we give up the SH2A reverse engineering。 That's the research work for the Hermes。

 And we also found the water BTSB through Hermes。 Now we will show the research about the back-hand。

 Okay， thank you。 I'll have to go over here to talk more about the cut control。

 We got an interactive chair in Hermes。 However， we have no way to attack Hermes directly。

 Now turn around and think that if we could control the back-hand， can we control Hermes。

 then can we eventually control the car？ At the right beginning we got a 4G LUTER。

 Then we wrapped up the YISIM to a SIM card adapter。

 Configured the APNIC information we found earlier。 But it doesn't work。

 because the 4G LUTER should no servers。 After troubleshooting。

 we found out that M2M back-hand could detect the M-E-I and if the， M-E-I changed。

 the account could be frozen immediately。 So before you do the YISIM。

 we had to write the original device， M-E-I in our 4G device。

 So we must fund the 4G device that we can change M-E-I such as unlock the 4G module development。

 board， modify it for the LUTER or MTK platform。 We got another available YISIM。

 which I took it off the board， because if we connected with， jump wires， which works easily。

 Finally， we managed to connect to the 4G carrier network。 On the LUTER system。

 you can see that it got an IPv4 address。 It belongs to the carrier's internet。 However。

 the carrier internet has a security policy。 The YIS are isolated from each other。 Therefore。

 we are not going to scan for internet address， because that is almost no help for。

 us to discover back-end servers。 We need to release the server to alarm。

 We can use this YISIM to access the internet。 And as soon as we send a GPT request。

 the browser returns a page from the China Unicom's， shop， but it doesn't matter。

 It just means the account is auto for credit， but we can still request back-end servers in。

 this situation。 There are so many fails we got from the system。

 It is too heavy for us to do what's engineering one by one。 Therefore。

 we use a firmware to encode yet another firmware analysis framework， which。

 can help us analyze all the failures' relationships in the firmware。 First。

 we use the domain of the leaked URL that we found from the UI blog before to find。

 fails related to the domain。 In the left picture， we can find some configuration fails with this domain。

 In the right image， we can get more URL information that is helpful for further pedestrian testing。

 For the related URL， we can do reverse engineering or authentication process and the head unit。

 pairing process to find the potential vulnerabilities。 We can request this URL we found before。

 since the GSP using mutual HTTPS， so we need， the client certificate， the certificate， management。

 library loads the certificates。 The infrastructure process application will request back-end。

 We found some key pairs and new certificates， and we found some PFX file， which is encrypted。

 We need password for the PFX file to get client certificates。

 We analyzed this library and we found that we all load the PFX and password file out。

 the real-estate booting， the application load certificate according to the market area。

 We found three types of certificates。 This library can decrypt the password file。

 It's using the AES256 CBC other reason。 The AESAV and the key are hardcoded in the ELF file。

 So we decrypted it with open SSL and got a PFX password。 Finally， we loaded it to the browser。

 You can save it。 They are available。 The certificate 00601 is for China market。

 And we found a social media plug in front of the head unit。 When you open first-time。

 it shows QR code and pull back and repeat the link。

 Then you can use your application to scan this QR code and the head unit will receive， a token。

 Then it will request a social media application to get user data。

 Then send this user a winner URL to the back-end because the back-end doesn't know why the。

 winner URL is credible。 So we can use this one-ability to read service local files or request the internet service。

 The TSP communicates with hermas by using the Daimler's their own protocol。 The support T。

 SAP trainer also supports the SMS channel via the network is terrible。 When the cost starts。

 the hermas wakes up and send their request to the back-end， in an。

 encrypted channel with local shared key。 The shared key are different among the workload。

 The shared key are dynamic and changed in time。 The hermas exchange and send the new key to the TSP secure channel via a car owner。

 send， their unlock command from the Mr。 CME application。

 It requires the user request to the back-end car control API。

 If the hermas is used in the sleep state， the back-end sends SMS for waking up， then the。

 back-end establish communication with hermas。 When hermas receive a car control command。

 it checks the car state and decide whether to， execute。 Then it sends the result to the back-end。

 That is the full process of the car control data stream。 For authentication and communication。

 a technical issue is not a good idea。 So we try to research the FMRE channel。

 We disable the hermas's cellular network， then change the platform number to our own， phone number。

 We can communicate with hermas with our own phone。 There are basic 64 encoded ATP data。

 This judge is followed。 It is secured due to the SHA256， HMAC and AES256， encrypted。

 Every message is a count。 Even if it doesn't use the encrypted data。

 we are not able to temper data or replay it， to hermas。 It seems that the mechanism is perfect。

 but we still find a point that we can exploit。 The back-end internal service has no authentication mechanism。

 They are fully trusting the service in the internet。

 If we can get access to the internet behind the back-end， we can work almost all the service。

 in the back-end。 After a long-term analysis， we discovered the summoner bill is in the back-end。

 And we found the most important remote service。 It can open the door， start engine， open the roof。

 start a close roof。 But the engineering survey is limited because it is a very valid server。

 That you need to pay about 450 bucks every year。 Besides， it has a SATA electric fence。

 The service is based on FBS4。 When you want to start engine。

 the back-end send request ask her hermas for challenging， code。 And then the back-end ask her。

 "them internal service to calculate the SBS target， the， algorithm is installed behind the back-end。

"， We don't know the detail about the algorithm。 By the way。

 not all the customer this server and it depends on whether the demo database。

 started the FBS4 data of your car。 Currently， we can only start engine successfully in Class A and Class E。

 Let's summary our research。 We followed the responsible disclosure policy。

 And when we found this vulnerability and the way we can achieve our control target， we。

 reported our funding to the MESNED security team first time。 In our research。

 we found about 19 vulnerabilities in MESNEDED's BANCHS car and compared them。

 to a tech chain to remotely control the car。 The key impact is that we can send the remote service command to the car。

 However， we didn't go too far。 We were getting more access to more important servers。 And yes。

 we did see many security considerations in the architecture of the vehicle。

 We spent so much time to bypass this security mechanism。 Regarding to the vulnerabilities。

 our access vulnerabilities will fix that data。 The rest of the vulnerabilities are being fixed by MESNEDED's security team and their vendors。

 Okay， the guy will take over from here to introduce their instance response。 Thank you。

 Thank you very much。 And thank me Nrui and all of the amazing team at 360 for providing us more details。

 Now we were able to see fleet backups in action。 Now why do I say it's fleet backups in action？

 Because once we got from 360 group the responsible disclosure through our website， through our。

 email box， then we were able to kick in all the incident response procedure to treat this。

 as an actual incident。 Following standard steps， first we initiate the incident response teams。

 So this is mobilizing teams， professionals and operationals in order to do the investigation。

 understand where we stand， engage in direct communication with the researchers from 360。

 analyze and prioritize what are steps。 With the goal that within 48 hours we are able to fix all the immediate vulnerabilities。

 just making sure that they can't be a potential customer impact with these vulnerabilities。

 After performing the investigation， we start working on selective blocking of servers， ports。

 maybe even services that will on one hand make sure that no hole remains open， it can。

 be exploited and on the second end we have minimal to no customer impact。

 So we are able within 48 hours to contain and make sure the vulnerabilities can be exploited。

 and start working on immediate fixes so you can bring the services back online。

 Then we have in parallel the forensic teams， forensic teams looking at all the data of the。

 recorded data just to find that there is no evidence of other attackers， not beyond the。

 partners from 360 that were able to use and exploit the sample vulnerabilities in the， past。

 So to make sure that we find no evidence of a potential real life breach and develop the。

 long term fixes。 Long term fixes that can be fixed to the car so within a few days we are able to bring。

 all the services back online while we remain completely confident that the sample vulnerabilities。

 can be fixed and thanks to the partners of 360 were able to do that with direct communication。

 with the researchers。 Then we have like any operational activity， the lessons learned phase。

 What's interesting here is that we are not only talking about technical lessons learned。

 of the vulnerabilities themselves。 Being look at all of the phases that allow the researchers to reach their goal to make。

 sure that we put hardening measures in place so it will be even harder for a researcher or。

 attacker in the future to try and walk similar path even if not the same one。

 Of course the vulnerabilities themselves can't be fixed。

 And like every operational activity we look at how we behave， we look at how we respond。

 it so we can provide better security next time something happens。

 And we are proud to say that we managed to work with our KPIs。 I have two messages。

 key messages with giving this presentation here to this great community， of Black Hat。

 First message is of course security operations or fleet security operations。

 Hello in securing the car but securing the fleet。 I guess it will be true for any IoT system it's more true for cars。

 Second is the work with the community。 We have here an example how a strong research community working with a strong industry can。

 bring better security。 And this is a concrete example of our strong security research group。

 Working with a car company capable of responding and engaging in communication is providing。

 better security for the customers。 Thank you for coming to this session。 I hope you enjoyed it。

 As I said in the beginning， I hope that we'll be able to meet face to face in a more normal。

 kind of Black Hat maybe next year。 And please enjoy the rest of your sessions and keep healthy。

 Thank you very much。 Hi， hello everyone。 Thank you for taking the time to join this session。

 We were very keen on sharing as much information as possible beyond what was shared on our stay。

 And I want to thank the partners from 360 for joining this。

 We don't have much of this live session so I will pull the next to me in Roy to say thank， you。

 Goodbye。 Thank you everyone。 Thank you for listening our talk。

 The way my team is doing the research on connected car survey， yes， and Mr。 Spence is a very good。

 company and they are very care about security and they are very friendly to the security community。

 Thank you everyone。 [BLANK_AUDIO]。