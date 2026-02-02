# Weaponizing Apple AI for Offensive Operations [UooCY59nQSQ]

Thanks for coming。 So today， I'll be presenting a topic about weaponizing Apple AI for offensive operations。

 right， So I'm a lead red teamer work for CB health。

 and I'm specialized in more in post exploitations and techniques that you know。

 usually differenters don't see。Right， I used to write blog。 and if you like my work。 And also。

 by the way， all my code p O that I'm gonna present today will be posted in my blog。

 And if you like to take a look at it。 you know， you can follow my blog。

 if you're in team space or AI researching and stuff， you can also follow me on LiedIn。Right。

So today， I'll be discussing more about Apple AI stack as an overview。

 like you know what all the frameworks are there and native components of Apple and the main component is core Ml。

 which is a legacy framework。 And we we see how we can weaponize that for C two operations and payload staging。

 And also there are other native frameworks called vision。 and。Foundation。Which is now built for Mac。

 And we'll also see how we can weaponize that for other offensive purposes。 right。

 I also built a framework called M L R。 It's a C2 framework， which。

Completely whats based on core M L。 And I'll show you demo how this work and how payload staging can be performed with MR and communicates completely with native Apple APIs。

Right and also， now， in case if you don't want it to use M R Ca C2。

 we can also use mythic Apple now Apple payload and see know how we can embed that payload into。

The M model and see know how we can weaponize that for offensive purposes。

 So all this will be demo as well。 and the code will be posted in my blog。And also， as an end。

 we will also discuss about direction blind spots and know how we can mitigate this from a defensive side of it。

Right。So AI stack overview。 So there are multiple components when it comes to Apple AI。

 The legacy component is in core Ml engine itself。 It's a lightweight shipped with iPhone's Mac and used for。

 you know， day to day normal activities been used by you know， iPhone photos and other applications。

 And also it's been used by third party applications as well。

 And there is a newer framework called Apple Foundation model， which is like。😊，A large scale model。

 which is used for other， you know large scale activities。

 but it is like closed private and it is used only by Apple。

 So well be targeting more on the corem engine rather than the latest one because this corem engine is still available on all the latest Mac。

 and it can be weaponized。On top of it， there are other frameworks that you see Vi。

 A V F and reality kit photos，3ri and all those things were there。

 Those are all native frameworks of Apple。 And I'll be picking up now 12 in this case。

 now the vision framework and also the A V F and see how we can weaponize that。So what is an M model。

 So M model is core M model。 So it's used by core Ml model engine。

 So if you look into Apple MacBooks。 you know， there are tons of Ml model files sitting on your desk which being used to buy different applications for different purposes。

 It is a lightweight model that will be shipped along with your applications。

 and it's designed for fast and on device executions。 and it is very lightweight。

 So it can be run on mobile phones。 So whatever that we want to discuss。

 it is still applicable on the mobile devices as well when it comes to offensive purposes。

The assumptions are， you know， the model file is not signed。

 So the gatekeeper or the notization does not check whether you know the model file or whatever your application is lowering is signed or it is secure。

 So that's why， you know， we'll be using that for offensive purposes。

So there is another format called M L model C， which is a compiled format of a model Ml model file itself。

 And that is also， if you look into all your Mac applications， most of the files you might see。

Dot Ml model or dot M model C， which is you know， it's a， it's a compilation of model file。

 That's it all these are in a nonhu readable。 So even if you try to read the model file。

 you need a core Ml framework to load and read the content of it。So what's inside on a model file。

 So there are multiple components involved。 and the code components are the model descriptions。

 which have your known names types。 What this model is all about and stuff。

 And the model type and the model parameter。 So the model parameter is the code parameter。

 which actually sets all your trained data will be sitting in your model weight。

 And there are like 10 to hundreds of model layers that you can use in that particular parameter。

 But you know， this is where all your payloads can sit as a model weight。

 And I'll be showcasing how I'm gonna weapon that as a model weight and see how we can。

Put a payload encrypted payload and see how we can bypass all those things。 And there are metadata。

 and I'll be demoing two things here。 One is how payload can be injected in model rates and another one is how payload can be injected in metadata of a model and see how we can load the model and execute the payload。

Right。So how M model works。 So basically， you know， when you're building an application。

 you wanted to use Apple AI for efficiency。 What usually the developers will do was they will use a model file Ml model file that they can build create and train the date train the model and ship it along with your application。

 compile it into a model C file or in a model file。

 and then you can ship it with your application that that's what you know when you try to load along with that the application。

 the model file will also get load and execute it in memory。So the next one is the vision framework。

 It's not an Apple AI stuff， but it can be used for sending data to Qml pipeline。

 So vision framework is used for， you know vision related activities like photo processing image processing and stuff and text recognition and it is a lightweight and very silent。

 it has its own native APIs for processing all the data。

 I'll be showcasing how we can embed data into pixel on an image and how we can extract those pixel data from using that vision APIpis。

The next one is the A Foundation。 A Foundation is used for audio video processing。

 There are many applications use AV foundations as after today like you know Zoom cap kit and IMovie there are much more applications A foundation。

 It is again another native component that in Mac and used for audio video processing。

 So I will be demoing on how we can create audio file and instead of putting an audio payload into a metadata or a description or somewhere else。

 we will be putting a payload directly in audio syn and then see how we can extract that using the A foundation libraries。

😊，So abusing each layers。 The first one is the model file， like I said you know。

 we can place payloads either in the description or metadata or in the model weights。

 each date each layers have different types of formats。 So for example。

 if you are placing your model if you payload into a model weights， itll be a float variable。

 So if someone try to read it， all they see is a float data。 That's。

And the descriptions has its own way of formatting。 Likewise， you know。

 we can place your payloads on every layers of a model file。

And if why because why no one able to read this because， you know。

 E D R our AV engine was unable unable to detect these kind of things because， you know。

 it actually need Q libraries to read the model file and extract the data。And the vision framework。

 So like I said， we should have its own set of Apis。

 which is used for reading extracting data from an image。

 and we'll be using visions libraries to extract an image read an image。

 extract the pixel data and put it in a readable format。

 So each component used for different purposes and well be showcasing everything on how we can it。

 So for example， when I say pixel， you create an image。 And you set the opacity like 15 person。

 So when you open the image that image looks like a readable image like you know it's not a corrupted data or something。

 you can still view the image。 But the the payload actually sits in the image as a pixel and for simplicity purposes for demo I set it to 15 person so that you know you can see what's happening what is sitting there。

 But you know you can also set it to like one person。 So it's not visible to human eye。

 So if someone try to open the image by default， it look like illit image and or you can inject the。

Lo into a logo file of your application and ship it along with your application。

 So during your runtime process， you can extract the payload from the image using this native API。

 and which looks completely legit。And the A Foundation is kind of something interesting。

 It's not a technography。 I mean， it's kind of stnography。

 but it's kind of a bit more advanced because what I'm doing is we will be placing payload as amplitude。

 for example， I have a payload like hello world or something。

 What I'll be doing is take the payload convert into a binary zeros and once。

 And the one represented as like1 do as an amplitude and zero is represented like 0 do2。

 So we is kind of an allowed1 do zero is kind of allowed and0 is like like mode。

 So when you play that audio it still plays in we see something whatever you play it。

 it still plays all you hear is a beep sound like ups and downs of beep sounds。

 But you can use a foundation as a framework。 read the audio file extract the payload and decode that and then pass it for further operations。

 So you can it's more like a technography。 but kind of bit of advanced because the audio file still look legit。

 It's not a metadata or something we are actually placing the pay。😊。

L intoto as an amplitude of an audio file。Right， I'll show a quick demo of Vi and A Foundation。

 how this thing actually works。2 so。Have have a Python script that actually uses Python libraries to generate an image file。

 So when you， when you run the code， it will just create a image。 And if you look into that image。

 it's like know a normal working image。 and it's not corrupted or anything。

 all you see is pixel data， which have a payload in it。 So in this case。

 Im hard codinging encryption key for my payload， which I'll showcase how I'll be using this。

 but this is just not an example。 like you know how we can place a payload or a key or something on an image as a pixel and how we can extract it。

 So to extract it。 you just use I wrote another script in Python which is use vision libraries。



![](img/7cdacd86ecf67221c0131a9ab300fc61_1.png)

![](img/7cdacd86ecf67221c0131a9ab300fc61_2.png)

To read that image and extract that payload from the image from the pixel。Right。

So this is how you can use vision to read and extract payloads during execution of your payloads。



![](img/7cdacd86ecf67221c0131a9ab300fc61_4.png)

![](img/7cdacd86ecf67221c0131a9ab300fc61_5.png)

So the next one is the A V Foundation。

![](img/7cdacd86ecf67221c0131a9ab300fc61_7.png)

So this I created a two swift code。 One is to generate an audio file。

 and another one is to read the audio file， extract the data。 For example。

 what the script will do is I have a code have a payload called hello world。

 And what I'm doing is I'm converting that hello world into a binary zeros and ones。

 So if you look into the code So zeros are representing as amplitude of02 and one the one bit is representing as one do0。

 So when you play this audio file， all you see is like a beep sound。

 but technically know it's not corrupted or anything。 even during the deep analysis。

 it youre still unable to extract what exactly sits inside the audio file。

 But through audio foundation libraries。 you can pass this file。

 So I have written on another code to read this audio file and extract the payload。

 So what this will do is it will。Read the audio file and convert。

 extract all the binaries and decode the binary and extract the nucle X data from that audio file。

 So the audio file look legit。 And it's not detected by any of the scanners。

 but you can still send literally like one number of payloads in a audio file and extract it through a engine。



![](img/7cdacd86ecf67221c0131a9ab300fc61_9.png)

All right。So the next one is an Ml Arc。 So M Arc is I wanted to test all this hypothesis。

 like you know how this M model itself can be weaponized for payload executions and C2 operations。

 So this is lightweight C2， which is written and Python。

 and it's used uses fast A for communication between client and C2。So instead， you know。

 Jason or D L L willll be using model file as a payload transmission mechanism。So what itll do is。

 So we have consider we have a C2 and an agent。When you so we have a dropper。

 like you know dropper is written in Python。 So when a C2 C2 is running and you run the dropper on one of the victim system on a MacBook。

 So as as you run it， itll go and register to the to my Mr C2 and it'll register the session。

 itll create a session for you。 And then if you want to send a command。 for example， know。

 I got a session。 and I wanted to send a command like know who am I or you know something from my C2 what my C2 will do with it will take your command convert it into hex encode it or or Zar encode it。

And then place it into model metadata。 It'll create a model file in memory and then place the payload into model metadata。

 convert that model file into binary and send the binary over the network。Right。

 so on the victim side， the victim received that binary。

Create recreate the model file from the binary。And use quol libraries to extract that hex encoder data and decode it。

 And then execute it。 And then what itll do is it will again send it back the output to the C2 again through M model。

 So again use quol libraries to recreate a model file by placing the output into the same way by vera and then send it back to the M。

 So M can use Q Ml engine to decode it， extract the output and display it to you。

 So that's how this M C2 works。So I can show a quick demo of how exactly this is working。Right。

 so I have a server which is written in Python。 and also the agent is written in Python。

So when you execute it the server， all it does is now it just listen for client communication。

 And then on the other side， I go to an agent。 again， it is written in Python。 As soon as you run it。

 it'll create a session for you。And go to go back to your Cus server。 and you'll see， you know。

 there is a callback beacon from one of the agent。And then， you know， to come connect to that agent。

 all you do is just connect and pass the session， I D。 And now you are connected。Now。

 if you want to send a command， like， for example， I'll be using L S iPhone L A as I input。

 And what's happening is， as in as you send a command to the C2， C 2 take this command。

Create a model， inject the data into model metadata and then send it to the server conent to binary。

 and the binary value will be send to the server。And。Sorry， send it to the agent。

 So the agent receives it。 Decode it and run the command for you and then take this output and again output put it in a model file using Qml libraries and send the raw data binary data to the server。

 The server again， like I said， itll extract that using again Q Ml engine and displayed to you。

 So this is completely this M C to is completely work based on on core libraries。

 and it is completelyth It's being tested against later speed and A engines。

 none of them been caught。All right。Okay， so in case if you don't want to use M MR as a C2 because there are hundreds of C2s as available as of today。

 and you don't want to use MR， which makes sense。 So the wellknow C2 for Mac is Myic and appel is one of the wellknow payload。

 So if you run appel as a standal alone like you know appel code it'll get detected by latest Es。

 So to bypass that what you can do is you can take the appel code。

 So what I'm gonna do now is I'll take the appel code and encrypt it using Z。

 I will place the Z key into an image file and then use vision framework to decode that payload so。

First， generate a key using image and then place the key in an image file。 Next。

 take a Appleel code encrypt it using Zar and then put it on a text file。

 Now you have your encrypted code as a text file and your key。

 So now what I' will be doing is use take that encrypted file。

 use quo libraries and generate a model file。 place the encrypted code into model weights and then send it to the loader。

 right， So I can show you a quick demo how this thing work。So。First thing you generate a key。

 This is now already we have seen like how we can generate an image using Python。

 So this image is placed called Z key。 the same the same thing that we seen already。

 This is the key that were gonna use for encoding the Ael code。



![](img/7cdacd86ecf67221c0131a9ab300fc61_11.png)

![](img/7cdacd86ecf67221c0131a9ab300fc61_12.png)

Next， we will be creating encrypted text file like know。

 take take your appel code means Apple dot Js and take the J S file， use our encryption。

 use the key and encrypt the Appleel code。 So you will see a text file got created。

 which contains your know encrypted appel code。

![](img/7cdacd86ecf67221c0131a9ab300fc61_14.png)

![](img/7cdacd86ecf67221c0131a9ab300fc61_15.png)

Now。You need to embed that Appleel code into an Ml model。 So what the third step will do is。

 you know， there is a Python Python script， which uses。CMl libraries。

 it will take the encrypted code from the text file and place it into model weights and generate a model file for you。

 So this is the dot M model file， which you can ship as part of your loader。

 So now what your loader will do is it will take the M model file and your image file。

 consider youre building an application and you have your model file along with your application and you send it to the victim。

 So the victim， you can place your loader like you know a post runner or no prerunner along with your application package bundle it and send it to the victim。

So when you run it。All you need is the model file and the image file， the image file。

 you can place it like a logo or something。 right now you can completely execute this in memory。

 So what I'm doing is I have another loader。 What this is doing is。😊，It takes the model file。

 takes the image file， use vision to read the key and then take the model file from use core Ml to load the model file in memory and decode it completely in memory。

 extract the payload and pass it for execution。Right， so as soon as you executed。



![](img/7cdacd86ecf67221c0131a9ab300fc61_17.png)

You'll get a beacon on your Me Co。So this is how you can。

Run app file along now by embodying into a M L model file。 And this is completely stealth。

 and it's never been detected during any of our operations。



![](img/7cdacd86ecf67221c0131a9ab300fc61_19.png)

Right。Protection blind spots。 So this model files are static files。

 which never been scanned by a engines or EDs， because you know， even if you scan it。

 it is really hard to find what is sitting inside。 And this audio files again it's a legitimate file when you play it at still play。

 So it doesn't look fishy and also the same goes for the image file as file。

 So you inject your key into an image logo or something。 and you send it along with your package。

 The logo looks legit。 And the image file never been detected as suspicious right So MR uses all this techniques。

 And that's how we were able to bypass all the E D as up today。😊，So as a mitigation。

 I would say know look for what is actually loading them a model file and where this model files from and look look for APIpis that these these applications are the loader is using like look for suspicious API know if application doesn't need to load image vision libraries and why it is calling look for those suspicious API calls which has been used by an application And that's how you can detect because you know this is not just another attack or technique。

 this is know if you look deeper。 this is like a supply chain attack。

 it can also be a supply chain attack。 So there are hundreds model files being posted in public forums and the developers is blindly loading those model files for the day today work。

 And this also can be a supply chain attack So detect this always look what As are being called and whether it is suspicious or not apart from looking for just this is a binary or DL look you know what is doing and。

Whether this image file or model files is look legt or is actually needed for that application。

 And that's how we can detect it。 As up today， none of the E DRs are A engines scan for model files。

 And that's why， you know， you can weaponize this。 And you guys have。

 you know write custom rules to identify these kind of attacks。That is all for the talk today。

 and Im happy to answer if you guys have any questions。

