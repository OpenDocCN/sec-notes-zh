# AppleStorm - Unmasking the Privacy Risks of Apple Intelligence [iL2McWODDnc]

Good morning， everyone。 And thank you for coming today。😊，Before we dive into today's topic。

 I wanna start with quick show of friends。 So let's start。

How many of you here in the audience own an Apple device。Okay， that's a lot。

 I think that was not a very good question since we all know how Apple controlled the market。

 both on laptops and i O S。 But that's not why we are here today。

 So let me ask you much more specific question。😊，How many of you use Siri， Sorry。

 I did not like that。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_1.png)

Okay， I think this is a very good point to turn off Siri or your iPhone because it's gonna turn up a lot。

 But， you know， series is kind of old news like we have many new sophisticated technologies today。😊。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_3.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_4.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_5.png)

So by saying that， how many of you use Apple intelligence。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_7.png)

Or at least， know about it。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_9.png)

Okay， so for everybody here who don't know what is Apple intelligence。

 it's the new AI buzzword of Apple since last September。

 when they launched it and mentioned that they want to enhance productivity and keeps our data private。

 And by that， they launch many new AI apps。 like writing tools， image tools。

 Even Siri is much more powerful with the capabilities of Apple intelligence。😊。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_11.png)

To be honest， this entire research began when one day I had the weirdest interaction with Siri。

 I was in the office， and I was working on my notion。 You know。

 the documentation app where we store all of our organizational files was doing a document review to one of my colleagues。

😊，And a few moments later， I start talking with Siri。And a few moments later。

 I noticed that Siri was referencing a topic from my notion， a title of one of my documents。

A classified one。And I start wondering to myself how Siri has access to my notion。

And was that happen entirely on my device， or I might have share information with Apple servers。

Do you want to know the answers to these questions， Stay up for the rest of the talk。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_13.png)

So my name is Yamagid， and I'm very excited to be here today。 This is actually in my first black hat。

 I'm。😊。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_15.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_16.png)

Thank you。 Thank you very much。I'm 25 years old from Israel。 I'm an 8，200 aluminni。 and today。

 I work as a team leader and an AI security researcher at Lumia Security。😊。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_18.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_19.png)

And I came here today to talk with you about some privacy concerns that come with all the new suit of AI of Apple。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_21.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_22.png)

And I want you to remember one thing during this talk， those risks， there policy concern。

 those privacy concerns we're going to talk about are。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_24.png)

Just one example here in Apple intelligence and Siri。 But they exist everywhere。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_26.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_27.png)

So without any further ado， let's go over our agenda。

 We're gonna talk a bit what is exactly Apple intelligence， how it does it。

 how it works in background and we'll see what kind of risks privacy are we talking here about and how I managed to find them out。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_29.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_30.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_31.png)

And later， we're gonna see some demos how exactly Siri and the other apps in the AI ecosystem interact with our data。

 And in the end， we'll see how we can mitigate these， these risks， or at least minimize them。😊。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_33.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_34.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_35.png)

So let's start a bit about talking about Apple intelligence。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_37.png)

So as I said in the beginning， I thought I knew what is Apple intelligence。

 And I think Apple got mad a bit。 So I'll try to stick to the script。

 What is exactly Apple intelligence。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_39.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_40.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_41.png)

So like I said before， Apple wants to enhance productivity， but keeps our data safe。

 how they are doing that by simply defining in their infrastructure two group of models。

 The first group are on device models， models that run on the user device on Mac O on ios。

 they do not require any network communication to operate。 But you know， theyre on device model。

 not enough in some cases。 So they created another group of models， their cloud models。

 which more of a language models and image models。😊。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_43.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_44.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_45.png)

And by working together， they build on top of that， many new experiences。😊。

But that is not exactly mentioned how they're working together。

 So if we dive the appear into their privacy policy。

 we see that this model working together by a model that runs on the user device that decide if we can run the user task completely on the user device。

 And if not， it will need to leverage a larger model from their private cloud compute。

 their server models。 and they're gonna send only the relevant data to the cloud in order to fulfill the user request。

😊，But， you know， it's not everything in the AI ecosystem。

 We still have another infrastructure involved in the AI ecosystem。

Apple has the main three components in the AI ecosystem。 The first one， like we said。

 is the private cloud compute。But we have two more infrastructure。

 The first one will be Siri infrastructure。 When we use Siri。

 there are two more servers that are operating in the background。

 The first one will be their dictation server， Gaazzi dot Apple dot com。 You know， that's。

 that's quite funny， because Gaazuni is a chief scientist in Siri。 So I wonder。

 who has access to this server。 And I wonder what is the purpose of this one since for many years。

 dictation happens on device。 So remember that。😊，The second one will be smoothuth do Apple dot com。

 their search services。 whenever you want to look for something online or something in the Apple ecosystem。

Siri used the search services。When I started this talk， I started looking online about these servers。

 and I found out that smart people thought it was a spyware because they noticed many。

 many requests go to that server whenever you hit a click on your keyboard。

 But I'm pretty sure it's not a spyware。 It's just a search services。😊。

And the third one will be the Apple intelligence extensions。 For example， Cha GPT。

 when we want to interact with him through Apple intelligence ecosystem。

And so now we know what is exactly going in the background and how all of these apps are working together。

 Let's talk about the risk， what we have here。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_47.png)

In this talk， I'm gonna go over the features of all the AI apps。

 And we're gonna see which features goes on the user device without the need to network communication。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_49.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_50.png)

And the second one we'll see is which features work on the private cloud compute or on any other infrastructure of Apple。

 like we saw， like Siri。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_52.png)

And when we go online， we're gonna see which data we sent from our device to the Apple servers。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_54.png)

I'm gonna see that by simply intercepting all the communication between theoryri。

 between writing tools and the image tools to their servers。

 We're gonna see which data leaves our device。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_56.png)

But， you know， it's not that easy as it sounds。I actually work very hard to find this data， why。

So with Apple intelligenceligence corere， it was kind of easy。

 They log every request that is being made to the private cloud compute so we can see it on our device。

😊。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_58.png)

But with Siri。It was more complicated。Actually， we're gonna see for the first time ever。

 as far as I know。😊，They have a communication of theory with their server。 Why is that。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_60.png)

So every app today they use S encryption to encrypt the data that leaves your device straight to the to the servers。

 And only the server and the client can know what the data is。😊。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_62.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_63.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_64.png)

But Siri use another security layer。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_66.png)

They use a technique that's called certificate pinning or SS L pinning。

 which makes the app to validate that the remote server is actually Apple server。 And by that。

 it rejects any other server。 For example， if I want to do some adversary in the middle attack in order to reveal all the data。

 I cannot do that， because Siri won't accept this connection。

So I worked very hard in the last few months to bypass the certificate pin mechanism of Apple。

 of course， only on my device。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_68.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_69.png)

And。After that， I decrypted all of Apple protocols behind that， and it wasn't very easy。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_71.png)

And in this talk， we're gonna go over different features。

 and we're gonna see the actual data that is being sent to Apple servers。So by that。

 let's start with the different scenarios。But before I want to start with some baseline for the demos。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_73.png)

These screens might look familiar to many of you since in the last months。

 they were all over the media。 after people noticed that whenever you agree to Siri to Apple intelligence。

By default， you are opt in to learn from this app， which I don't know what exactly that mean。

 but I made sure that in the beginning of the research， I turned off all these settings。

 So I won't be expecting to see any data that is not relevant inside the communications。

So by defining that， let's go over the first app， Siri。Our old love as a personal assistant。

 which we know for many years。 But now it's got upgraded using the capabilities of Apple intelligence。

 For example， one of the new features with Apple intelligence is the fact that we no longer need to use theoryri just with voice activation。

 We can type to Siri。 You know how manyeds。 It solve me that now I don't need to hear Siri every few minutes telling me。

 okay， setting up an alarm for 6 AM。 I can just now type to theoryri whenever I want without a need to use voice activation。

 So I was wondering in the beginning how I'm gonna research that I started with a few prompts。

 and I saw none of the network communication。😊。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_75.png)

So I thought to myself， okay， hello， what can you do， How are you not gonna work， They are。

 theyre going local processing with the localized models。

 So I needed to compose a prompt that will definitely goes online。

 The prompt that started Apple Storm。There is no model in the world that can know what is the weather today。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_77.png)

B， without invoking any API， some tool。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_79.png)

So I started with this prompt to see what's gonna happen when I send a request to Apple servers and which that I'm gonna have。

 we're gonna find， you know， beside the weather today， which is， by the way， very hot。😊。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_81.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_82.png)

So let's see what's going going on in in the background。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_84.png)

So I started。 I typed the prompt。 What is the weather today in Las Vegas。

 and I set up an infrastructure that broke that pinning。

 Intercepted all the traffic and decrypted the protocol。

 And the first packet that I saw was being sent to the search services with my prompt。😊。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_86.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_87.png)

Okay， that was very makes sense because， like I said， it cannot process that on the user device。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_89.png)

The first thing I saw in the packet was my precise location。 And at the beginning， I said， okay。

 that makes sense。 Like I ask about the weather and they want to improve my experience by telling me the exact weather in my location。

😊，But that's not really the case here。To be honest， for every request you are doing with Siri。

 your location is being sent by default。 even if you ask a math question， how much is one plus one。

 beside the fun fact that they need network services in order to answer a very simple question。

 like how much is one plus one， they also send your location。But at least they give control over it。

 We can go to the settings to Siri and Apple intelligence settings。

 and we can disable this dis Si from sending our location。 They separated to two different settings。

 not much of user experience。 But after I dis that， I couldn't find my location inside the traffic。

 So that's good。😊，So I scroll down a bit more in the packet， and I saw。An up name the weatherup。

 official weatherup of Apple。 And I said， okay， that also makes sense because I ask a question about the weather。

But after that， I saw another app。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_91.png)

And I couldn't recognize that one。 There were so many random letters。

 but I could recognize the parallel word， which is a virtual machine manager that I had installed on my device So I got inside to the parallels。

 opened one of my V Ms， which was Windows machine， and inside the Windows machine。

 There was a weather app。 with the same name I saw in Apple communication。😊。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_93.png)

So basically， I ask a question about the weather and it look up for every weather app I have on my device。

 including my virtual machines。 very intrususive， I must say。

 And they send that to C research services。😊，So does that happen just because I ask about the weather。

After a quick try。I saw that they're sending your apps for every topic you ask。

 You might ask about your emails or about media player or about coding。

Each time you send a pro to Siri， Siri do some topic modeling on the user device。

 And by the topic or many topics you ask， Siri will find every app you have on your device。

 including virtual machines and will send the names of the app to Apple servers。😊。

But is that the only apps that they are looking for。So not exactly。

 I managed to find another packet that is being sent not to the search services。

 but to the dictation server， which again， I'm not sure what is。

 what is the purpose of the dictation server。 and I could find all my active apps being sent。

 When I took that picture。 Slack was open。 Finder was open and my notion。

 and that was being sent to Apple server through the dictation server。😊。

So that was very weird by now， because I saw many data that is not relevant to my simple question。

 what is the weather today， And I thought to myself， okay。

 we had some nice data about the context of the user。

 Probably the next data frame will be about something related to the weather。

 So I went to the next frame。😊，And it got me very confused。Like。

 I wasn't really sure why they're sending infofoable Taylor Swift to Apple servers。And， you know。

 I was really frustrated about it。 Like how they know my favorite song。And after， after a while。

 I went after my browser。 and in the many， many tabs I had open。

 I saw one tab on YouTube that it that song was playing there。😊，And was I  waitate。

 are they sending my audio or just metadata about my audio。So I went deeper about that。

 and I tried many different scenarios。 And it seems like every time that something is being added to your now playing queue。

This data is going to be sent to Apple servers， both to the dictation server and the search services。

 If you listen to a song on YouTube， you listen to a recording on your device through VLC in YouTube or any other website。

If it's being added to the now plain queue， it will be sent to Apple servers。

Remember the demo we signed the beginning of the talk about notion。 I had a video in that document。😊。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_95.png)

And when I played that video， I paused it。 I went to different app。 This video was still on。

 but the now playing queue。😊。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_97.png)

And that was being sent to Apple servers。 So I must say that was very weird because。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_99.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_100.png)

I don't know why the title of the document was attached to the。

 to the audio in the the embedded audio。 So that's why we saw titles on the communication。

 Even when I needed ask， when I even I didn't ask about that when I asked about the whether I saw audio metadata。

 This scenario will happen if I'm not asking about notion notion at all。 or a similar question。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_102.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_103.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_104.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_105.png)

So summaring what we saw right now， I ask a very simple question about the weather。

 And it seems like serious thought I ask about other stuff like my favorite song or about my apps。

 Now， they even know that I have virtual machines on my device。 Like I said。

 I felt this is very intrusive， Like so much context about very simple question。😊。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_107.png)

But， you know， Siri can do much more than that， not just asking things that we know online。

 Siri can also use some integration。For example， I can dictate or write through the new feature of Apple intelligence。

 I can write to Siri to send messages or compose an email。 So I said。

 let's see what's gonna happen here。 I tried the Whatsapp integration with Siri。

 which required that I will install Whatsapp on the device。 So I thought to myself， okay。

 Siri gonna communicate with Whatsapp and they're gonna send a message through the Whatsapp process。

😊。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_109.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_110.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_111.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_112.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_113.png)

I was very shocked to see that Siri sends the message to Apple servers， to the dictation server。

The content of the message， the contest name and his number is being sent to Apple servers。

And I thought to myself， is that part of the core logic。So， I tested that。

I shut down all the communication to Apple servers。 I blocked it on my device。

 so Siri cannot communicate with Apple Ser at all。 And I ask Siri again。

 can you send this message again。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_115.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_116.png)

And the recipient got a message without this frame being sent to Apple servers。 So I'm not quite。

 I'm not quite sure why is this communication necessary。

 Also why is being sent to the dictation server， I typed it to， to Siri。 So this was very weird。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_118.png)

So I started making a list of many integrations that Siri support。

 And I look for each one if they send this data to their servers。

 and I saw this behavior only happens with a messaging service。 For example， What's up or im message。

 I saw that in exact behavior when using eye message either。

 But with other integrations like composing an email or going through my calendar。😊。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_120.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_121.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_122.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_123.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_124.png)

The content was not being sent， you know， beside the metadata about the speakers or maybe my active apps。

 which are being sent on every prompt we use。 But the content of my action。

 my email I was writing or the events I checked on my calendar， they were in sent。

 All of that was happening on the device like the Whatsapp on or eye message。 But in Whatsapp。

 there was another packet that is not necessary。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_126.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_127.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_128.png)

But， you know， Siri is one app in the AI ecosystem of Apple。

 So I thought to myself maybe another thing happened inside this ecosystem。

 But I was amazed that the actually really good stuff in the other apps。

 So I went into the writing tools after I finished with Siri。 You know。

 the writing tools where you can interact with data through your browser notes， your email。

 And you can make your content look more friendly or rewrite it。

 And I was very amazed that most of the features are actually working on the user device。

 Like I couldn't see almost any network communication goes to the private cloud compute or other Apple infrastructure。

😊。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_130.png)

The few features that do require network communication were went to the private cloud compute。

 as I expected， as a user， after reading their privacy policy and after looking at their marketing。

 I actually can divide which features require network communication and which one goes on the user device。

For example， in this screen， we can see when I turned off my network services。

 I enabled airplane mode， and I could see that a few features are now disabled。

 Those features required network communications。😊，And the other feature are not。

 Theyre being on the users's device。 And when there is a network services。

 it doesn't communicate with Apple servers like we saw in Siri that there is some frames that are irrelevant that goes to their servers。

 So that's quite good。 But it's not the only thing they did good in the Apple AI ecosystem。

 Also their image playground。 At the beginning， I thought， wow。

 maybe they are sending also images or the prompts we are making with them。😊。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_132.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_133.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_134.png)

But I was very surprised to see that all of their visions and image playground were entirely on the user device。

😊，I went through all the communication， and I couldn't see any packet that related to my prompt about my pictures。

 goes to their servers。 It was entirely on the image models， on the user device。 So that's nice。😊。

So moving on to our last feature of Apple intelligence for today。Their extensions。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_136.png)

When they launch extensions a few months ago， they said that we will be able to expand Apple intelligence capabilities by integrating third parties。

😊。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_138.png)

But actually， in reality。Only Cha GT is available。 You know。

 you can communicate with Cha GT through many instances of the Apple intelligence ecosystem。

 through Siri， through the writing tools， which is very nice。

 Like you have a small chat G on your device， but。😊。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_140.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_141.png)

There are two main things that you need to know when you're using Cha GPT through Apple intelligence。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_143.png)

The first one will be that if you thought that you're communicating directly with open AI services。

That's not true。 Actually， each request you are doing with serial writing tools to J GT。

 your goes through Apple servers， not through the private cloud compute or sri infrastructure。

 but they go through the third infrastructure， Apple intelligence extensions。 Also。

 if you loggged in with your organizational credentials or your personal account。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_145.png)

All your communication goes through Apple servers， not directly to open AI。

But that's not what I find the most concerned。When I used Siri with Cha GPT。

 I found that all of my prompt are being sent to two endpoints。

 One was Apple intelligence extensions， as I expect。

But the prompt was also being sent to the search services we saw before。

 And that is very weird because this is not part of Apple intelligence， as far as I know。

And I don't know who has access to it， why they're doing with this information。

 I have no clue about that。 but I do know that the response from the search services is redundant to the user。

 The user cannot see that。 You just see the response from the Apple intelligence extensions。

 So I'm not sure why they are sending those two pro these prompt twice。But， you know， at that point。

 I said to myself， okay， this is a good time to stop and go to talk with Apple about it。😊。

So I started a disclosure with them about this privacy concerns I had。 I started it last February。

 I sent them all the things we saw here today。 They required me much more information。

 They wanted in logs。 They wanted pictures and the entire demos。 And I said， okay。

 I will send you all the data。😊。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_147.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_148.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_149.png)

And I was very surprised that after a few weeks， they said， okay， we're gonna address it。

 We see here's something that it is strange。 And we're gonna。

 we are working on a solution to fix that。😊。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_151.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_152.png)

And it was， okay， I'm very happy I finished my research。 Now they are going to fix it。😊。

And then last month， I got another comment from them， which was completely different。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_154.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_155.png)

They started talking about that。All my findings weren't actually Apple intelligence concerned。

 There were Siri issues like the search services and the dictation server are not part of Apple intelligence at all。

 they are Siri core servers and they're not part of the private cloud compute。

 which we got confirmation for that。 And I was very concerned about that。 Like。

 I don't know who has access to that。 And how all this infrastructure。 And also as a user。

 it quite like， I didn't expect that because according to their marketing。

 I thought that Siri and Apple intelligence are quite the same today。 You know， you actually。

 if you disable Apple intelligence on on your device。

 you'll see that the logo of Siri change to the old logo。 And when you enable Apple intelligence。

 It goes back to the new logo。 So that was very strange to that comment。😊。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_157.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_158.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_159.png)

But they continue and they then said that， you know， most of the things you found。

 you should probably reach out to notion or to Whatsapp because maybe theyre not using Siri properly。

There is something called Siri extension， Siri kit that you can add to your apps and by that。

 add integrations with Siri。 So they said maybe what's up in the notion or not using it properly。

And I thought to myself。Notion has no issue with ciriate。 This is not about notion issue。

 It's about the now playing queue。 But I said， okay， Apple， I will reach out to notion。

 I opened a disclosure to notion。 And three hours later， they closed that， and they said。

 we don't care about that。And before reaching to Whatsapp， I thought to myself， okay。

 this is very weird because we know it happens。On I message， we know it's not part of the ch logic。

 we saw。 I disabled all the communication to Apple servers。

 and we still can when the feature still works。 So I said， okay。

 let's dive deeper to that and see what exactly I can find about that。

 So I said this is a very good point to test Siri kit。😊，So when you open a new app。

 I use Xcode the I D for Apple products。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_161.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_162.png)

And I went into Apple's documentation。 I said， let's create an app that looks exactly like their documentation with their recommendations。

 their configurations。 And I made sure that there is no hidden configuration or some hidden flags that I can pass to the Siri kit。

 So it once't sent this request。😊。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_164.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_165.png)

And after deploying that， I was very surprised to see the same behavior happened on。😊。

That exact on the Da app， when I use the messaging services with Siri with this exact app。

 So I'm not that sure that it what's up using that improperly。

 And I think it's maybe it's an an issue， maybe in Siy， maybe in Si。 I don't know。

 but I can control about that。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_167.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_168.png)

So I sent this new information to Apple a few weeks ago， and I waited for a respond。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_170.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_171.png)

Until last night， last night， they reached out in the last minute， of course。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_173.png)

And they said many things。 They started by saying that， okay， thank you for testing Siri kit。

 maybe there is a issue here。 So we'll pass it to our engineering to check it further。

 So I hope they're gonna fix it。 And in the next version， it won't happened anymore。 But let's see。

 let's see what Apple gonna say about that。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_175.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_176.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_177.png)

And they also said that they're willing to clarify some issues with Cha GT and the private cloud compute。

 You know， at the beginning， I thought that Cha GT goes through the private cloud compute。

 and it's part of it。 like they communicate with open AI servers through the private cloud computes。

 And now it's a different component。 So they're willing to improve their documentation。

 So there will be more clarification about that。 So that's really good。

 But until theyre gonna do that until they're gonna fix all of that。😊。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_179.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_180.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_181.png)

I want to share with you some mitigations I have when I did this research， I saw how I can。

 how I can protect myself from that。 So there's not a lot to do。

 but there are some things that we can mitigate those stuff。😊。

My first recommendation gonna be about the dictation server。 As far as I know。

 it's not part of the call logic。 So we can basically disable。 We can block this communication。

 And we can minimize the risk of your apps being leaked or your Whatsapp messages。

 this will mitigate it。 This will solve part of these concerns。 not all。

 But this for definitely won harm the functionality of Siri or other components。 By the way。

 this is a very good point to take some photos on these mitigation。😊。

My second recommendation will be about their settings。 You know， as we sign the beginning。

 there are many settings that we can control of what app they can learn from。

 I recommend for everybody here to go today。 Not on one day when you go back to the office today to go over your settings make sure which app you allow Apple to learn from your experience。

 One of my friends， I sign his phone that is sharing apps about healthcare。 And I told them。

 why are you doing that， Just disable it is many of our data live there end。

 We don't know exactly what does his mean， Learn from this app。

 So I recommend you go over that and disable every app you don't want to share。😊。

But on a further approach， I have some takeaways about this research。

My first takeaway was gonna be about privacy policy。You know， I think it's kind of obvious。

 Like everybody here， we don't really read the privacy policy or terms of use。

 When we go to a new app， we scroll down， we click that I accept the terms of use。

 and we keep equi with our lives。 So I'm not say we should read all of these legal stuff。

 But in this AI era， we must know what kind of data are we sharing， What am I exposed to。

 Are they able to learn from my experience， What exactly does it mean。😊。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_183.png)

So I think something need to change her， even it'， if maybe showing this data in a different way。

 You know， one day when I checked into my flight， there was a really nice。

 interactive way to show which stuff I cannot bring to my flight。

 Maybe this is one of the cool ways to show which data they send about us。

 or maybe use chat G T for that。 No， I can compose the prompt that can summarize the privacy policy just to a few topics of what are theyre sending about。

 And it gonna cost me2 or three minutes， But it will probably。

 I probably have at least the knowledge of what am I sharing about。😊，But if you don't do that。

 like all of us like me， we can also use other solution more of any enterprise context。 You know。

 governance， it's not a new world like governance is really important mostly today when， you know。

 AI are everywhere like here in Apple， we have intelligence and Microsoft we have pilot。

 Gemini and Android。 it's everywhere。 And organization need to have the proper tools to know which data leaves their device leaves their organization And we need the proper tools to monitor this behavior。

 You know， it's not just about the prompt or what in my am I uploading to the AI service。

 It's about the whole context。 so most of the concerns here。

 We't about the prompt I send where the extra data that they are looking after。

But these tools are not gonna to work when we don't have transparency。Like I said in the beginning。

 I worked very hard to break the certificate pinning。

 I kind of dashshed my head against the wall for many days and weeks to bypass the certificate pin。

 And after that， to break their protocols was even more harder。

Pinning is a very good mechanism in many features。 like when you are upgrading software， for example。

 But when you're interacting with AI， you uploading data。😊，That should not be an obstacles。

 not to researchers like me who just want to know what。

 whether I uploading or to organizations who want to monitor their network。

 We should have the visibility here。This is very important today when we are interacting with AI。

 like we get a lot of marketing claims and promises and。These tools can。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_185.png)

Close the gap between their promises to reality。 So this is very important。

But I want to share one more thought I had from this research。 And， you know。

 we're gonna finish in a few more moments， and we still have time。

 So this is a really good point to think about questions if you have about my research。😊，So。

 one last thought。And during this adventure， rollercoer， we can call that。 You know。

 I started one day， I found many risks， many， many privacy concern about Siri Apple intelligence。

 You name it and。😊。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_187.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_188.png)

in the next day， Apple saying， okay， this is not Apple intelligence。 This is Siri about Whatsapp。

 about notion。 And I was very， very confused， especially on the fact that theyre separating Siri and Apple Apple intelligence to。

 two different things。 But， you know， their marketing。 I understand it like it's more of the same。😊。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_190.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_191.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_192.png)

And， know， I got more into it， and I got very confused。 Like one day I can just go to Siri。

 I can ask it about the weather。 and Siri will answer me by using Siri features。😊。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_194.png)

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_195.png)

But on the next day， I can ask Siri， okay， what is the weather vibes that I should prepare for。



![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_197.png)

And that will activate Cha GPT that is being operated by Apple intelligence。

So we have here two features。Which basically ask， I ask the same question， different phrase。

 and they activate two different features。Two different privacy policies。Two different terms of fu。

The same up。Me as a security researcher。 I can know that because after this long adventure I had in the last few months。

😊，But is an average user。Can he see that。I have one last question for you， for today。

Can you tell the difference between them。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_199.png)

Thank you very much for listening。 I've been honored to be here today。 and thank you for coming here。

😊。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_201.png)

I think we still have some time for questions。 So if you can just go to the microphones。

 I'll be more than happy to answer you。😊。

![](img/5fe1cc9c454f6256cc462d2c8e9eb89c_203.png)

We do have a couple minutes for questions if you guys want to line up if anybody has any questions。

Yep， I I got one， so did you use the virtual research environment。

 the local PCCC installation to do this research and is certificate pin still a challenge there？

m so I'm not sure about the virtual private cloud compute。 I use it on my Mac。

 I actually used some tools to break the certificate pin there。

 which I'm showing also on a blog that I'm gonna post。 so you can use the script I use。 But no。

 that was on the regular environment， which theory。😊，Everything is usual on the device， also in i O。

 but in i O， theres some issues with pinning not all the version。 I can bypass that。

 So most of my research was on the Mac O S。 I appreciate your work in verifying the promises from Apple。

😊，Thank you。A question about the WhatsApp interaction you had there。

 I'm wondering if you got the sense that the data that was sent particularly on WhatsApp and to the server on Apple had the same level of security or encryption that WhatsApp guarantees to at all。

 whether I don't know whether that you found any information on that or whether your outreach in Apple Apple came back to say。

There's a level of encryption or security that matches what Whatsapp offers when when using a service like that。

 Yeah， that's a good question。I'm not sure about that。 Like， you know。

 they do use certificate pin and SS all， which is very good tools。 But end to end， I'm not that sure。

 like end to end have a different meaning。 If I send to you Whatsapp message。 It's between us。

 And there is a third person here。 And I'm not sure who has access to it。

 I just know they're sending it to their servers， I'm not sure what they're doing with that。

 I have no clue about that。 I just know it leaves my device and it's not needed to do that。

 like I can， you see， I blocked our communication and picture is still working。

 So I'm not sure what they're doing in their servers。 Thank you。 Yes sure。😊，You have more time。 yeah。

 sorry。 first of all， congrats for your investigation。 Thank you。 And just my curiosity。

 did Apple try to understand how what you did to bypass the pin because by passing the pin is something that from their point of view shouldt be interesting to know the details or what you did。

 Yeah， they were not particularly interesting in how exactly where I put the hooks and how exactly I did that。

 But I told them I used some free the script and I used it on my device。😊。

They didn't care about that， yeah。Thank you。 If someone had more questions， I'll be here today。

 I'll be also on the Irish pub around 6 PM。 So feel free to come and ask questions。😊。

Thank you very much for coming。