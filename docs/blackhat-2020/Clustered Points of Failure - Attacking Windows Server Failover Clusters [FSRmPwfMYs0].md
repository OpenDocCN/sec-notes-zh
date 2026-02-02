# Clustered Points of Failure - Attacking Windows Server Failover Clusters [FSRmPwfMYs0]

All right， hello everyone， thank you for coming。嗯。So introduce myself real quick。

 My name is Garrett Foster。 I am a researcher on the team at Specrops。

 been there for a little over two years now， background and red teaming excuse me pen testing。

 spent a little bit of time working in a sock， doing kind of like security analyst work and then got my introduction into IT working as a help desk and all the way up to assisted Min。

Some of my responsibilities over at Specter， Ive focused on endpoint management software。

 and then as a researcher， my responsibilities are try to do some red team enablement and to find new attack pass to try to implement into the bloodhound graph。

Okay， so let's get into it。 So this slide is an Ldap filter。 And if you were to run this filter。

 it will show you every single clustered resource in the domain。

And my goal for this presentation is that you'll be glad you took a photo of this because by the end of it。

 you' want to search for these， whether you're on the red or the blue side。

 So on the agenda side of things， how are we， how am I going to convince you that that was a good idea。

 break it out into four sections。 Well have an intro。

 kind of get everyone on a baseline of what we're gonna be talking about。

 what clusters are and where they kind of like where they came from with Microsoft。

 We'll get into story time， talk about how I even got onto to clusters。

 Like what even LED to all of this and then do a bit of research together。😊。

And we'll cover some attacks。 So if we were to compromise the cluster。

 what could we do with that kind of control， And then how。

 how bad could we actually make the the fall out。 And then at the end。

 we'll give some kind of give you some defensive advice for remediation， detection and so on。So。

 okay， so let's get into it。 fail over clusters to kind of answer this question。

 We're gonna go back in time a bit。 all the way back to 1997。

 And I know some of you in this room weren't even born yet。 But in the spring of 97。

 Microsoft was holding event。 It was called Scalability Day。

 they were hypping up their the release of Windows N T4 do0 Enterprise edition。

 And a feature in that release was Microsoft's cluster server。😊。



![](img/503d47d6da41d135181480df15ac6c71_1.png)

So this is Microsoft's first introduction into high availability。

 I think it's pretty interesting that this blurb talks about how if one of your servers were to go down。

 the next one would pick it up because your one server would lead to the company falling over。

 So high stake expect them。

![](img/503d47d6da41d135181480df15ac6c71_3.png)

嗯。But I bring that up because this is the current definition that Microsoft uses to define what Windows Ser failover clustering is。

 So it's a set of independent computers that work together to increase the availability of applications and services。

 So in 30 years， the overall， the goal with the service hasn't changed since then。



![](img/503d47d6da41d135181480df15ac6c71_5.png)

So which applications and server services， the most common that we'll see today are file servers and easily the most common are databases。

So these two applications and services represent single points of failure。 So if the file server。

 the database server were to go offline， lose access， whatever happens。

 you lose access to those resources。

![](img/503d47d6da41d135181480df15ac6c71_7.png)

So as we kind of fix that， we have clusters。 We'll have multiple servers to be able to support that one resource。

 So if we lose access to one， then the next man up kind of picks up control of that。



![](img/503d47d6da41d135181480df15ac6c71_9.png)

Okay。So story time every week at work， we have kind of a meeting where the services team will get together。

 We'll talk about what we're working on and have kind of a team highlight。

 And at this in this particular meeting， we had a couple operators say， you know。

 they were pursuing this attack but it was successful， but they saw some really weird behavior。



![](img/503d47d6da41d135181480df15ac6c71_11.png)

So let's kind of walk through what they had seen。 They had just run the blood on collectors。

 So now they've got it up in the graph and they're trying to follow this attack path。

 So let's kind of break down what they were looking at。 on the left side of the graph。

 We have this relationship between the domain computer security group and a machine account。

 So they had control of a machine。 So they they had an identity that was a member of domain computers and had this edge right account restrictions。

 So right account restrictions is relatively new。 last few years， it was discovered by Dkyon。



![](img/503d47d6da41d135181480df15ac6c71_13.png)

And in his blog， he was investigating what happens when you stage computer objects。

 So if you were to create one， the default setting is that the permissions for who can join that machine to the domain is set to domain admins。

 But he was investigating what happens if you delegate that permission to a different identity。

 And what he found is it applies this this property set called right account restrictions and included in that property set is the permission to configure resourcebased constrained delegation or RBCD。

If you are unfamiliar with RBCD， a few years ago， my。

 my boss's boss Alad Chamir posted this blog called Waagging the Do Abbusing RBCD。

 It says it's a 41 minute read。 I'm still reading it today， so。Take that for what you will。 But the。

 the relevant piece of the blog that's going to apply for what we're looking into is that you can abuse RBCD to hopefully compromise that host system。

So this is what the operators were looking at。 They said， okay， cool， let's。

 let's abuse right account restrictions。 We'll set RBCD and then compromise that computer。

Because on the other side of that graph is we had a log on session for tier 0 users。

 So they're thinking， let's， let's pop the host and we'll just steal the credential material for this user。

 and that'll get us towards our objectives。So that's what they did。 We had。

 It was an nonobvasive test。 So like off the shelf tooling was fine。

 They pulled up some scripts from imp packet it， and set RBCD and tried to impersonate an admin role and then pop that system。



![](img/503d47d6da41d135181480df15ac6c71_15.png)

![](img/503d47d6da41d135181480df15ac6c71_16.png)

Only problem is they' were getting errors。 So we impact it if you've ever used it。

 errorsors are kind of a normal thing。 But the errors in this case。

 we were seeing something unusual is we were getting bad networking names to kind of like host name resolution problems。

 which。They were certain DN S was working。 and then they tried to use other tools from the tool set。

 So WM I， Xec S And B， whatever it may be， all of them were failing， so。They pivoted。

 They switched to， let's take that that RBCD ticket that we've generated where。

 we're impressing that admin。We'll just take it to a window system。

 and then we'll use it with scheduled tasks。 So they brought it over， past the ticket。

 launched the snap in for scheduled tasks。 They can just we'll launch that and then execute a payload there。



![](img/503d47d6da41d135181480df15ac6c71_18.png)

And it worked。 They were able to create the task， execute it remotely by proing it。

 and they got their call back。 So cool， were on， we're on the road let's go grab that credentialula。

😊，The only problem is when they took a closer look。

 they found that the system that they were trying to interact with。

 which is cluster doluus domain came back。 the payload executed on a completely different host。

 which is weird。 So， but they kind of kept digging。 And once they had that access to the system。

 You know， they were targeting a certain log on user。 And after closer look。

 they found that the account was there， too。 So we were on a completely different host。

 potentially where the log on session exists。So we're。

 we're all kind of brainstorming ideas on what this could have been。 You know。

 it's not uncommon to see these DA accounts or these accounts kind of living everywhere。

 So that was a potential explanation for the account。But the， the host names， those。

 those were causing issues。 So some theories were， well。

 maybe it's like a DN S alias that we're registering。 But to set a DNS alias or a C name。

 you actually have to set the service principal names for that alias。

So they took took a look at the SNs， and we didn't see any sea namess。

 But what they did find were fell over cluster related SNs。 So that's how we got here。

So it left me and several others with a number of questions。

 So why did scheduled task work when impacteded scripts failed？

 So what was going on that's different there。Why that host。

 Is there any way we can predict it or control the scenario of this。

 we're going to execute a payload again。And then。What's going on with session data， You know。

 is it just that that account is being used everywhere or is it something else like。

 is there is something else that we can determine。And then lastly。

 how does curbo authentication work， And no， not in general。

 I don't think anyone can answer that question， but mainly along the lines of clusters， like。

 is there something different or unusual going on with how Kboos works， which we'll get into。



![](img/503d47d6da41d135181480df15ac6c71_20.png)

So my usual flow of trying to get the answers to these types of questions is to just lab it out。

 put myself back in the shoes of the admin role and to deploy that service and see if I can just quickly get answers that way。

 So I'll follow Daniel's advice， get a lab set up together。 We try to get it as properly as we can。

 So let's do it together。 We'll have We're gonna do with threenode cluster。

 I've taken a liberty of getting some prerequisites done already。

 The featuress been installed on every system。 and I've got the storage set up。

 Those weren't really relevant to the conversation。 So I just kind of skipped over that。😊。



![](img/503d47d6da41d135181480df15ac6c71_22.png)

![](img/503d47d6da41d135181480df15ac6c71_23.png)

So we'll start from one node。 actually deploying the cluster uses an MMC Ss。

 It's called the failover cluster manager。 We'll pop that open and start creating the cluster。



![](img/503d47d6da41d135181480df15ac6c71_25.png)

So it's typically integrated with A D。 So we'll find the three nodes that we're gonna actually use to。

 to set this cluster up， Just add those there。 And then we're gonna create what's called an access point for administering the cluster。

So here， we're going actually give it the cluster and name。

 And then we're going to reserve an I P address for that cluster name。

 So we're starting to get a little bit of breadcrumbs about what might be going on。

And that's it for the cluster。 It's very simple。 It's very easy。 If all the preres that's check。

 everything passes， will'll have a cluster now installed。So the next step is to create the role。

 So I mentioned earlier that we had， the file servers and then the databases are the most common database。

 I didn't want to deal with messing that to creating that。 So we'll just do a file server。

 It's the easiest to lab out。So we're again going to create an access point。

 This one is called a client access point。 And we' give that a name。

 and then we're gonna reserve an I P address for that name。

We need some storage because we're actually hosting a file share， and that's， that's it。

 So now we have a working cluster and roll。

![](img/503d47d6da41d135181480df15ac6c71_27.png)

So what did that do， If we take a closer look at this at the manager now。

 you'll see that we are connected to a cluster do Lous。 domain namespace， our roles。

 So we created the file server role。 It's showing healthy up and running。 And all of our nodes。

 all of our servers have now been promoted to member nodes of the cluster。😊。

And now we have what's called a clustered network。 And this is things kind of get interesting here。



![](img/503d47d6da41d135181480df15ac6c71_29.png)

So in every node， after we set up this service， it's going to have what's called a network fault network fault tolerant virtual adapter。

 So it's a lot like when you create a VM， you'll see that virtual adapter being installed。

 But this is how all the cluster nodes communicate with each other。 They'll use this network adapter。

So there's just a few ports that are required to be open。

 So the firewall would have to be opened up between all the nodes。 The first we'll dig into is 3，3，4。

3 on UDP。 And this is where the the clothes actually， the nodes actually communicate their health。

So every second， a node is firing off a a packet。 It's about 50 to 70 by large。

 and it's in sequential order。 So 1，2，3，4， ping pong。

So what it's doing is actually a TCP packet that's going to get plumged through the net FT of adapter。

 that network fault tolerant adapter， come back， get UDP wrapped。

 and then it will egress the host through the physical， the physical ni。So I sent every second。

 So sent that out to every other node。 And that's intended to have that fail over that heartbeat。

 So if we lose， I think it's on server 2019， if you have up to you have 20 pings without a pg response。

 that's determined that the node is no longer healthy and can't be a part of the cluster anymore。

 So that's how the heartbeats work。

![](img/503d47d6da41d135181480df15ac6c71_31.png)

So the other side of the equation is we have R PCC。 So we'll have the static 1，3，5 port。

 and then we have dynamic ports for endpoint mapping。

And we can see what RPC is used for by just doing a quick wire short capturing and inspecting the traffic。

So this is just a capture of when I was connecting to the cluster using that snapin that we had seen。

 It kind of breaks down into three parts。 We'll see the initial bind  to 135。

 and we see the EPM or the map request for the cluster API。 So the snapin is providing RPC。

 the actual interface I for the cluster API。 and it's responding， hey， you need to go to this port。

 So we'll just come back and connect to 55602， which is where the interface is listening。

 and we'll reestablish that bind and if we're authorized you'll see us jump into those cluster API calls and actually interacting with the administrative namespace of the cluster。

Which answers the first， one of our first questions。 Like， why did scheduled test work， It's not。

 it's not a like。Interesting answer。 It's just simply that the snap ends work and are able to resolve the string minings and resolve those host names。

They impact scripts。 When you try to execute them， most of them are writing files to disk。

 So they're trying to capture the contents of those files and are resolving the host name that we're binding to that virtual namespace。

 And it can't find the the file path。 that just doesn't exist。

So fixing that knee packet of strips are just running with silent commands and not expecting output。

 All of them will work fine， which you'll see in a demo in a bit。



![](img/503d47d6da41d135181480df15ac6c71_33.png)

So let's bring it back to some of the stuff we were building out in the lab。

 some of the logical components that we'll have to dig into。 And those the V O， the CO。

 and then the node。

![](img/503d47d6da41d135181480df15ac6c71_35.png)

So the VCO is a virtual cluster object。 It is the computer account that represents the service itself。

 So in this case， it's the computer object of the file here。



![](img/503d47d6da41d135181480df15ac6c71_37.png)

So what we didn't see what was happening behind the scenes when we set up the lab is when we create those objects。

 it's actually creating an active directory machine account that represents this， this service。

 It has a machine account， I D and I D。So conversely， with the the cluster。

 when we set that up as well， is creating what's called a cluster name object。

 And that's the computer account for the cluster itself。



![](img/503d47d6da41d135181480df15ac6c71_39.png)

![](img/503d47d6da41d135181480df15ac6c71_40.png)

So the way these work is that each node in the cluster is considered a member server。

 and that can own or host the V C or CO resource at any given time。



![](img/503d47d6da41d135181480df15ac6c71_42.png)

It can only be owned by one node at a time。 so you can't have like multiple Vs。 And there。

 and you'll see kind of like there's not a lot of nuance to where they're going to be hosted。



![](img/503d47d6da41d135181480df15ac6c71_44.png)

So each of these represents a namespace， right， So we talk about those access points。 So the VCO。

 when we're connecting to that， willll have the clients connect to the V namespace。

 This is the users and computers that need to interact with that file share。

 So they'll connect there。

![](img/503d47d6da41d135181480df15ac6c71_46.png)

![](img/503d47d6da41d135181480df15ac6c71_47.png)

The opposite side is the CO represents the administrative namespace。 So。

 So when admin need to come in， troubleshoot the cluster， add more nodes， whatever it may be。

 they're going connect to this namespace。

![](img/503d47d6da41d135181480df15ac6c71_49.png)

So let's visualize it from the lab that we just set up。 So we have our three nodes。

 And then since we stood it up on that first node， it's the owner of both the B C O and the CL resource right now。

 So that's， that's the status of our cluster。If we were to lose node 1 and it failed。

 the V C O and Cena will move to the next healthy node。There there's not a lot of control here。

 admins can say， hey， I want this to be the primary failover node。

 But the point of this is that at any given time， any node can own these resources。



![](img/503d47d6da41d135181480df15ac6c71_51.png)

Which gives us some more answers。 So why that host， it's just a coincidence。

 when they connected to that connected to that virtual host name。

 the node that responded was just the owner at that given time。

15 minutes later can make a different one because theres some type of lag in the network。

For the session， what was going on there is that bloodhound， when it runs the session collection。

 it's using the net session on API， which is also RP PC。

So it's taking a DNAN S entry for that virtual name。

 actually running the net sessionession No API in the host， the underlying node。

 the owner node is responding with session data。 So it's actually accurate。 It's just。

 we have multiple host names for one， one resource。😊。



![](img/503d47d6da41d135181480df15ac6c71_53.png)

Which leaves us with Kibs。So。Let's walk through the Kbu handshake to show you exactly where。

 my brain broke。

![](img/503d47d6da41d135181480df15ac6c71_55.png)

So the client， we're authenticating to the domain。 We're gonna to come to the KDC。

 give us the secret。 We're gonna to have that authentication say， cool， here's my password。

 I need to take a granting ticket so that I can access resources on the environment。😊，DC responds。

 I have my TDT。So now I need to go to the database。 I have to come back to the KDC and say。

 here's my ticket。 I've already authenticated。 I need to access the database service。

 Can you respond with a service ticket， and I can take it to the database。D C is like， cool。

 here's your ticket。 I'm encrypting it with the secret of the service that you're trying to go to or that accounts password。

 I don't care。 All I need is a ticket to be able to supply it to the service so that it can decrypt it and authorize me。

So I have my ticket。 I'm going to come to the database。

 and then it's going to decrypt that for me and say， cool， you're authorized。



![](img/503d47d6da41d135181480df15ac6c71_57.png)

But this is where everything broke for me is the the whole point of that shared secret is that I have no knowledge of it。

 I can't decrypt the ticket。 I can't manipulate it。But in the attack breath we shared。

 we're taking a service ticket from one host and a a whole other host is decrypting it and is able to validate and authorize me。

Shouldn't be a thing。So I did what any sane person would do。 And it was assume the worst。

 So my first three was that potentially okay， every single node or every single identity is sharing a password。

 Then it was。O， maybe it's unconstrained delegation or something I'm not aware of or some type of impersonation with the cluster service。

And this kind of spiraling just continued when I looked at the， the node that we just stood up。

 So here's the， the first note。And now that we're more aware of what identities are are part of the cluster。

 things start to get kind of stand out。 So we have the the node that we're on。

 which is the test cluster， too。 And then we see that B C and the C O resource have active log on sessions。

 So makes sense theyre the owner of those name spaces at that time。

 So if clients are connecting to it， they need to be able to to decrypt those service tickets。

Only problem is， if you check the other two nodes at the same time。

 we have the same type of event happening。 So we have the node has a log on session。

 and the V C O and Scino have logs on every single node at all times。So。

It was easy to kind of debunk the the spiraling theories， dump the di。

 looked at all the passwords are all different。 There wasn't any type of delegation going on or anything like that。

 so。The only thing that made sense is that， okay， each node has access to these resources or these identities。

 passwords。So I Googled around a bit。 And without knowing what you're specifically looking for。

 it's kind of hard to find answers。But I found this blog by John Marlin。 He's。

 He used to work at Microsoft about two decades。 He was an engineer on the clustering team。

 And he talks about this repair actor directory object。 So this recovery action。

 So if you've ever done Siss had been work， this might sound familiar。So essentially， what this。

 this is doing is it's rotating the password for the computer object in active directory。This you。

 it。Excuse me。 So the idea with this is that you， you may have come run into see where a client has lost trust with a domain and it needs to have that。

 that fixed。What's happening there is that the password that the client has。

 because machine accounts are responsible for resetting their own passwords。 It's rotated it。

 But the domain， the D C doesn't have the new password。 So now we have a broken trust。

 So this is where my， my thinking is， is like， this must be what they're talking about。

So here's where things get interesting。 You see， see if this password is different from what's stored in the cluster database。

 like what's that， we'll have things that cause kberose errors。So the cluster database is a registry。

 a series of registry entries that essentially excuse me。

 that that store all of the entries and settings for your cluster。 So if you're setting up a network。

 the I address， the namespaces， all of those things live in this， this database。

This database is also present on every single node。 So my thinking here is like。

 if there is a credential somewhere， it's likely stored in here， its only problem is is where。

 So how can we find that。And then the block finishes up with， okay。

 so if we have that that problem happen， we could repair that。 And， and I'm thinking， okay， let's。

 let's watch this and see what happens if we were to run this repair function because my theory here is that。

 okay， this must be rotating the password。

![](img/503d47d6da41d135181480df15ac6c71_59.png)

![](img/503d47d6da41d135181480df15ac6c71_60.png)

So let's walk through it together。 We'll have Proc1 listening in the background and just try to capture all the events that are happening when we reset that。

 And there's a ton of registry activity。 If you've ever done Proc 1。

 you've probably seen how chaotic and overwhelming it can be。



![](img/503d47d6da41d135181480df15ac6c71_62.png)

But I've taken out the， the key pieces that are relevant to the what we're looking for。

 So the first is this call to the crypto container goodid。 Sore the value of that。

 And it's pointing to another goodI。

![](img/503d47d6da41d135181480df15ac6c71_64.png)

So we kind of follow along where we're going with that。

 and we'll see another queryries to this crypto checkpoint or a cryptography checkpoint。

And this is pointing to a crypto container， A CSB container that contains the private key of this。

 this certificate pair。 And then the data entry is just the hex encoded public key of that certificate。

It's okay， cool。 We're on the right track。 I'm taking cryptography passwords makes sense。

 So let's press on。 And the last event that we follow from that rotate is we see a reds set value to resource data。

 and the contents of that is this massive encoded blob that looks。😊。



![](img/503d47d6da41d135181480df15ac6c71_66.png)

Like， it's potentially encrypted。 wasn't E P API。 So I needed to。 So now my thinking is， all right。

I'm gonna have to reverse engineer this thing。 I'll give him my best effort。

 So popped in Ne Guro was having a pretty rough time of things。

 So I kind of called in back and reached out to a coworker。 His name is Ed McBom。

 If you've never met him， he has he's the author of the LA whisperhiser Project， super smart guy。

 a really， really strong with Windows internals and the level programming。

 and is really strong with Windows authentication protocol。

 So I figured this is probably the person to talked to。



![](img/503d47d6da41d135181480df15ac6c71_68.png)

Sa， hey， I gave it a shot。 He says， just， you know。Send me the， the binaries。I get on Slacks。

 here's the entire directory。 The closer service is the primary service。 She's like， cool， I got you。

 I'll take a look at this tonight。I try to say face a little bit。 say， he， man。

 I gave it my best effort。 I think I， I can see kind of how things are going。 I just couldn't quite。

 get it to the finish line。 And Evan just gives me a thumbs up reaction。 If you've ever met Evan。

 that's a good sign that something good is gonna gonna happen。😊，So four hours later。

 Evan comes back and he has a solution to my problem。

He found that the decryption has done this specific library。

 so is a dependency of the cluster service itself， and it's handled in this decrypt class。

And he knows this because Evan is a resourceful guy and add access to the private symbols for that library。

 and he sent me a screenshot of what he's looking for。

 Some of you might recognize the red arrow from Evan。

 but he's pointing to this decrypt class and that made it easier for him to take that PDDB or those private symbols to Giedra and start doing kind of like decompollate to start reverse engineering the binaries。



![](img/503d47d6da41d135181480df15ac6c71_70.png)

So he throws it in a gi。 It's like， okay， cool。 It turns out there's all these debug statements。

 And now we have the entire structure of that resource data bb。

 and we can kind of determine what's relevant to the decryption flow。

So we have a roadmap at this point。So he comes back the very next day and gives me a link to a gist。

It' was like no way， he didn't get this done already。So if you take a look at the gist。

 he's got kind of the proof of concept code at the top there kind of shows the structure of that data bb。

 the resource bb。 And if we go down to where we're actually going handle the decryption。

 it's probably going to look familiar to some。 It' your relatively routine CSP decryption。

 So we'll going to handle on that private or that private key what we're seeing with that registry query and then we're going decrypt the embedded secret that's included in that blob。

 use that secret to derive the encryptption key for the bb。

 and then we'll have the ciphertex we decrypt and have the plain text of that output or the decrypted ciphertex。



![](img/503d47d6da41d135181480df15ac6c71_72.png)

So it took us a bit to understand the format that was。

 that was come was the result of this decion because it was much larger than what we would expect a machine account password to be。

And。What we found was that it is actually storing the current password for the， the CNN on the V。

 as well as the previous。And this is by design and intended。 So this is all about high availability。

 So if we were， it was 11 AM on a Wednesday and people were using these resources and the password rotates。

 we would session stomp or ticket stomp all of those connections they'd have to re authenticate。

So it acts a lot like the KB T TT account。 If you've ever made the recommendation or been responsible for rotating that password。

 You're always told wait 10 hours or 24 hours preferably。

 And the reason is it would stop on all sessions。 So once we figured that out that the blob is storing the current。

 as well as the previous password to ensure that we wouldn't take a stop。

 we could decrypt everything。

![](img/503d47d6da41d135181480df15ac6c71_74.png)

So here's the， the output of what the， the P O C， once we've massaged the code a bit。

 is to under a week。 now we can decrypt the， the machine account passwords for both the CNO and the B C O。

So if you' ever mess with these passwords， they're all very， very long Uniiccode strings。

 which can be difficult for us to use。 So the tool will just derive the R C 4 N T M hassh of the scene on the V O。

 And now we can use those。So now we have the answer。

 like the overall of a high level question of how Kbos authentication works in the cluster is that every single node has access to the passwords for both of these roles at any given time。

 They're all in Ls。 When the cluster service starts up it checks。

 it does a log on for those accounts。 And it's ensuring that the credentials that it has are valid。

 and it can assume in the event of a failover， it can go from the passive to the active node and handle that service。



![](img/503d47d6da41d135181480df15ac6c71_76.png)

So。What can we do as attackers if we control this？So we have access to service credentials at this point。

 right。We have the CO and the V C O。 There is a Kbos extension called S for you to self or service for user to self。

 We control the service so we can use that extension to forge tickets for arbitrary users for services against our。

The benefit for this from the offensive side is there's really no protection against it。

 Protected users group doesn't apply。 We can't say like mark the user account control to say。

 cannot be delegated。So this is very， very beneficial for us。

So let's kind of walk through an example of what that would look like once we controlled that credential。



![](img/503d47d6da41d135181480df15ac6c71_78.png)

Okay。So we'll use get S T。 So we're gonna perform the Sfr to self。 We're using the。

 the hash of the the B O account to generate that service ticket。

 and then we'll just export it and be able to use it for some other tools。So using another tool。

 we're going to connect to the cluster API， we're in personating a cluster admin。

 and now we'll start doing some enumeration。So we well en all the nodes in the cluster。

 find each endpoint that we have available to us。 And then we'll check for the groups。

 And this is going to tell us what the cluster is。 and then all of the roles that are installed on that cluster。

From there， we can run the get group state command。 And this is will tell us who the active note is。

 who currently owns those roles。 So we're looking for。Like who owns the file share role。 we can run。

 We can simulate what we did in the beginning。 Let's。

 let's run the schedule task command with silent。 and we'll get a callback from there。

 all we have to do is just move that resource to the next node。

 And then without doing anything with the ticket， without modifying it in any way。

 we can just run the same command。 And now we have a callback on the other node。

So we do that same step again。 We're gonna move the roll from the the node that we just set it on to the third node in the cluster。

 And then again， without modifying the ticket without having to rerun that command at all。

 we'll just execute the task。 And now we have executed a payload on all three nodes in the cluster with one ticket。



![](img/503d47d6da41d135181480df15ac6c71_80.png)

So the result of that means that if， if we own the node， it doesn't matter how we got there。

 Whatever control of the node， if we have admin rights to it。



![](img/503d47d6da41d135181480df15ac6c71_82.png)

We therefore own the whole cluster。 So all of the nodes in the cluster and all the roles that it's it's hosting。



![](img/503d47d6da41d135181480df15ac6c71_84.png)

![](img/503d47d6da41d135181480df15ac6c71_85.png)

But could it get worse， Can we own the domain from this point of view。



![](img/503d47d6da41d135181480df15ac6c71_87.png)

To answer that question， we got to go back to the lobbying。

 I purposely left this bit out so that we could talk about it here。

So the CO has some extra responsibilities in， in the the cluster。

 It is the actual identity that is going to create and manage the VO。

So if you're going to have this CO， your cluster in an organizational unit or O U that isn't in a default computers container。

 you're going to need to give the CO certain permissions for that O U。

The permission that is required is only create computer objects。

 And this is the only permission that's required。

![](img/503d47d6da41d135181480df15ac6c71_89.png)

If we go back and take a look at the lab that guy that I was following， it is giving。

 It is telling me to grant the CO more than that。 So generic right。

 create all child and delete lead all child， which is way more than just create computer objects。



![](img/503d47d6da41d135181480df15ac6c71_91.png)

It gets worse when you keep digging and you start looking for different ways of of deploying these or or troubleshooting things。

 because one thing that they don't mention in the documentation very。

 very explicitly is that the computer， the CNN will also need the ability to modify DN S entries and so on and so forth。

Sort of compete。 So to fix that， you'll see full control， generic all generic right， C all computer。

 kind of grim cr all child objects。Just thrown around， which can lead to some， some problems。

So the the next few slides are gonna be some attack graphs that start from a CO compromise。

 So we have control over that virtual account。 We have control over that identity。

 These are from different enterprise environments across different verticals。

 So here's an example of where we have control of a C。 It was given generic right to an O U。

 And those permissions inherit down so far that we can go from a CO to a D C sync。

 We can compromise a domain from there。😊。

![](img/503d47d6da41d135181480df15ac6c71_93.png)

Here's another example where the CNO is a member of a security group that has been given full control of the domain object。

 which means CO to D。 C syncnc。

![](img/503d47d6da41d135181480df15ac6c71_95.png)

Here's another example where the CO is a member， has rights to a computer object that is a member of exchange trusted subsystems。

 which。I don't really have to talk about exchange too much。 It usually leads to a path to D。 C S。



![](img/503d47d6da41d135181480df15ac6c71_97.png)

And here's one of my favorite graphs is so we have the control of a C。

 and we can follow a chain that gives us access to a identity that is synced to Entra so we can go from onprem。

 jump up into Entra， abuse some of the intro permissions come back down onprem and then use that to compromise a D A。

 So from a virtual account that that that shouldn't exist or that should exist。

 but shouldn't be used has a path to D A from a high attack path。😊，So what if。

 what if the full control and the generic writer or whatever doesn't lead to a path like this in any kind of way。

 And we just have those rights to anno you。It was always useful to have that because you could。

 you could create computers。 You could You could create users。

 but it didn't really lead to compromise of the domain。 that wasn't a huge risk。 Like， yeah。

 there were some issues， but it didn't lead to like the whole thing falling over。So that all changed。

Earlier this year in May， when Yuval Gordon from Ecammi published this blog about Ba successsor。

 kind of broke the Internet when this came out。So what happened here is if you're not familiar is he was investigating some of the the services in the new features in server 2025。

 in particular the DMMA or the delegated Maned service account。

 And he found that if you were able to create this identity or create this object。

 you could use abuse that object to impersonate privilege accounts and ultimately compromise the domain。

So。You needed the ability to create computer create the object。

 which we've seen is coming from those cluster objects。So he published this。

 And then like five minutes later， Logan Goys and some other developers。

 they published proof of concepts on abusing this。

![](img/503d47d6da41d135181480df15ac6c71_99.png)

And that same day， I started seeing messages in like bloodhound slack and discord channels where they were talking about。

 hey， what are these weird cluster accounts I'm seeing that have these permissions because people trying to。

Audit their environment to remove this。 So they're looking for those permissions that we've just talked about。

 So starting to get attention， people are starting to see what was going on。

So let's back up from just focusing focusing on the domain compromise。

 and remember what the clusters are intended to give high availability to。 It's not just the domain。

 It's the services and resources that we we're clustering。So earlier this week。

 Chris Thompson has saw SQL， right， our databases are， I said that that was the most common。

 far away， the most common clustered service。 So now it's on bit of a microscope。

 Chris Thompson had published a blog where he had developed a bloodho collector that is focused entirely on MS S SQL and analyzing the permissions that are associated with that。

😊，I'm not sure if you've ever tried to look at MS SQL permissions。 It can be a mess。

 but this has made it easier。 So it's going to be much more accessible to users to， to dig into this。

What else？ It's not just MSSQL。 So ADF S has the option to have always on availability groups。

 We can look at those earlier this year， there was a blog by a post by Max Keasley。

 where he was investigating a attack pass from the database side of things and compromising it through。

 through that route。Exchange， exchange uses a database database availability group。

 So if you're still managing this onprem， I'm sorry， especially recently with new Cs coming out， but。

The attack path we shared in the beginning actually started from a CO in an exchange D or a database availability group。

 So there's your proof that this is kind of a real world thing。Don't have any references there。

 Everyone knows how bad exchange is。 SM or configuration manager， the brain of the entire thing。

 So all of your endpoint management， the brain of the entire thing lives in the database。

 So if that were to fall over， we lose access there。

 So you have high high availability configs there。 So clustered resources on the database。

 And SMs gotten a lot of attention recently as far as text and domain compromise。

 And then the last one to share because this one's interesting is there's even Microsoft published documentation for clustering A or active directory certificate services。

If so four years ago， this was actually presented here at Blackca by Wil Roder and Lee Christianensen。

But with their talk certified preowned。At the time， they published say they had8 E S Cs。

 I think were're approaching almost to 20 at this point。 So， but they're actually clustering this。

 this service as well， which。Is interesting。So how common are clusters。

I had a co kind of query all of， the blood enterprise tenants that we have。

 And we found that it was in 82% of the environments that we have access to。

So 82% of the environments had at least one cluster， one clustered role。

The average number of unique clusters in each environment was 141。And then the top end of that。

 in one environment had nearly 2700 unique clusters。 So that's a massive attack surface。



![](img/503d47d6da41d135181480df15ac6c71_101.png)

So how do we go about fixing this。 What do we do to kind of solve this problem。

 And it's the first step， Its really easy。 Like we only need that one permission in the the O U that we're building the clusters in。

 so walk the permissions back， only create computer objects。

 So we're gonna audit those cluster virtual accounts and see if they're given those excessive permissions and try to fix that。



![](img/503d47d6da41d135181480df15ac6c71_103.png)

There's that spend one more time or that El filter one more time。 If I've convinced you。

 I'll give you a sec to take a photo real quick。

![](img/503d47d6da41d135181480df15ac6c71_105.png)

But on the detection side， we have。 So that client access point， right， When we registered this。

 we assigned and reserve an I P address for that， that identity， for that object。



![](img/503d47d6da41d135181480df15ac6c71_107.png)

We now know that it's also authenticating from。 So it'll authenticate from here。

 but we now know that it's authenticating from every single node。

 So we have a list of expected log on locations。 So if we see a authentication from any different source address。

 we have a reasonably high fidelity alert that something malicious is going on。😊。



![](img/503d47d6da41d135181480df15ac6c71_109.png)

Kind of keeping that same thought process going is where like， okay。

 what can we What can we monitor now that we understand that the clusters， these。

 these credentials live in registry and they're stored in this resource data key。

 we can monitor this。 Only the cluster service and maybe it its child processes like the resource control monitor will need to read the value of this。

 this entry。

![](img/503d47d6da41d135181480df15ac6c71_111.png)

So we can create a cycle， which are always really strong for， for detecting malicious activity and。

 and trying to see if any other principal is trying to read the contents or value of that attribute。



![](img/503d47d6da41d135181480df15ac6c71_113.png)

F。That was a duplicate slide in my bed。 Okay， so key takeaways like， what。

 what I want you to leave this， this conversation with。Number one is， if you own the node。

 you own the cluster。 It does not matter how you've gained control of it in any type of way。 If you。

 if you own one identity in there， you're going to own every single other identity associated with that cluster。

And then lastly， that second， clusterist configurations can lead to compromise， purposely vague here。

 because it's not just the domain。 It's the compromise of those clustered services。

 which which are frequently tier 0 and the things that would keep you up at night if they were to fall over。

And the next piece is set it， but don't forget it。 Remember。

 I I started this whole thing off saying this has been around since 1997。

 clusterluing is more than active directory itself。 And we're creating these virtual identities。

 right， So these are these are A D accounts that aren't mapped to a host。Over time。

 the clustered resource， so， the nodes themselves are going to change。

 We're going to have operating system upgrades。 It's going to go from bare metal to virtualization to maybe even up in the cloud。

But these virtual accounts， these identities aren't going to change。 So the idea here is。

 don't forget about them。 Treat them the same as the the active accounts and give them the same type of respect。



![](img/503d47d6da41d135181480df15ac6c71_115.png)

But that's it real quick。 I wanted to acknowledge some people that kind of contributed to the conversation。

 the biggest one being my wife， Riley。 She is the reason that I'm here。

 And then a lot of the friends and the team over at Specter ops。But that is it for the talk。

 Thank you。 This is a QR code that will link you to the repository for the tool that I wrote to kind of interact with the RC。

 It has a link to the proof of concept code for decrypting those credentials。

 And then the readme has a high level conversation of everything that we just talked about here。

 The detailed blog is on the way。 I just have to finish writing it and get it through the Q A process。

 But that's it。 Thank you so much for attending are there any questions I can answer。

 I've got a couple minutes。 Otherwise， actually be heading to our booth to answer questions。

 But I do have some time。 Are there any questions。😊，Don't see him。 So if they're oh， my better。

 There is one right here。 What's up。Yes， it'll be in the b。

The the question was in case you didn't hear is do you have the blooddown queries that are relevant to find the clusters themselves。

 Yes， we also have to extend some of work on some of the collection methods in blooddown to streamline that。

 And that's my responsibility。 So that's in progress。Any other questions？ I know it's lunch。

 And you guys are probably all hungry。 So that look like it。 Thanks again。 I appreciate it。😊。

