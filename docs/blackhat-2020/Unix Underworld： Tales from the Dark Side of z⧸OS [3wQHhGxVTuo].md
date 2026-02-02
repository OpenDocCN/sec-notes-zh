# Unix Underworld： Tales from the Dark Side of z⧸OS [3wQHhGxVTuo]

Wow， welcome， everybody。 Thanks for coming to our talk。

 I know that we are the only thing standing between you and lunch for some of you happyappy hour。

 And so I appreciate you being here。 We'll try to keep it interesting and on time。

 So if you followed filler I ever about any of the talks we do， you know， we talk about mainframes。

 We talk about Z O S and mainframes。 and this one is no different。😊。

But the goal of this talk is really to get you to feel like you maybe actually know something more about this than you think you do。

 A lot of times， the， the idea of learning this whole new archaic。

 complicated platform can be a bit much for people to take on。

 And so we decided to build this talk because there is a component of。

 of Zs that we're gonna talk about today that is the Unix subsystem。

 And a lot of you probably already know。A lot more than you think you do about hacking mainframes or testing or security on mainframes because of this subystem。

So I'm Chad Rickensro。 I'm a software security researcher。 I work for Brocom。

 and I find vulnerabilities in mainframe software。 I grew up at 90s hacker kid。 Basically。

 what that means is I didn't like authority and people telling me what to do。

 And I wanted to try to get things that I shouldn't get all that kind of stuff。 reverse engineering。

 I like doing this stuff。 I've been doing the mainframe side of the house now for a little over 10 years。

😊，So my name is Philip Young。 I'm the director of mainframe pen testing at Netsby。

 Most people know me by my other name， soldier of Fortrann。I， too， was a 90s hacker kid， shocker。

 I know we look exactly the same。 So Im also aframe cyber mainframe security enthusiast。

 and I've been doing this for about 10 year。 In fact。

 we met this is our 10 year anniversary being on stage together， doing a talk。 I know。 I know。

 Thank you。😊，And that's it。 Thanks for coming。 Yeah。

 We'd be remiss if we didn't mention one other person。

 There's three people on this slide and two people on stage。

 So Mark Wilson has been doing this a little bit longer than both of us， he didn't。

 he couldn't be here today because we didn't invite him。

 But he really started doing some of this work before we did。

 And when I started wanting to get into it about 10 plus years ago。

 the only prior art I found anywhere was Phils and Marx。 And so just。

 just by way of calling it out because where we， where we were and where we are now is vastly different in terms of the number of people that are into this。

 Yeah so。😊，One of the nice things like I said， we've been doing this for 10 years。

 So we work closely together。 We call each other besties。 And I am terrible at code。

 And Chad is amazing at code。 So when I write some code up， I will send it to him and be like， hey。

 this is crashing。 I don't know。 why can you， can you take a look。

 And then the first thing he instantly does is puts me on blast on Twitter every single time。😊，Yeah。

 I mean， if you have fill right code， I can tell you two things。 One is it probably will need fixing。

 And 2， it'll be pretty， right， So you see like all the little ASI art on the bottom there。

 that's fell。 It's gonna be gorgeous。 most I immediately remove all of that。 And then。

 and then generally fix it。 Alright， take a look at this photo。😊。

Tell me where in this picture is the mainframe。Unless you work for companies that have made friends。

 No， yell it out。 I can't。 We can't see any of you。 So someone's gonna have to yell it out。

Without that much time。No， no， you can just do next live。 Well， that's the mainframe。

 That part of the， the， the room， that's the mainframe。 It's the main frame full of CPU。Right。

 the rest is just input and output， right， the card reader terminal。 Okay。

 so that was like the original thing Today， it looks like this。

 And that huge room like device now fits all in one rack， right，1。

1 rack And something like this right now is processing billions of dollars worth of transactions somewhere。

😊，Tllions even Yes， I was just chatting with somebody right before the talk。 You know。

 you are all using this platform， whether you know it or not， whether you like it or not。

 multiple times throughout your day， if you're participating in society in any way shape or form。

 Financial institutions， governments， large retailers， healthcare care， airlines， shipping companies。

 All of these things depend on this platform。 The fact that you don't hear about it all the time is really just kind of a kudos to the platform itself。

 because it just works。 It runs。 It's highly resilient。 It's tremendously productive。

 and it just does what it does。😊，So for the purposes of this talk。

 I'm gonna just mention a couple terminologies here briefly， and we'll get into it more later。

 Raef is an external security manager。 So it is， you can think of it kind of like the active directory。

 right， It controls your authorization， your authentication。 If you get a mainframe。 If you get Z S。

 You don't have to use rack F。 There are a couple of other commercial E S Ms。

 A F 2 and top secret are owned by broad common rack F is owned by IBM for the purposes of this talk。

 We're mostly talking about Ra F。 but it probably applies to all the other E S Ms as well。A P。

 F library， the authorized program facility is。An operating system control that decides whether or not your process can achieve super user privileges on at a CPU level。

 right， We'll talk more about that later。 But just author or AF authorization means that your process can elevate its privileges at a CPU level and start doing things like reading and writing memory special operations are system attributes that your user could have within rack F。

 And if you have those then your power within the Raf is kind of unlimited。

 You can pretty much do whatever you want to do。 And then the other thing well mention by way of terms here is key0。

 So all of the memory on the mainframe has a memory key associated with it。

 And whatever all the processes also have a key associated with them generally speaking。

 you can read and write memory in the same key。 So like user key key8 you can read and write that there's a special one called key0。

 which lets you read and write any memory。 So we're telling you these because we're gonna start displaying some attack paths along the way。

😊，Will make more sense as we go。 This is a slide I used from a talk about 10 years ago when I got into this。

 And these are the kinds of things that I was regularly told when I said， hey。

 I want tona start applying some of these， you know。

 who's testing this system And I want to start applying some of these attacks looking for some of the same kind of vulnerabilities on this platform that I see in X 86 that I see an arm and it's not possible。

 There's magical， fairy unicorn juice that keeps that from happening。

 I don't see this is much anymore。 the word is getting out。

 we're slowly but surely changing minds and hearts about this。 But there's still some。

 There's still some corners of the world who think that this thing is just cannot be hacked。😊。

So by way of attack pass， this should look familiar to you if you do this on another platform。

 if you do it on mobile or arm or X 86 or whatever。

 these types of attack paths are not unique to the mainframe。 Now。

 some of the specifics are gonna be unique。 but the point of this talk is to really tell you that you know more already than you think you do about hacking security or pen testing or whatever on this platform。

 network attacks， network attacks our network attacks we use TC P I Web apps。

 all this kind of stuff on the platform works the same way。

 file system attacks inec data sets and files， same thing as any other platform。

 Your external security manager， right， just having things miscongured or having the security not set up correctly just like any other platform。

 And then number4， which is where we're gonna spend most of our time here， Z Uniix。

 So you'll hear us refer to the Uni subsystem in Z by a couple different names because it's changed names over the years。

 And in various corners it's still called these different names。 Uniix system services。

 O M V S or the newest kind of iteration Z S。Uunix is all referring to a Uniix like interface to Z O S。

 It's not a container。 It's not its own operating system。 It is a Uniix。

Like interface to the operating system。And for me， it was very much a gateway drug。 right。

 It was how I got really into it because I felt familiar with this。 Like。

 I knew how to operate within。Uunix， and I was comfortable then learning other concepts about the platform by just starting inside of Uniix。

 right， You've got shells。 Now， those you might notice are not maybe your favorite shells。

 but there are shells that come with it that you know how to operate within。😊。

You've got your hierarchical file system starting at your forward slash route。

 So you can see things like directory toveral and path manipulation attacks。

Your file system permissions work the same way。 You've got your three sets，3octets， read， write。

 execute bits， just like you know how to do in Linux and Uni。

The E SM can provide more security above and beyond this。 so that can complicate things a little bit。

 And it also can make it less secure。 How do we access it？ Well。

 if you already have access to like T S O IPF， the green screen side of the house。

 you can use O S command to get a kind of shell prompt。 SH is the most common。

 And that's what we'll do here。 it's the easiest way。

 everybody knows how to use it and it supports S H just like you would S H into any other system。 or。

 you know， if you know already a lot about MS and you know how to do JCL and batch processing or whatever you can build all these kinds of things and scripts and run it that way。

 we can access datas from Uni system services so we can copy and move data back and forth between MS datas。

 So when you look at this just as an example， just running some random commands and Uniix。

 it should look familiar to you A lot of this， you already know how to do。

 You already know what it means。 You know how to navigate it。

 And we just have to add a few things to your knowledge based in order to get you kind up to speed。😊。



![](img/60bd439152ccb7d7588e8c1f6bd70d0d_1.png)

Alright， when we structured this talk， you know， we wanted to， it's really a about awareness。

 We want you to know that these things exist。 We also want you to sort of show you how easy it is to kind of hack mainframes。

 It sounds like you hear about in movies。 So， you know。

 multiple tools exist today to do things like enumeration， right， This is， this is sort of like。

The thing that you have to do the first part of your pen test。 And so， you know。

 we have multiple tools that exist today。 So Enum is a Re script that runs。

 a lot of our tooling is gonna be written in what's native on the operating system。

 The newer versions come with Python， But the older versions do not。

 So you cannot rely on certaining language。 So everything we write we write in Rex。

 or Java or shell scripts。 O Enum is based on Ly Enum， If you've ever used that before。

 but it has some other fun stuff mixed in。 And then Zshog is if anyone truffle hog。

 It's the same thing， but for Uni file systems。 and we try to find secrets。 And the last one is。

 I'll talk about that in a bit。 So all of this tooling。😊，Is open source。And it's freely available。

 You can go get it right now if you wanted to， right， That's like。

Our philosophy is like we stood on the shoulders of the Gis。

 We want other people to stand on our shoulders， right， like this should be free and available。

 and people should be able to use it， right。And so if you've been paying attention。

 you might have seen a file there called all dot JC L in the Github Repo， right， What this file does。

 getting files onto the mainframe during my pen tests can be very hard sometimes， right。

 And you'll see one of the challenges we're gonna have in a second。

 And so it's it's better if you package everything up before you try to upload a bunch of files。

 So this， this， this J CL will actually upload all of our scripts。 It will run them for us。

 It will do all that stuff。 right， problem is。😊，Most mainframes speak Eodic。 Now。

 you can upload files to Unix using SP right， or SFTP。 But S CP problem is， if we use SP。

The files are transferred in binary mode， right， It doesn't do any ask the Esodic translation。

 And so when we need it to be in Eodic。 So we got to do the conversion first before we upload the files。

Then we got to， there's some code page shenanigans going on。 So we got to get rid of some。

 We got to replace some bytes。And then we can finally upload the file。 And then once。

 once we S S H back in， then we can just run the Unix command， submit all that JCL。

 and it will take care of all of it for you， right。😊，Now， if you're looking at this。

 this is how we're gonna like， for the rest of this talk。If you， if you see the blue prompt。

That means we are in Linux。 That's our staging area。 That's where we're building our tools。

If you see this prompt， that means we're on an I DM mainframe。 Okay。

 that means we're in the mainframe。 We're S SH in。 We're doing some stuff。Here's Enum dot Rex。

 This is the Re script that runs。 It gathers information in memory， sometimes。

Information that we normally wouldn't have access to， but it's in memory， right？

 It has a whole bunch of other commands。 So here it's gonna display the security information。

 We're gonna go into more detail about what it actually found later。 O， M V S Enum is like I said。

 it's based on Ly Enum。 It also gathers a lot of information。

 It's quite noisy right because it's just the shell。

 It's written in seahell because that we know that's gonna be there But it's， it's quite noisy。

 It does all kinds of of checks。 it'll check your access rights within F。

 It does all kinds of stuff on top of Lyn Enum to help you profile the system from within Unix。😊。



![](img/60bd439152ccb7d7588e8c1f6bd70d0d_3.png)

![](img/60bd439152ccb7d7588e8c1f6bd70d0d_4.png)

The last thing is but during enumeration is we try to break out of the environment。 You would not。

 You' would be surprised how many times I can go from the mainframe to an AW S instance on multiple ports at a client where we。

 we did that。 We did， We did exactly what I'm about to show you。And they were like。

 we haven't used that ISP for a decade。Right， so that's the kind of。

 that's the kind of fun stuff that we're finding。 So you can see here I run Port scan。

 Port scan was actually written by Owen Hey， a buddy of mine。

 And I just run Egress Tester on a different box。 And you can see all the ports that get connected in。

😊，So let's go through some of this and talk about what we find after our enumeration of our fictitious system here。

Looking at this enum script a couple things I noticed right off the bat。

 So first thing it's gonna to tell me this is the Raf database and it's backup。

 and also another interesting bit in there is that the KdF AE S encryptions are not active。

 So there's two different ways Raf can store its passwords。

 One is using a legacy algorithm that's based on Des and M5。

 And then there's a newer version of it that uses like AE 26 and iterations and whatnot。

re both know they both can be brute For using John the Ripper or hash if you get a copy of the database offline。

 the De based one is much faster and can do tens of billions of guesses per second where the Kd AE S1 will slow you down a little bit。

 The other thing I want to mention is these scripts are pulling this information out of memory。

 So there are control blocks， data inside memory and every address based on the system that has some of this basic information in it。

 And it's documented but we've written the scripts to pull that information out of there versus running commands that might generate the same information。

 And that's because you might not have。😊，Access to those commands。

 And if you start running those commands， there's a good chance you're gonna start creating log entries。

 and it's going start looking like somebody's doing something。

 But if you pull the information directly out of memory， there's no downside to that。

So looking at this just a couple of other things from this script output。

 Phil's gonna talk about this in a minute。 We can ask a route without a password。 Okay。

 so what does that mean， hold on to that thought。 We see some some privileges。

 If you look at un privileges here。 We've got read， write， execute 7，7，7。

 right so anybody can read or execute or modify these files。 You've got a shell script in bin。

 and you've got an in net D dot co。 There's a real life attack。 We can do that。

 we can do privileged mounts。 So this is another attack we can use to escalate our privileges。

 which I'll talk about in a second。 And we can successfully issue this X adder command。

 So this goes back to ApF authorization， and we're gonna also dig into this one。 But basically。

 this lets us create ApF author programs inside of Uni。

 then we can build We can build a privilege escalation based on that。😊。

If I look at the output from Zshog， Zshog is going through your files and looking for passwords or usernames or other kinds of tokens or interesting information。

 we see this all the time。 we see like a D do properties file or something like that might have a username and a password in it or a Java properties file might have a password in it。

 And this tool just go through and find anything that looks like it。 And then from that。

 you can test it and see if it。 Yeah， So now that weve figured out what is wrong we've done our vulnerability assessment essentially。

 Now we need to start demonstrating impact。 you would not believe the amount of pushback you get in the mainframe space when you have a finding sometimes。

 So you have to really demonstrate you did exactly what you did。 So the first one。

 obviously the easiest one is the store credentials。

 right This is from an example that happened like live during a pen test don't don't store credentials in files。

 If you have to do that， make sure the permissions are right。 permissionmissions on Uniix。

 like obviously if if you run Linux， you're very familiar with permissions。😊。

But a lot of the mainframers， they've been doing mainframe for 30，40 years。

 They're not as familiar with Unix permissions as we are。Right。

 and so they say they oftentimes get around。 So， for example， this was the program， you know。

 modified for this talk。 And you can see right in the file are user credentials for me to use。 right。

 And so when we， when we pull those out right here。 The problem was this file。

 the owner of the file had C H moded this triple 7， They had made it world readable。

 writeable and executable。 So that means anyone could read right， the script didn't work。

 And then they change the permissions。 And now it works great， right， So then obviously， look。

 the easiest thing to do is which is SS H。 Sometimes mainframe hacking is really easy right。

 Like everyone thinks it's super hard。 This is the easiest thing to do， right， like。😊。

It's not that hard。 So here you go， I'm gonna S H And。

 We don't have to like wait for this whole video to finish。

 like a lot of a lot of what we're doing is is is taking tactics and tools that have worked for years on other platforms and just tweaking them。

 Yeah， so that they work here。 But here you can see after whoops， whoops。

 I fast forwarded too quickly。 O， there's another demo that has the exact same thing。

 I'll talk about it then， okay。😊，If we look a little bit into this last one here。

 we can issue this command。 What does that mean。 So I'm gonna dig just briefly into AF authorization and what that means。

 And we got to start at the CPU level。 So Z architecture is a different CPU， It's not86。 It's not in。

 It's not arm。 It's its own CPU for this platform。 It has its own instruction set。

 So there are a number of as instructions that are CPU instructions。

 There's the majority of those instructions are meant to be an able and to be run by any process that's in like normal state。

 This is called problem state。 There's a reason for that。 It's because it's where you solve problems。

 right， But that's normal operating state。 And then there's a small subset of CPU instructions that can only be run at supervisor state。

 which is like the privileged level。 So you can change between those states。

 any process can change between those states。 The privileged level instructions are the ones where you can change than your memory key and do things like read and write any memory。

There's an operating system control now there's an operating system control that dictates who can make that change。

 Which process can make that change。 And most of that on Z O S is done through AF authorization。 Now。

 on the green screen side of the house in in M O S side of the house。

 that is done with a series of libraries。 So there's a list。

 the AF authorized library list and a list of libraries， which are like folders for programs， right。

 So if you have a program in one of those libraries。 Or if you can add a library。

 you control onto that list， then you can run an authorized program and switch your memory key and all that kind of stuff。

In Unix， there's no concept of libraries or whatever。 We just got folders and files and whatever。

 So those programs get AF authorization by flipping a bit。

 There's a bit that gets assigned to those programs。

 And the way you flip that bit is by running this command。 So the command is X adder。

 And then with the plus a with the plus a flag assigns that AF authorized bit to that program to be able to run this。

 you need read or better access to a profile in your E SM that is B P X do file adder dot A。

 So when you're doing your enumeration。 If you find that you have read access to this。

 you can probably run this command。 And that way you can then build an exploit AF author it and run it。

 So what does that look like。😊，So I'll walk you through this。This is an asler。

 this is an asmbler macro mode set macro。 and its sole purpose in life is to switch your processes state between problem and supervisor and back。

 And key。 there's 15 memory keys 0 to 15， but the special one。

 the one we're interested in is that's 16 keys，16 keys between 0 and 15。

 But the one we're interested in is 0 because 0 has a special capability of being able to read and write any memory。

 So we want to be able to execute this instruction。

 The operating system will prevent us from executing this instruction If it hasn't been issued from an AF authorized program。

😊，So since we're dealing in Uniix， we're gonna talk about what that means。 And then what would we do。

 Because just because we can get to the point where we can read and write any memory that still doesn't give us the ability to like take over do whatever we want to do on the system。

 we still have the E SM to contend with because the E SM isn't checking to see if you're in supervisor state that's a CPU level control But the E SM is checking memory EM is checking memory to see what privileges you have and are you able to do X。

 Y and Z。 And one of the places it checks these privileges is this data area called an A。

 which is really just like a cache of your EM privileges。

 So when you log in the system builds this control block in your address space。

 And it has representations of a lot of the major privileges that you have。

 And so when you try to do a thing。 the system checks this。

 So one easy way to take advantage of getting key0 through your AF authorization is to generate another A tell the system to generate one。

 And this is a privileged operation， you have to have。

You have to be in supervisor state to be able to do this。

 But have it generate a new AEE for some other user that has the privileges you want。

 So we're spoofing or impersonating that user。 So in this case， you can see， you know。

 I've got Chad Brocom whatever this is low privileged AE。

 But because I was able through AF authorization to get to be able to run an exploit。

 what I can do is I could generate a brand new AE for Phil because I know that he has the privileges I want。

 And then I can just point change1 little address and point in my address to the new one that I just built It's still gonna look like me logged in。

 everything still gonna my address was still be owned under me。

 But now when the E goes to check privileges， it's gonna check where this pointer goes to And it's gonna go to this new AE that I built。

 So what does that look like。 So this is this is a snippet asmbr code that kind of shows you that this is is fairly easy you understand what's happening here。

 So the first thing that happens is we're gonna execute this macro。 This is the test。 if can。

You can get through this if this works you're golden。 if you can't。

 if you didn't do the AF authorization correctly or something else is wrong， this the。

 the program willend will stop right here。 You'll get an047。

 and that will be the end of your program。 If you get past this instruction。

 Then everything else that happens here， you're golden。

 The next thing we're gonna do is we're gonna generate this A E E using a rack F macro called Rar。

 And we're gonna generate it。 we basically pass it a few parameters。

 But the main parameter we passing it is literally just a user name。 So down below。

 you'll see I've got Phil as my user name there。So it's gonna generate as if Phil logged in this new data area。

 And then I will go through the trouble of repointing my pointer to mine to the one that I just created。

 So how do we build this up。 So we write this assembly code in a text file in Uni， we assemble it。

 This might look familiar because these commands are very similar to commands you would use。

 if you were doing this in Linux or Uni， right， we assemble it。

 we link it and then we run that one special step here where we AF authorize it。 If that works。

 your golden， right， If you can run that last command without any errors popping up。

 then you've APIF authorized it。😊，So the next thing you would do then if you wanted to use those permissions。

 like， so I'， I've become Phil and at least for purposes of my privileges。

 So Phil keeps all his secrets in this one file called secret text because he's good like that。

 So I'm gonna run my exploit。And for purposes of security， I will become Phil。

 You can see that effective user I D when I did an I D。 So now the system will treat my。

 my privileges as if I'm Phil， and I can go in and see what kind of secrets that Phil keeps in his secret file。

thanks， Phil。 Love you ready。

![](img/60bd439152ccb7d7588e8c1f6bd70d0d_6.png)

Alright， this， you've probably seen this one previous already， right， You， oh， you can suit a root。

 What does that mean， I understand that， So technically， in in a Linux environment。

 this would be game over， right， like you've taken ownership of the entire box on the mainframe。

 You're almost there。 You're so close。But you all you really can do is manipulate the Unix file system。

 right， because what happens is your， your address base and memory that that A C E E， all that stuff。

 that doesn't change。 Only your Unix U I D changes as you're doing commands。 So what is that。

 What does that look like in practice。 So here you can see， I'm Phil， I type the I D command here。

Right， very normal uni commands。 I type Sue。 So I automatically。

 I don't have to provide a password because we're part of that。 we have access to that profile。

I type I D。 You can see I am a different user， like in the Uniix system， right。

But when I go to run the T S O command list user， it's L U。

 There's every command is an acronym on here。 So it's list user。 When I type the list user command。

 it still says that I'm Phil on。 So， yes， I might be U I D 0。 I might be super user。😊。

But if I try to do anything outside of Unix， it， it's's still just Phil doing those things， right。

So why don't we just do the S， S H thing again， That's the easiest thing to do。

 Just find a user you know， as an admin。And then do create an S， S H key that you can make。

 So that's what we're gonna do。 So we're just gonna suit a route。

 We're gonna see they don't have an S S H folder。 So we just make one。

We also then make unauthorized keys file。 We own it。giveive it the right， the right permissions。

We put our public key in there， and then we're ready to go。And on the Linux side。We is SS H。

And this is what I was trying to show in the last demo。 But thankfully， it's here as well。😊。

If you look so， So now we're running as markers。 So now we're logged in。

And Mark has what's called attributes special in operations。

 You heard Chad talk about that much earlier in the talk。At this point， it is game over。 Like。

 we can do anything we want with this， with this account now， right， So。

 so usually what we're doing when we're doing Penta is we're targeting other users who have this or APIF authorization。

And it's gonna look like Mark did it right because we're operating under his idea。

 Let's talk about this mount idea for a second。 So if you think about any time you've operated on a Linux system。

 you understand somewhat， at least probably the idea of mount points You might have multiple hard drives and each of those hard drives has a partition And then those partitions get formatted and then you mount them at a certain point。

 you've got a mount point slash boot and it slash root and maybe another one slash user or var or whatever partitions on a hard drive。

 So Uniix system services works very similarly。 But instead of using hard drives and hard drive partitions。

 it's actually a Z data set that kind of acts like that partition。 So you create a data and Z。

 you formatted。 And then you can mount that data to a mount point。

 And that's how you get different mount points for capacity or products or whatever you have。 Well。

 the AF authorization for those programs is stored inside that data is a bit。

 It's not stored in E it' stored as a bit inside those data sets。 So here is the attack。

 If we have the ability to do what's。Call a privileged mount。

 meaning we have the ability to mount one of these file systems in a privileged way。

 So there's a nonprivileged mount and a privileged mount。

 Noprivileged means you can mount it and still look at everything in there。

 but the system won't honor any set U I or AF authorization bits that are set regardless of set the system just won't honor it。

 So that's a lower level。 If you have this higher level of secure mount privilege。

 meaning you have update access to one of those two profiles。

 or you can S you or somehow get to super user I D 0， which also has this privilege by definition。

 Then you can do a privileged mount。 So what is this attack looks like The attack looks like this。

 So I would go back to my lab and I would build the exploits that I want to have on on a small Z FS file system on my lab。

 And then I would create the programs。 I'd AF authorize set U I them， unmount them， package them up。

 carry them with me， go to the target system， The client system， whatever I'm testing， upload it。

Their system receive it and mount it and then run the tools。

 and the system will happily run those tools with AF authorization that I AF authorize on a completely different system。

 right， So if you， you can run all these commands manually。 But this is just showing you in JCL。

 it's fairly straightforward。 Once you get the data restored。

 It's actually taking more time to figure out how to get the data offloaded back uploaded on restored than it would。

 then once you run the mount one command mount。 And then you point it at the。😊。

The ON D S directory that we created， go out there and I'll look at it and make sure it mounted。

 Okay。 You'll notice a couple of things about this。 When I list it。 First of all。

 I'm using a capital E switch。 That capital E switch doesn't exist in Linux here。

 it means show me the extended attributes。 So down in the。

You'll see between the normalocts and the owner。 there's some other bits there that a。

 if that A shows up。 that means it's an AF authorized program。

 you'll also see in the previous one it's owned by U I D 0 and you've got the set U I D bits set there as well。

 So that program will run as U I D0。 And then we run those and the system is happily will happily give us the run those exploits as AF authorized。

 even though we built them on a completely different system。 Yeah。

 he makes it sound so easy Alrighty， So let's talk about buffer overflows for a bit。

 So lots of units programs are written in C a nonmemory safe language right， And for a long time。

 it was theorized that you couldn't do buffer overflows on mainframes。

 until someone prove them wrong。 So not me。 They're right here。 So basically。

 it's just like any other Linux environment， you have C programs and you can do bufferflows。

 But if the program is AF authorized。😊，Then we can do the exact same exploits。 We all。

 We've been talking about this whole time。 How do we find A authorized data sets。

 Very easy with this simple find command。 That's it。 The extended the extra attribute。

 a finds all of the data sets that are APIF authorized， all the programs， all the programs， sorry。

 All programs。 whoops， all the programs that are A authorized。 So this is one screenshot of like。

 it's scrolled for a long time， right， And this is on my lab system。

 where I don't have any vendor products or anything installed。😊，Now， look。

 we've only got like 8 minutes left。 I do not have the time to get in depth into mainframe buffer overflows。

 There's just not enough time。Thankfully， you don't have to worry about it。

 Other people have already done the research and have given talks about it。 So Jake La Bellll。

 who really is the the godfather of of like mainframe buffer overflows。

 gave a talk at Defcon two years ago。 Chad gave a talk at Defcon 10 years ago。

 talking about mainframe buffer overflows。 And then Jake and I also gave a workshop which is freely available on Github where you can learn how to write mainframe buffer overflows。

 right， now。😊，You learn how to write a buffer overflow。Then you all。

 then once you find an overflowable buffer and AF authorized program。

 it's just about putting some shell code together and exploiting the buffer。

 That's all it really takes。 So this is just， that's just the shell code from the exploit that I had back in the assembly stuff that I showed you。

😊，All right， let's talk about some honorable mention because we didn't have time for them all。

 improply using the E SM to manage security。 You can actually let it drive the security in the file system and they will ignore the permission bits and go whatever rack F or whoever says to give that access right。

 I was on a file system。 The permissions were set correctly。

 the EM was not And I read access to every single file， including private keys。

 This is hard this will confuse you because it will say like， oh。

 I should have read access to this file。 But if they have some certain features turned on in the ES。

 the E supersedes those permissions。 And so you might be looking at something like this is 77，7。

 but I can't read it。 What's going on。 That's your clue that maybe they've turned on some of EM permissions to override what's in the file system。

 It won't reflect it back to you。 So it could go either way。 It could be more restrictive。

 And in some cases， like said to be less restrictive。 Yeah。

 the other one is worldriable file and slash bin that was run every time a user logged in was a shell script。

 So I just edited it easy stuff。 This is not It's not super hard。I see this a lot。

 So I just wanted to talk about it here since I of the stage。

 But I see lots of temp logs that are world writeable on mainframes in Uniix。 right， Don't do that。

 Please don't do that。And the last one here is L F5， like like they still run Web apps， right？

 And we found a vulnerable Web app that allowed local file include to files we couldn't access in Uniix。

 right， So all kinds of things that you see here in， in like the Mux side of the fence。😊。

So itd be remiss。 And also， I know that IBM is in the audience。 So we had to throw this in here。

 like， how do we prevent and detect this kind of stuff， I mean， ultimately。

 the goal here for probably most， if not all of you。

 but at least most of you is to not have this happen because I like it when my bank card works。

 I like it when my airline is on time。 I like it when I can buy something online it shows up like7 minutes later。

 how do we prevent and detect this kind of stuff。 So prevention is a lot of what you think it is probably if you were doing this in a Windows box or Linux environment look at those file permissions。

 I mean， I have scripts I run that just look for world readable world readable thing you're learning in pen testing 101 on Linux check there are other profiles that are important there are classes out there。

 facility classes， Uni classes serve off classes。 But these particular three profiles up here are very dangerous I'm of the mind that none of this should be on anybody's daily driver I D Nobody should have the ability to。

As you to route without a password to do privileged file system mounts to have all of those file modification attributes that you can get from these on their daily driver。

 I D。 That should be a break glass scenario。 You just should not need to do that every day。

 because the more people that have these permissions， right。

 That's just a bigger attack surface for us if we want to startsing passwords or tracking passwords。

Looking at your logs， right， I mean， it's， it it's basic， basic stuff， right， We get S M S。

 the system messaging facility will， will kick out log log messages。

 Look for both successes and failures， because you might have somebody who's already ahead of the game and they're and they're succeeding at this。

 And you want to know， why are they constantly assing to root。 Why is that working。

 Why are they constantly AF authorizing a program in in O M V S in Uni system services。 Like。

 is that something that you would expect them in their job to be doing on a regular basis。

Looking for large numbers of unauthorized attempts on files， right。

 somebody scanning the file system will show up like a sore thumb in those logs。

 You should be looking for that。 And， of course， thousands of outbound TCP connections。

 Egress filtering is still really， really bad on the system for some reason。

 the number feelss not exaggerating。 The number of these systems I found that have a pass for being able to connect directly to systems on the Internet have open SMTP relays on them。

 what what could happen bad there， all of these things exist on these platforms。

 and you have to look for them and they're all in the log。

 like you can find all of this activity by looking through the through the logs。

 And then the last thing is Unix file system auditing。 Yeah I'll explain what that means。

 So the nice thing about Z Uniix is if you pass L this capital W here you can see it's now shown us a new field that we get。

 and it's all Fs， right。😊，So when so a colleague of mine David Bryan and I had to figure this out because we're working on a client to help them improve their modern capabilities。

 So what this means is is it's like two halves， right， when you have the user controlled side。

 So users can set。 if you can see H mod the file， you can see H audit the file， right。

 you can set like like， I want this file。 if people execute it， I want to know。

 then you have an admin control side， which the admin consent。 This is the default setting， fail。

 fail， fail。 So that means if you fail to read executor right， it will generate a log。

My recommendation is you go and find your privileged files that you really care about and change it so that you can have them be audited for every access attempt。

 Right， So now youd see it's all A's。 And that means all。

 So that means success and failures will get logged for read， write and execute。😊，Now， first。

 I want to do some shout outs because we wouldn't be up here if it wasn't for a bunch of people in the community。

 So first， I want to say thank you to the mainframe hacker community。 You can see there's some。

 some screens there from those folks。 But， you know， we when we started。😊。

There were two mainframe hackers，3， There were three。And then Henry joined us。

 And now we've got like a dozen mainframe hackers or more so or more。 the Mo discord。

 if you if you're interested a little bit about this topic and you think。

 I want to learn more about mainframes Moshax has a great YouTube channel also has a great discord where you can chat about mainframe stuff learn about JCL。

 all that kind of stuff。 the mainframe cybersecur community at share， is great。

 if you are thinking about sharers like the mainframe industry conference， you can go there。

 and they are super welcoming， they love having other cybersecur Ns to talk to about mainframes。

 It's great。 I just throw in also you know， the large companies that deal in this platform you know。

 especially I'll call out IBM， they have been very partnering they partnered well with us through this stuff because ultimately。

 they don't want to see the system get hacked on the front page of the Wall Street Journal anymore as much less than anybody does。

 And so they。😊，Really well with us over the years。 And we get to come up here and say the things that they can't。

 But it works really well。 You know， it works really well。 And I。

 I I appreciate all the support we get from those guys。 Yeah， so this is how you contact us。😊。

If you have any questions， we're gonna go to the wrap up。 Yes， apparently。

 we're going to the wrap up room so you can ask us your questions there。

 This is how you can get a hold of us。 And we want to thank you for having us come speak with you。😊。

