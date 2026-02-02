# Not Sealed： Practical Attacks on Nostr, a Decentralized Censorship-Resistant Protocol [O97xhyHFSsw]

Hever and thank you for coming decision。I'm He Kumma from NCD， Japan。

 And today I' will show you the protocol products on NA， which is a distributed Internet protocol。

And our wall paper appeared at the ITle Pre conference， USN P 2025。Today。

 and I focus on the implementation and code rubber dressing and trying to behavior that we didn't cover at our research paper。

Oh sorry。So。This is a joint work with a member from Japanese Research Institute at University。

 NC T and NEC C Corp and the University of O and the University of Osaka。😊。

So the red start with the with background and central social media can change policies or sometimes moderate contents。

So alternative， things like a masteron a blue sky and it also the north where to control your data and identity。

The two data presentation focused on the no， one of the crypt graphic distributed distributed SN S protocol。

So let's talk about the distributed S S。It can power two main models。

The right right hand side is a fedrated model， which is a platform like a masteron where a different server interpret。

 but identity is still anchored to servers。On the other hand。

 you can see the right hand side self serving Pra model is is a Pra noster where users hold their own keys and not server own id。

In this model。The server simply read the messages。 So it means the thrust shift to the client validation。

From7。And this brings us freedom， but also shifts all responsibility to the users side。So。

That's proed us to the two questions。 The part is。Can a client verify poverty key without the central server。

The second question is， do we very base the she of a new orthoox surface。

So we you like to answer this question by digging into no aspectss and the client code verification。

So。I haven't enjoyed this no detail yet。 So let me talk about it。

The not is the simple and the open protocol without a cent server。

Use have a private key and private key peer as their identity。There are over 1。

1 million users account， and anyone can run D server and users can choose any client on the application store。

And it supports the public post profile， encrypted dark messages， micropayment。

 and also march through device signing。So we's talk about the nota specification。

 and the noia comprises a set of the model specification device to the no implementation possible。

 the weighted carbon nis。And among these and poor specs。

 porky specs on the cryptographic perspective。And the left hand side is the the real basic specification。

 And that is defined to this rockhe， event rockhe and cryptographic signatures。

And second spec is ni H。 that is define the encrypted direct messages using the key agreement by the E DH and message encryption by the A SG VC。

And Northster also provided the the things， which can be used for for much device sign in。

This flud match device de throws of similar cryptographic structure at direct messages。

And the rat line is the need for 57， and it provides the micro payment。

So this study present three pre contributions。The first we has 56 prospectss， and 9 client。

Increase their open source and the aura。Proprietary things。

And we are also finding this seven vulnerability on this。And the demon rating 8 artwork。Second。

 we build the proof of concept each vulnerabilities， and we exploit it that breaking confidentiality。

 integrity and ability。The third contribution is we proposed the mitigation and worked with the developers over two years to record and fix these issues。

Okay， let's talk with about our findings。 So the v we identified far into the three primary categories。

 the integrity violation and breaking the confidentiality on the encrypted direct messages and the micropayment hijacking。

So they are not theoretical floats。 So they enable practical exploitation。

So we start with the three videos of our POC concept we used。

 So the first one is simply the for note event。

![](img/96003bd038fd0bf7b5b65d3245080782_1.png)

So the we hand side is abituous the screen。And the bo。 and now I like to send the for event。 And yes。

 the for for procedures are arrived that appears in the bo screens。

 So that is very simple for the for thes。

![](img/96003bd038fd0bf7b5b65d3245080782_3.png)

And second one is。P their attacks on the direct messages and breaking the confidentiality on the encrypt w。

So， right hand side is。Artcticas terminal and Artctica is trying to making the foldedged messages。

 It contained the Marss Arcticas URL。Okay， so writing for the work this。 And， yes。

 the attack now that creates the project encrypted message。

 And now it arrived at the message to the victim device and after that。 So the。



![](img/96003bd038fd0bf7b5b65d3245080782_5.png)

Secret part of the URL like authentication。Token in the appears in the to the server a serverboards。

The final idea is hijacking the micropayment。This is a combination of the4 ret on the direct messages。

On the4ier attacks on the profile。So which is similar to the first video。

The sur Bob public provide contains Bitcoin address。 Bob's Bitcoin address。

 So attacker intercept it and replace that advice to attackers address。



![](img/96003bd038fd0bf7b5b65d3245080782_7.png)

The right hand side is the article of the warrant。 So actually I would like to pay it with the above warrant。

 but。Yes， but artists， it doesn't work with and arts will receive the artist， yes。And which coins。

 so。That's the over of the rate of the cause。Yes， so to understand these burden。

 we need to look at the both sign and the implementation issues。So a point of view the design stage。

 the protocol views key is a very huge issue。And it also the maring kitchen like AC is also the really huge issues。

So on the other hand， the point of the implementation stage。

So there are common mistakes including skipping the signature of barificationcation or incomp to rebarificationcation。

Re cache violation。So it appears the different client from different developers。

So I think the reason is the different vulnerability appeared in the different client。

Is the caused by the unclear part of the specification。

And I think it raise the different client to behave in the different ways。

 which makes these security risk going too bad。So re removal on the step by step are tracing。

 So we categor the identified attack based on the specific cryography properties they compromised。

So next flight， we walk with the concrete example and code Web implementation for each pC。

So this is just a remark。 the normal flow for the as are sendingending the event to the above。

So let's go over how signature verification works in no3。

Each event should be signed with the author of the private key。 In this case， each signed。The。

 the art， the past using and hockey。And Bob received it。

 and Bob verified the event using our to the public key。So some of you guys are wondering that。

Whether there is any sortbar kitchen。So actually， the specification of noil does not require where it's about to verify signatures。

And。Of course， there some really robots implementation do check signature on their side。

 but they do this to save their own strategy。From the product from this spam。

 rather than the helped clients。So。And also， no specification says server do not have to trust。

Therefore， we set the targets that server do not verify the signature on their side and simplify even from center to recipient。

Okay， so， and this shows the actual client implementation。That is a really simple form。

 So many client even give signature checks。 So they。Do not。Bify， verifyify signatures。

 So it is especially dangerous for a profile event because profile has the bit address for users。So。

This rack of the enforcement over to invite the martious event to spread across the networks。Yes。

So at see， the kind of data can make forgery。Based on this。知。

The data can also include the encrypt direct C and par parts and bit for the micropayment。Yes， okay。

 just all robot。Of no can for。D to D Barnati。So it means the rock of the barcu of the signature is very simple。

But it is very impactful things on the X S。

![](img/96003bd038fd0bf7b5b65d3245080782_9.png)

So I would like to talk about the on code Web， code Web things。 So right。Work at something。

 but no signature checks at all。 So many client accessary mobile apps with barfur do choose their battery life。

So they are accept the event without checking the signature。 That's serious wrong。

There are no change checks on the court。Now， yes， I told。

I tolded the simple signature verification flow。 So let move on the another。

Sinceh that trash points and invest bypas significant barification on the dams。

 which is a popular I noal client。So at the first grant。

 the da qua appears to increase proper the signature verification logic like a this one。

And working with the D， we found the check chest before the acation。So the event cache checks in if。

 event event cache checks is on event D RD wide verified before， so。

So catch has their very far event。诶即。The function of the brave you returned the tool。

 And the other thing is。Reland force and the bare of signature。

So that is a summarize of the logic chain and export software。

 So when the event is not found in the cache， damageds problems standard the critical。

So to compromise the verification pass to a must gain the control of the decision point and making the return barrier over the verification function change to tool。

So if they succeed in making it return the true by injecting the f event with the forged I D and targeting the cache entity。

So an that does not require you breaking the cryptography。 It just by where the cache。And。

How can we the contradict point so we can use the event I D。

Now we made the articleer changes the content， but keep the same event ID， but same content。

So that is the content that represents in the artist Bitcoin address with articles Bitcoin address。

So write the fast work the normal communication probe like。To content and the tool event event R。

And after that， we can move on our attack as f when the bubble receive the fake event with the same R D that prior a event。

 but the content is modified by a。😊，So， okay， so then there's the。

Pobs device defines the cache and before it's verified result。Okay， so let's talk about the magician。

 So the that it's just a really simple thing。 So recification of the event I D and check the event I D that it contains the data。

And function the car entire just the hushing content。 So it just did very easy things。

So while the sum sum up to the product integrity that we have the barification signature barification on the red hand side。

 and we we can also have the cache event。

![](img/96003bd038fd0bf7b5b65d3245080782_11.png)

Then so the put of them together sometimes very bypaing can happen。



![](img/96003bd038fd0bf7b5b65d3245080782_13.png)

Okay， right step the back there at what the main did S N S。 So in the center right platform。

 the cryptography， if the crypttoography checks the field， the sub can still help by anything like。

Subveride verification。But the noa is fully self turbine system， and they are an centralized server。

 So aquars must check their own on the device。So this is meaning is a very quick kick from to the effective distributed N N S。

 So right now， record down serious issues high far text integrity and then encrypt direct messages。

So that is very and the simple problem。 the encrypt the are the messages。

 that is the key exchanges EC D H and encrypt by the CC。So， but， so as you know， so， client。

 I'm already skip signature verification。 So it' just。An A C C or without any integrity support。

So what does the attacker want， So attacker would like to change the message to the sand beam Bitcoin from the high。

So， but they want to， they， but they don't have the shares key doing the our。

But so how can they do that。

![](img/96003bd038fd0bf7b5b65d3245080782_15.png)

let's skip this right。And。The working how does CBC more than Christian whenever Marity。In CC mode。

 each point text box X with the in breath previous。Sar broke before in Christian。

So the attackers changes a few bit in the eyevy。 They can manipulate output without knowing the key。

 How about that is unable for， because that is the output is you random things。

So I to another way to produce making the messages to the giving bit coin。

So there are two types of the doctors here。 So right hand side is I told。You。

 and right hand side is un practical for you using the non print X and alpha X P。

So this is the computer simplified。Modification and the protocol for you。So in them in。

 in random preing， the attacker had no control is is just a getwork。 but non print takes。

 they can compute their different and use it to inject flight messages。

So remaining programming is how to get on the point and experience real system。

So the destruction solution is the crosspart attacks。 I mean。

 we can get a nonpne di cyber attacks P from another protocol inu。

So we can use the deigation protocol to get the。Non print text。 And this are for text。

So that is a normal pro。 and it just， it， is all almost same for as the direct messages。

 So between the arch device and radiation device， like disktopici。And in the Auto code flow。

 Auto code making the Mar Q code that has both private key and auto codes URL。

So then always this can， the Martious of code， the， the session。

 the Marious session between art and above will start。

And art and encrypt the nonmet data using this session key。

So article can get the non print text from met data and software text from this mar session。

So the takeaway of the typephertex integrity is the， the first one is the， you。

 we must use the authenticcate and Christian to provide cyberpherex integrity。

And second one is we must separate key between different protocols。Okay， so the rest topic。

 Ra topic is breaking the confidentiality on the encrypt direct messages。So。

Before they entering the attacks。 So I'd like to talk about the link reviewing messages。

The main messaging up， including some no client support the R preview and the link preview automatically refresh web pages metadata。

When the， when the messages increase the URL。So the type of the method crime side generation is have a side generation。

And now in this presentationation， our focus on the crime science generation。And so。

What kind of be the problem can each cost。The client side generation generation can be further divided into two types。

 The one of it is the center side generation， and the other side is the receiver side generation。

A very known issue is that the receiver generation preview， DRP advice can be rig。

It mean that it can be the private issue， but it just no， there are no confidential break。

So we think， can this behavior actually help us help me have a tattoo and and recover the pre text on the exhibited direct messages。

So。Canar still run the pras。Even if the encrypt algorithm is strong and safe。

So in the chapter of the Frog like security competition。

 we usually see the part work attacks using error over timing rigs data， you know。

So re ask Z world messaging have the features to react decorated content inbe ways。The answer is。

 yes， we can use the link for。Hoor。I a cold。So write the work there。

The attacks on the separate URL that is contained like a invitation for the U meeting or the shared link for the crowdest storage。

So here is how the article works the technical label。 And after a declarationation。

 the no their clients see the URL and they try to query。

And the the first and of the DN art series were to get the domain part of this the encrypted URL。

 like an example， net。The attacker never decorated anything in this part。

So they just went to the response。So now thet step further using CC Rability。

 the pre bit of the star text to change the URL domain and to the from the example to the M dot test。

So when the victim brush with the modified self text from the attack。

The link previews fly used to their art servers。But this modified URL had the secret part of the。

Ua work is a secret talking for the meeting invitation。

 so from the access rope on the attack suburbs， attack Chan gets the secret part of the U。And so far。

 I've shown on how the data account still part of the U rocketet tele but。What if they want to more。

 So what if they want to prove messages around the URL。So let explore how to do the next right now。

 they consider the different angle instead of using the preview recover non URL can be recovered non print text outside of the URL。

So。The rest is the attack know the messages status like K HTps。

 but they don't know the first three by of the messages in this case。

 So they flip byte in the IV force the satellite byte to be the age。

 It's just like1 byte word force and using encrypt the message for the。

If the client should had the age in the de data messages。The preview and prior you to the。

Artaer servers。 So the attacker can the guess the timing of the client has the。3 B at the age。So。

Our detector can then repeat the and apply to the second by and first byte。

 and the final can can get all of， all of the print text around this empty。



![](img/96003bd038fd0bf7b5b65d3245080782_17.png)

So that is a takeaway。 And that is the same as the cyberpher integrity。Okay。

 the after you talk the three takeaways were of the this presentation。

 The first one is it decent where。s tap the risks and over， so。Yes。And second。

 the takeaway is about the real attacks and how to stop them。The we showed， and these are text。嗯。

And the fake and profile and importation and changing the aggregate M D is using CC R VT。

And URL and Torick through link previews。And also， the parent is recover using raku rocket behavior。

The problems are not just encoding mistakes。 They come from the butt design and implementation。

So our results shows that strong political graphic algorithm are not enough。 So we need to。

I'm making the secure recordings。For the critical implementation and the cry system。So the take away。

 how can we design the safer decentcentized system。So in ride systems fix can be deployeded quickly。

 you know。But in the decent right one， many variant。嗯。De by the different。Communities。

So it is difficult to fix the boundaries。At the same times。

So we need to get the design right from the start。So that means the。

Clre rules for the signature checks， not just assign it。But how to verify it。 that is a really。

 really important thing to the specification。To be。Mo to be upstate。

The second item to improveability is a key inspiration， so。

Cryient must use a different keys for the messaging。Identity and derog。

At such things it hard to update is a deal rust。 the client must verify everything。And even from the。

Any event from the server。The root help be the protocol where cryptto and where we replace this thrust。

And。Yes， so that is a very important thing。 So either to repeat。

So the updates the distributed essence may take more time than the centralized ones。

Because there are independent， we administrators in their。AndSo many server implementation。

 And there are so many client implementation the different from the different developers and different communities。

So that is a summary of our works。So this was the fastful security analysis of NA。

Covering the cryptography and implementation fraud。

We showed many client skippped the signature checks。

 or it can be bypassing using the client side cache。And my application， like a CC model。

 let you change software text。 So we need to use the message authentication code or authenticated encryption to provide integrity。

So just sometimes important And things。 So outside of the critical things curve things like a link preview。

 so。Andm。Recevas side link preview break the private URL and token and messages when site projects can be modified。

So we also showed the fix and design trade tips。So that is the very important thing。

 The takeaway is the strong cryptography things is not enough。

 We need to good code and good a better crypt particle of design to making a decent white system secure。

So thank you for this things， so。We have the academic paper。

 and it would appears in the US conference so you can check the more detail a point of view the academic site。

And you can also check our website to the summarize of our works。And the URL。 And。

 and we can also find our contact point from the our Web site。Thank you for listening。

 So you can ask me and sing。After the decision。Thank you。

