# Tracking the Tractors： Analyzing Smart Farming Automation Systems for Fun and Profit [OxnY_25suS8]

So welcome to the talk tracking attractors。 Well， at least it's other art tracking attractors。

 but we came out somewhere completely differently。😊，So my name is Felix。

 I'm here with a colleague Bianhardt。 B is kind of my go to guy for everything Lola embedded。😊。

And to then together with a lot of O T pen testing。 So we have pen testing stuff like PO Cs， H Ms。

 and， for example， sar systems。And quite to the contrary of this picture。

 we never to do anything farming or agriculture culture in general。 However。

 we have a friend or a cool researcher， Sebastian。 And one late evening after food runs of dartts and maybe some peerers。

He told us basically， hey， if his friend is selling automation systems for projectors。

That can basically turn any tractor smart。 And we were like， w， you can automate the tractor。

 And that got us into researching this topic a bit more。Well。

 it turns out digization in farming is a huge topic right now。

 Every aspect of modern day farming can be augmented withtization nowadays。 For example。

 the most common augmentation you see nowadays is GPS card farming。

Which ranges from just having your GPS information off your field up to full automation of your tractor automatically driving on your field。

And I always felt like， that's all crazy future Te。

 I will never see anyone use this close to where I live。 Well， after we talk to some farmers。

 turns out they're quite happy to adopt adopt these technologies。😊，And， for example。

 one farmer even used the infrared drone to spot wildlife on his field before plowing the field。

 So utilizationization in this field is。Increasing。But aside from activationisation。

 we also have like active research in there because the tech surface of these components。

 is becoming more connected。And most of the automotive research that's been done in this field is also like classic automotive research。

 like attacks on canvas or on E C Us。But there's also somewhat more dedicated and specific research like they called the talk from sickles from sickickles on death Conferti。

Where he showed that he was able to chainbreak a20 attractor and play doom on it。

 which is the perfect P UC。😊，And if you're interested in this talk， also check out this talk。

And asides from active researchers， also active attacks， like， for example。

 a farmer in Switzerland could ransom that。But at Susan Brasomware was not his normal PC。

 It was his milking robot。And the system was also tasked with taking the vital values of the cows。

 So suddenly， nobody had any vital data of the cows anymore。

 and nobody noticed that the cow gets sick。Which indirectly LED to a death of a car。

 And that's probably one of the most unexpected impact of rents in Vert。Okay。

 so how does this automation work， So you see here， this is like a two typical automation system。

 And on the right screen， on the right side of the screen。

 you see like this is like a steering system。 that's a post market solution that can help you to automate a tractor on the right side。

 you hear this see this is a demonstration set because we could。

 we couldn't fit the whole tractor in our lab。😊，And the whole setup consists of of a steering wheel。

 a tablet on an HMI and a GPS antenna。And it's not that expensive because like 5 to 10000 bucks。

 which is very cheap compared to a new tractor， which often starts of a quote of a million dollars。

 So I probably you can see why farmers like to use these systems。And， to get。

 to give you a better understanding how the system integrated， it's red out with forwardboard。

 So you put on a tablet， replace this thing will put on some sensors。



![](img/636c140212269d09d3c0fe4260e3716f_1.png)

![](img/636c140212269d09d3c0fe4260e3716f_2.png)

And then I could to go。 So Danny have major tractors smart。

And it works for all kinds of following machinery， basically。

You also can integrate stuff like the Plow or other additional hardware on the tractor。

And the only thing then left to do is like， you still need to like define your field。

 decide what kind of pattern you want to drive in the field。

 And next time you go to your field with a tractor。

 you can just hit play and automatically p your field， for example。

And that's quite powerful automation。So， and before we started really researching these topics and。

 and these different devices， we did some little market research to see which systems are more more common。

 And because we live in Central Europe， we looked at the systems that are being sold in central Europe and the two dominant brands。

 The one is adynamics and the Alice Vi Viacmo。 And if you look a bit more to the the east。

 you see in Russia， for example， you see Navomo。Which is a different vendor。

 but has a similar system。 There are some other vendors。

 but these are like the system that's being sold the most。And we got our hands on us F。

 T T and in the react system， and turns out。

![](img/636c140212269d09d3c0fe4260e3716f_4.png)

The other same system。Because like FG T is like building the systems。 And svak， for example。

 is just a rebrand of the same system。 And it was actually great news for us because we knew now we would not it does not only affect like one or two devices。

 It affects like as huge part of automation market for the systems， because like these are two。

 the two dominant brands。😊，And Bent will walk you through some of the mostpo。Thank you， Felix。 Hello。

 also from my side。 Let's begin with a rough overview of our 82 system。 We have two components。

 three components here。 We have our 82 HMI tablebit。

 which is basically the interface of the farmer from the cockpit cockpit。

 We have our motor and our steering wheel， and we have our GPS antenna on top of the tractor。

 which gets the GPS coordinates from the satellite。😊。

communicates it then via H TP S and M Q T T to the cloud aserodynamic cloud， which is。

 which is situated in China。 You also have a Bluetooth con remote controller which you can use to remotely control your 82 tablet。

M Q T T is a lightweight with messaging protocol， which is commonly used in I T I O T products。

Where you can use for exchanging messages between different M2 clientss。We are central broker。



![](img/636c140212269d09d3c0fe4260e3716f_6.png)

So immediate focus was to monitor toward the out traffic。 we have。

The teleus encrypted M Q T pro connection here。 And what we have here is that。Tele is encrypted。

Communication。那个。嗯。So but， what， what we have here is encrypted traffic and。Good， no problem。 Like。

 is here it is like tele less crypto traffic。 And we had the issue like。

 we wanted to see this traffic what's going on in there。 But since device was a rental device。

 And we were like， well， we cannot like， modify it。 so we have to give it back。

 And we had the problem。 We need to decrypt the traffic。And so we did our common thing。

 like we redirect traffic and verified if there would be something form of tele exploitpiitation issue。

 because that would be the easiest thing without having to modify the system itself。And well。

 it turns out， tele tele validationation isn't a thing。

 And the just dot validate any kind of tele certificates in the system。

Which was great news for us because then we had access to the credentials of the client device to the broker。

😊。

![](img/636c140212269d09d3c0fe4260e3716f_8.png)

And here's the example。 we just opened a raw net net Necut socket and redirected amplitude Q T traffic in there。

 So this is like a raw hex stamp of the amplitude Q T packet。So you see here in the greenfield。

 this is a client I D。 This is like a user agent in H，D P， like arbitrary text field。

 And at the bottom， you see like in the blue field， you see like the product name and the password。

Really playing to the Pittsburgh。But like the really odd thing here is like the product didn't。

 the device didn't use the， its serial number。 It's used the product with like 82。

 So it is all devices use the same username， the same password。Which is already a bit sketchy。

But we will come to this， later on。Well， and then we thought， okay， we use just M Q T exploreer。

 a common clients to use this protocol and put in the passport。

 put in the username and we could to go。 Well， turns out didn't work because。



![](img/636c140212269d09d3c0fe4260e3716f_10.png)

They are using a client tele certificate as well。 And that's one of the weirdest tele combinations I've ever seen because on the one hand。

 they have like no telespidation， but they use a client tele certificate before usingname in a passport。

😊，Well， so our next task was basically， we needed to extract the certificate for our authentic。

And yeah， we had two options， either actually the whole application or at least the part that handles this a certificate application。

 but it was obfuscated。Or use feedta instead。 Freeta is a really cool tool for all kinds of dynamic binary instrumentation。

 So we use that tool as custom script to dump all kinds of T S secrets from the application。😊。

And with that， we got our Taylor client certificate。So now we were able to access the broker。

 So we were now able to access the cloud structure and see what kind of messages are being sent to the broker。

 And you see here， for example， in this in the screenshot， especially in this red area， you see。

 for example， the system sends its GPS coordinates to a broker and to notify， hey。

 I have moved and then updates the map of the system。😊。



![](img/636c140212269d09d3c0fe4260e3716f_12.png)

And on this broker， all， all kinds of different topics。 Like， for example， we have topics for。

 for opt of of position。 For example， if the vehicle parameter changes， for example。

 if the farmer puts on a different system or different hardware on the tractor。And also。

 the function invoke function。Whi' is kind of interesting because this has exposed a lot of different functions。



![](img/636c140212269d09d3c0fe4260e3716f_14.png)

But the thing is now， we did this research over a year。 And within this year。

 the infrastructure changed。 So initially， it was way weaker because the initial was just the username and aesthetic password for all of the of the devices。

The password was just just two Charless， two characters。 and， yeah。

 and the number 2020 for password for all of the products。Well， they changed that to。

 to a sales registration service。 So all devices now registered as the broker with a custom password and a custom username。

 However， that was good news for us because it was like initially we only had access to like the European broker because we had only a European device。

😊，But with that， we could suddenly retreat on all kinds of broker they had in different regions。

 So we were now able to access like data in the US， data in China and data， for example， in Russia。



![](img/636c140212269d09d3c0fe4260e3716f_16.png)

Okay， but then we thought like， what can we do now， So we tested different test cases。 Like。

 we tested what happens if we use like an extractor doesn't exist。 The system just accepts it。

 You can log in with a password and a username and a serial number， which doesn't exist at all。

 So it only depends on the on the password。 You give yourself。 And then you can just say， hey。

 on this device。 And it says， you're fine。Also， we can also try to to impersonate different tractors。

 Like， for example， we knew from， from our friend that there are different serial numbers for different tractors。

 We had some of these。 We tried them and they broke it。 Okay。

 you can log in and just imp person at another tractor。But， in fact， then。

 if you can imp personate one tractor， why not impersonsonate all of them。



![](img/636c140212269d09d3c0fe4260e3716f_18.png)

Well， it turns out two characters did the trick。This is an amplitudeput multile wild card。

 which basically just tells， hey， broker， I want to have all the topics and all the messages now。

And the broker was like， okay。

![](img/636c140212269d09d3c0fe4260e3716f_20.png)

And so we suddenly had like a lot of data there and like access to thousands of devices that were connected to this broker。

😊，And for example， in the red area， C， for example。

 there are 600 tractors connected with 80000 of 80000 messages。

 This was just if you open it for 50 seconds。

![](img/636c140212269d09d3c0fe4260e3716f_22.png)

So it's a lot of data。And it was so much data that we had real， real issues with that。

 because suddenly， our system started crashing because it ran off out of memory because we got so much N T traffic in。

 in there。😊，So we had to build a custom tool for this。 So we called it a taactal hackney。

Which basically， yeah， the name says itself。 It just takes all that amplitude dator and just ingests it。

 And we had to like， handle like 100 K messages per second。

So it was custom some engineering that we had to do to get this to work。

And here's a little architecture。 So we have like all the extractors that are connected to their local cloud broker。

 and then we use our t car to connect to this broker and just get all data。

And because like they had the self and shut service， we not have access to all of these brokers now。



![](img/636c140212269d09d3c0fe4260e3716f_24.png)

Well， and since we exit to the data， we could do the synmon in analysis now。

 So over the time of a few months， we saw like 40 cases，440000 systems， roundabout。

 And most of the systems operate in Asia because like like it's the Chinese vendor。 So well。

 that's the product where' being sold the most。 But in second place， we have the European Union。

 where they have like 50，5000 of these systems。And in the US。

 there are just 300 of the systems being sold so far。



![](img/636c140212269d09d3c0fe4260e3716f_26.png)

And the data we see is like， we see all kinds of different GPS data。

 So we know what what the farmer is doing at each of them of each of his daily operations。

 We know where he lives， which feels he tends to， what peer addresses he uses。

 and also sometimes what email addresses to use。 if they， for example， share a field with each other。

So there's all kinds of privacy data in there。But we also have like visualized pit of。

 And see here in the in the US。 You see， for example， these are。

 they are driving everywhere in the in the United States。And also in different states。

 And on the left， you see like example that zoomed in a bit where see， for example， theyre。

 they're using the system also on public roads。 So it's not like theyre traffic actively with their of the system。

 but they are just not properly turning it off。 So it is also used on public roads as well。

And the Bi cl states， you have to turn this off if you drive on a public road。

 But based on the GPS state that we know that farmers are not doing this。 So they。

 they do not drive active eists， They just don not properly powered off。Okay。

 but then we started looking at where to see tractors。 And we。

 we said these tractors in some crazy places。 For example。

 one of them was 20 km away from the frontland in Ukraine。😊，And about 3 in the morning。

So you also see， like this has all kinds of implications that we saw now suddenly can track tractors all around the world。

 We also saw a tractor that was like close to North Korea， for example， that drove in South Korea。

And yeah， so we had a lot of fun at looking at tractors。And what they're doing。And we also。

 for example， saw some interesting pets because we saw like when tractors are driving closer to Ukraine。

 we suddenly saw them cling out sometimes。 And we think this was GPS chaming because suddenly they were all over the place。

😊，Okay， but this was just what we could do with passive reading the data。

 But what if what if is just send some data to the tractors。

 And the thing is like the the command that you can send to the tractor is quite limited。

 So you have like only some commands you can say send to them。 But these are the commands。

 lock and send not。So the lock basically locks the device， and you can no longer operate it。

 So the tractor is no longer in， in the tractor is operating normally。

 but the automation is completely gone。And you can spend the notifications。

 So you really annoyed them and sent them， for example， some messages。And the problem is like。

 the broker has no proper authorization。 so every tractor can send a notification to every other tractor。



![](img/636c140212269d09d3c0fe4260e3716f_28.png)

And we have a short demo video for this。Where you see that the system is just a normal operation and the driver and is just in running in a own model。

 So it's operating on a normal field。

![](img/636c140212269d09d3c0fe4260e3716f_30.png)

And suddenly， you see a pop up message attractivetract will locked now。

 You can specify whatever you want。And it's gone。And。That will。

 that that will be possible for all attractors that are connected to the system。 So suddenly。

 all of the systems would be done。And like only the vendor can restore the system。

 So only then every farmer has to call the vendor to unlock this。

 But the attacker could also spend this command again， and then you're locked again。

So the broker has some really。Wild vulabilities。But。

We also thought like the systems I was used in public roads。 What if we can hack the steering wheel。

 What if we can take over the steering wheel。 And like， for example， drive the tract on a ditch。

 for example， do a hardware to0 at some point。 and suddenly it goes off the road。

So we started researching a bit more how the steering system itself works。

And the steering system consists of all of HMI， a steering a steering wheel and some GPS sensors。

And the HMI itself is just an Android tablet。 However， the vendor did lock it down a bit。

 So it's not just， you're not just able to launch any app， but it's。

Only launches the default app of the vendor。But it didn't implement it lockdown properly。 So like。

 you were able to get your system settings enabled A，8P access。

 And then you had like at least some shell access。And in normal Androids。

 you now would need to line some exploits to get full route access。But， well。

 this was not a normal Android， because it turns out。



![](img/636c140212269d09d3c0fe4260e3716f_32.png)

The device mostly routeed。And because it took us like basically five minutes through the device。

 So we had it on the table， started it， clicked to， the userscape， connected USBC。

 Then we had route access。Because the vendor needed root access for his own code。

 And we looked at the code and was wondering， why， why do they use the route access anyway。Well。

 turns out they were kind of lazy when it comes to security features because they did not implement a proper authorization mechanism for accessing other devices within Android。

 So they had like subsystems in there。 They were just raw serial devices in Android。

 And to access them， you， you， you would have need a decent permission system in place to。

To a proper office author。 But they did they just dropped S U S U S U S U binary and then executed their own code。

Well， but this was good news for us。 So we had route access at the Android system。😊。

But we wanted to do this， also remotely。

![](img/636c140212269d09d3c0fe4260e3716f_34.png)

And that's where we had kept hitting a lot of dead ends。 So we tried a lot of things。 We tried。

 for example， to send Melisa messages via the broker to get some form of coec。However。

 the command that you can send from from the broker is quite limited。

 and it also lands only in safe travel codes， which is was。

 which was what vulnerable for any kinds of memory corruption or other issues。

We also tried for via via a network。 For example， Does it have any services exposed， No it does not。

 It has no services exposed。So that left us with the following free attack vectors。

So with physical access， we can just route it， but it's。

 does not so create a tech vector because then we need physical access to the device。

 The next thing is R C via Bluetooth。The system itself is so outdated that it is vulnerable to some well known Android Bluetooth vulnerabilities。

 And they are public experts for this。However， it's like you have to be Plutth purchasing chasing for this。

 Plutth has to be enabled and the export crashes crashes to the system more often than it gives your shell。

😊，So， not the best way。And then we started looking into something that we can man in the middle and just replace the response from the server to the client that could trigger costic execution on the the on the HMI itself。

And that's where it struckhold。 because what better feature is than than the over output feature。



![](img/636c140212269d09d3c0fe4260e3716f_36.png)

And for some reason， the vendor really dropped the ball on this one because like。

 they didn't have like transport encryption for this。

 and they did not have like any form of validation or signature for this。Normally。

 when you have some form of fiberr update or over the update。

 you have some form of strong esymmetric signature that protects you from tampering with the image。

And in this case， they just use M5 sun。 So， well。I guess we can calculate that。

And on the right side of the screen， you see like， this is a typical request。 You hear it say， hey。

 it's time to update。 We are updating to version 25。 And this is the URL you need to download。

And that's like a textbook example for drive by download。 So if you're in the middle。

 you can just replace this with any kind of binary you want。And because the device is a pre routeed。

 This leads to a full root， root code execution。Well， but man in the middle is always hard to get。

 You have to be to get adjacent。 It doesn't scale well。That's true。 And it doesn't scale well for us。

 but it's well documented as， for example， nations that actors or advanced threat groups are capable of performing men military attackss on Internet connections。

So while we cannot exploit the v at scale， they probably could。Okay。

 so the ta itself is quite simple。 You become in the middle。 You replace the response。 For example。

 in this case， you just replace it with a custom URL。And you have to calculate M5 sum。

Replace the code with0 at the bottom。 That's important。 But the or the client doesn't take。The。

 the final opt。And also， you have the possibility to add a custom message like say total another virus。

 trust me I a dolphin。Okay， so if you do this， then a user created up by this delightful dialogue。😊。



![](img/636c140212269d09d3c0fe4260e3716f_38.png)

And if the farmer says， oh， the status instance is legit and accepts it， we get full root access。M。

 this is like a funny example， but a real tech could use as something more malicious or something that triggers take Kos symbolbolic and social engineering to get the the farmer to install the system。

😊，Or the update。

![](img/636c140212269d09d3c0fe4260e3716f_40.png)

And as a P O C， for example， we use the metapreta just as a simple P O C。

 And you see here that the A P K is being downloadedre from Web。

 and then the meta Cta session started。So it's quite easy to route this device also remotely。



![](img/636c140212269d09d3c0fe4260e3716f_42.png)

And because it's already routed， we have already rooted access。So at this point。

 we had like full route access via a network。But the thing is。

 it was still a long way to go to the steering system because the steering system itself consists of multiple components。

You have like this Android tablet， and then you have like a EC C U。

 which is an embeddedella device with， with， which takes multiple inputs from， for example。

 the Android tablet and the IM U。 the IM U is like orientation sensor to know how level the tractor is。

And。To go， the E combines its inputs and then makes a steering decision， which。

 which is then send to the motor control unit， the M U。

 And then the M C U is a real device that turns the steering wheel。Yeah。

 so our next task was was the techctic issue。However， this was a rental device。 So normally we。

 we would like open it up it just attach Jtch to it or get some form of flash dump to do some form of analysis on it。

But this was an option for us。 So we had only option was either to direct flash。

The farmer we the Android tablet。Or reverseary a protocol is still being used for steering。

And so we played it safe。And we reverse into the protocol。Yeah。

 but that took us some time because like the P was just 200 k of raw arm assembly， no symbols。

 no strings， or anything。 We didn't know it takes exact specification processing。

 and we did not know what kind of operating system was used in there。

So our strategy basically was to look for some function that has a high density of condition statements and a high density of magic bys。

 because that's typically how packet handler looks like。And at some point。

 we found a function that basically specified how a protocol looks like and。

We reverse it the the sub function， clean， clean up the code。

 And we came up with the following protocols spec。So here you see what kind of commands possibly be with steering system。

At least we think these these are the correct commands。 But on the right side。

 you see the different packets or the packet structure。

 And you see the packet always starts with E C 91， has some headers。

And the most important byte is like the byte 16。 So because the byte 16 is the command byte。

 it basically tells this， the steering system what kind of command you're sending。And for example。

 this is a waypoint configuration command， which basically specifies that you're driving from point A to point B。

 So after the command by， you have like the， the command payload。

 which is when give this case for float values for two times latitude and longitude。

And the important thing is， like。It's not just enough to， to send a command like turn left or right。

 because that's impossible。 You have to specify like a certain route to say， hey。

 we're driving from point A to point B to point C。 And then the E C U decides how to reach that point。

 So the system itself has no direct control over steering wheel。So if you would， for example。

 want to create a PUC where we hijack the steering wheel and just do a left or right one。

 we have like to calculate a 90 degree angle to our current position。To make， for example。

 a heart rate1。Okay， so with the protocol specification now。

 we were close to taking over steering wheel because we still needed one thing。

 And that was the correct communication flow。😊，Because it turns out you need multiple packets to。

 to a steering wheel， because you need at one， you need a heartbeat to the E U。 because if you。

 if you stop the heartbeat， the E U shuts down， you need to specify waypoint。 for， for example。

 the configuration that tails the steering system to to drive point A to point B。

 specify some configuration parameters and then and execution command。

But then you go to go after you send these types of commands， the steer wheel starts turning。

If you want a full PC attacker， you still need to be aware that you have to stop the originalal app or it will interfere with the steering。



![](img/636c140212269d09d3c0fe4260e3716f_44.png)

Okay， and we put all this together in this demonstration。



![](img/636c140212269d09d3c0fe4260e3716f_46.png)

And here， for example， the system is just an operation。 It's in passive mode。 Like， for example。

 some people use it on public roads。 It's just， you can just normally use the tractor。

 but it is where the prelude byteer。 and it just can flip the switch。Ant steering is skull。

Then you have full controller of steer wheel。And the problem is， like。

 the steering motor is quite strong。 So you have now two options。

 Either you start fighting with the steering wheel over control。

Or you're quick enough to turn the system off。But this basically takes a split second to drive the tract on a ditch or lead to a tilt over。

So， good luck with it。Okay， but in beginning， were told like。We ended up somewhere a differently。

 And turns out that the vendor also has a lot of other devices connected with a broker。

And at some point， we saw like a call that's a function call that called Take photo。

 And we were like， what's going on here， Why， what， what。

 who's taking a picture and why is taking a tractor a picture。Well， turns out it wasn't attractor。

 It was a lot more。That the same window is building。

 but is connected to the same broken cloud in structure。And how this lawn mo operate。

 they basically just take a picture of the camera， upload it to a cloud pocket。

 and then you send a cloud pocket URL via the broker。 And because we were listening on a broker。

 we saw like the URLs to these cloud pocket pictures。And normally when you have cloud packets。

 there would be some form of signature， some form of token that protects you from just so you cannot just download the pictures from there。

Well， the man of a quote also for that quote also about this。So suddenly。

 we had like full access to camera feeds around the world from multiple from lawn mos across different continents。

And like， we have a whole selection of video feeds from different lawnlors， suddenly。Well。

 talking about scope C。

![](img/636c140212269d09d3c0fe4260e3716f_48.png)

Okay， here also shot demo photos。And here you see， for example， different lawnmas in the left。

 you see like a soccer field and some stadium。And this is just a short snippet。

 But we had like access to a lot of different of these video feed， suddenly。Yeah。

 it's it was also quite also quite interesting to watch what what all these tracks what long was are doing。

Okay， but devils are the craziest part。We also saw something different。 We saw like， at some point。

 we saw like a device called open open door， call elevator， closed door and open door again。

 We're like， what's going on here。Well， turns out the the is also some building some different platforms as well。

 They are like also building automation robots that are able to like， take the elevator and drive。

 for example， from point A to point B and also open doors。😊，You might have seen this， for example。

 in pars or in hotels where suddenly you can call an elevator robot and it will drive up to your to your floor and show you some drinks。

 for example。And the thing is like， we were a broker， and we were able to do the same calls。

And we was were interested。 Maybe we， we could， we could try this course。

 but we didn't do this because we like to to， we didn't own the hardware。

 So we didn't want to interfere with some operations。

But we are quite confident that we would be able to just replay these calls and call an elevator and call an open door somewhere else。

😊。

![](img/636c140212269d09d3c0fe4260e3716f_50.png)

And this is also， for example， how the vendor sell the system。

 So it has like the integration between， for example， an elevator and a robot。

 And you can just use this。To open doors and call an elevator。

 And that will be probably one of the most unexpected red timingming strategies ever。 If you。

 for example， heed like this to robot and suddenly you get access to a door or to an elevator。Okay。

 so what does Daendo say about this？ Well， we have been talking to the vendor for over a year now reported this last year in June。

 And the communication was a bit tricky。 At some point， Dawenda took a little break。 And in May。

 he came back and said， yeah， everything is fixed。😊，But we don't know what they fixed。

 They didn't fix the security issues。And two weeks before Blackhead suddenly。

 the CEO noticed that there's something going on。 and we got in touch with the CEO of the company。

 And turns out there was some internal miscommunication and the security issues have not been properly communicated with an organization。

 and they are currently still mitigating these issues。Okay。

 so let's close out to the lessons learned。Okay， so digization in， in air culture is a huge topic。

 And you see a lot of new technology being fielded there。

 And there's a lot of research potential there to be tested and to be。To be heed。Also。

 from our personal experience， security awareness in this field is not that high。

 So especially when it comes to users and sometimes the vendors。

 they don care about cyber security or the issues to introduce with introducing these technologies。

And also， agriculture is at large scale， also created infrastructure。 So this should live。

 this technology should live up to， should live up to a higher standard。



![](img/636c140212269d09d3c0fe4260e3716f_52.png)

Okay， so that was it for the talk。 I would like to kick off the Q And A session。

 and I would like to answer the most important question first。



![](img/636c140212269d09d3c0fe4260e3716f_54.png)

Does it run tomb。 Yes， it does。👏First。So wes a vendor here today to listen to。The question was。

 iss the vendor here。 we don't know。 I don't think so。 also we don't know。

They have some mitigations in place， but we haven't had the time to test them because were like two weeks before blackheads。

 So， they they， we had some emails correspondence。 They they say， hey， they fixed it， but。

We don't flow。But the thing is like， it's not just they have to fix that。

 They also have to like upgrade all the farmers for this。 And yeah， they have to call， well。

 this probably take them some time to upgrade all these devices。Did I get it right。

 The the steering wheel had also Bluetooth controller。

 And did you look into communication between this Bluetooth controller and the steering wheel。 Yeah。

 we that also we， we we took a look at this because， but then。

 but this is just standard Android Bluetooth interface。

So some farmers use this as as a remote remote steering controller for this， like， like a remote。

And it just connects to the Android and just inputs some， some normal inputs like a keyboard。

 for example。 So it's just standard Android Bluetootht。Okay， so， so it just acted as a heat keyboard。

 right？ Yeah， it is ex， similar to keyboard。Let's please。Yeah。我。のは。The question was。

 if you could really refine the the steer wheel control。啊。In theory， we would have been able。 But PC。

 we just used topoint， so we basically can specify a point to drive from A to B。

 And then we will have to calculate what kind of route we want to drive。

 So we're quite close to doing a full control over the steering wheel， but we never implemented this。

Yes， please。What functionality is the MQTT broker？Provide the system。俾我哋啊你。Yes。Yeah。

 that's interesting because there's also some other platforms that are integrated with this data。

 They like， they have a fleet management software where you can track the data。But it's also。

 it's also for， for tracking what kind of operations you did on a field。

 you can drive the tractor offline as well if you have the the maps offline。 So it's not。

 it's not mandatory to use this every time， but it's used by most of these farmers to like have the data on your fields of your fields。

The farm good。Access to you。The other can partially for So they have to synchronize the maps。

 But if they did this， then they can use this in offline mode。Okay， I think that's it。

 Lets say thank you and。这有。