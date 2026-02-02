# XUnprotect： Reverse Engineering macOS XProtect Remediator [1pJWqtBxb50]

Hello， everyone， I'm here to talk about reversing export remediator。My name is Co。

 I'm a cyber security research at a Presco Fro security。

 which is a Japanese security company conducting a wide range of security research。

 I barely working on Apple product security， especially Mac securitycur。

Previous I gave out at bracket A Eu and Quadbro。I want to say this presentation is a deeply technical look at the exp re or XPR。

 I walk you through the how XR detection logic works， What kinds of mar are targeted by XPR。

 I also talk about problems and books， which XPR utilize。Just to be clear。

 this presentation does not cover any evaluation of X。

 So I won't be discussing how effective it is as a Mac S product。 Also。

 I'm not gonna talk about the traditional X product， which is better every on Macs。

 So if you're interested in that， I highly recommend checking out this straight excellent talk。

So while you gain from this talk， you will walk away with a deep technical understanding of XBR。

 that's especially valuable for Bluechmer， but I believe this benefits Red Chima as well for Bluechmer。

 you learn how XBR detects and removeware this will help you understand its capabilities more clearly and you also get insight into Apple exclusive threat intelligences and better XPR。

This will help you understand recent mar that hasn't been publicly documented。For red chambers。

 you will learn about some vulnerabilities experience and problems and books。

 will help this knowledge help you discover new challenges in the future。

Here's the author of this presentation first I give you a background information so we are on the same page then move on the training I used after that I walk through the rebraing results along with a bit of vulnerability research and a founder if you'll come to your conclusion。

So what is exporting remediator in the first place。

 According to the Apple's platform security document。

 exporting remediator or XPR is a third layer of Macs Mar defense。

 The first and second layer include the Apples review notization gatekeeper。

 and the traditional export deck These two layerss are designed to prevent Mar from being executed or distributed。

 But X PRr kicks in after the Mar as executed。 It job is to detect or live Mar that is actively landing on the system。

XPO was introduced in Mars Monre as a replacement for model removal to Mt。

 Since then MT has stopped receiving app。 not only XP is applied typically once or twice a month。

 As of today， XP contains more than 20 scanning module and its believed that each one target the specific model family。

 For example， a binary named export remediator ar is a scanner that removes well known a hardware。

There other expert scanners like export re mediator， barcacher， Bluetop， bunlow， etdotrato throat。

So what is remediaish needed， It's because some sample can buy the first and second layers of defense。

 For example， in the 36 price chain attack reported two years ago。

 that Trojanized 3 C X app was notized by Apple， meaning that it passed gatekeeper checks and launched successfully free。

Also， some Mar uses social engineering to trick users into disabling gatekeeper In both cases。

 Mar can strip the first and second layers of defense。

 So Apple needs a way to detect or live Mar that's actively running on the system。But why do we care？

First， from an offensive security perspective， X PRR are target because they come with a powerful entitlement。

 like through this axis。 So bug in X PRR could potentially reduce to TCC bypas。In fact。

 Gary Kman published an exp on Twitter that demonstrates how to gain a full disk access from Li via one of the experienced scanners。

Also， Exp scanner， along with both do and user privileges。

 meaning that vulnerability Exp could potentially resist user to root privilege discretion。

From a defensive security perspective， XPR is also an interesting targetctic for reversing。

Since Apple tends to use its own naming scheme for Mar。

 several Marve families targeted by X PO remain unknown。

 Research like how to play out in Shi and fewstock have identified some of this。

 but several to remain unknown due to the lack in depth rebursing。😔，Also。

 Xo's limitation logic is too unclear。 It seems to simply stand fast using error。

 and they read in that match。 But is that all， thats still an open question。

The main research target is this a precaion bundle exported app。

In this banner and contents Mac director， there are 23 different scanning modules。

 They are all about2 megabys in size。There's also a binary NA export Act。

 I have to be confused with the Tra export Act。 There's an X PC service， Na export Pro service。

One not thing is that these XBR derivative boundaries are re in swift。Other can on last right。

 they include distinctive section name specific to suite boundaries like65 entry，65， type left， etc。

A director touch on related work， Howarddo G has published many books about XBO。

 Alden Shimi discovered that XBO contains secret strings that Ya ruless and Ppath and Facebook has compile list of XBO related Mar names。

But as far as I know， I haven't seen any detailed or diversing results of XBR。So next。

 let's move on to the drawing， how I did， what I did。

I rise more than 20s with boundaries using binary ninja。 Of course， these boundaries are stripped。

 but surprisingly some symbols can still be recovered。

 That's because I performed binary dithing using bendif and found that X scanners functions with Exp payload digest。

 This means we can impose the symbols exported by that diet into the X scanners。That said。

 reversing sp is still challenging because they are stripped switch bin。

 switch bin usually include typepo related symbol， things like type method accessor and particle width table PwT。

These symbols are very helpful for reversing since they tell us what kinds of objects are being instantiateated。

 but in this case， those symbols are stripped as well。

 so there's very structure information left to work with。But fortunately。

 even if the binary is are strip， we can still access a lot of type information in the binary。

That's because suite to support reflection Meadata is for reflection can be accessed through suite specific sections like certifiedified portals and certifiedified types。

 by extracting this metada， we can recover type information we can extract this type information using IPSW Sut command which is created by blacktop。

 but the extracted type information cannot be imported directly into a dec symbol。

That's why I developed a new B program called Bja Su to Uner。I've just published on Gitthub。

 this program is based on IPSSW Su， and it annotates type Me datata accessor。

 PW symbols and cross method to the disaster listings。

It also support other features that help switch reversing like switch to string analysis。

Here's a quick example without this plugin it's harder to detect what this function is doing。

 what kinds of data is reference， but after running the plugin。

 the function gets renamed to an appropriate type meta axisor giving us much more insight into this function。

Here's another example， as we can see before running the plugin。

 there's no type of related information available， so it's harder to tell what kinds of objects are being in Sun data。

 but after running the plugin a lot of type metadata gets annotated giving us much more context information。

I also developed a dynamic analysis tools to have things with reversing。

Switch to bin contains lots of inducted branches， such as call through Vi and PWTs and。

AResolving the top branch targets manually using LEDDB can tious。

My LDB script capture these branch targets and export them as a j file。

 The exportive data can be imported via a binja missing sync plugin。

Here's an example that captured index of branch targets can be imported into the binary ninger like this。

This program also annotates protocol written table and the function symbols when possible。

 for instance。F is called through Pw T。 This program annots which distract and which protocol are associated with that call。

So it provides plenty of helpful information for reversing。I also created some custom ADB command。

 The standard expression command of ADB doesn't properly handle complex suite objects like existential containers。

 So I built some enhanced comments for dumping more suite objects using these static and dynamic analysis tools。

 I was able to dig deeper into the X scanners。Next， let's move on to the reversing result。 First。

 I'll give you an overview of how limitation works。Here's an overview。

 The content Macs export binary is registered as both launch agent and launch demos。

 Its execution is scheduled by Du CS， which is a background task schedule that lands based on system activities and available resources。

When Exp project runss， it sends an XB request to Exp plugin service。

 which then kicks all the X scanners one after another。

Each scanner includes its own limitation logic， which is red using limitation better DSer and along with something called X X P program API while limit flat。

 the scanner also collect proven of files。 This allows them to identify where each limited file originally came from。

 and finally， limited flat information is reporteded back to Apple。Okay。

 then let's talk about how each Exp scanner is initialized。

Each experience scanner has a function called Bo in 0， which runs before its entry point。

 its load is to de sensitive strengths， useful remediation， such as fi path。

 ya ruless and li expressions。These sensitive strings are encrypted using a simple ex cipher。

De crypt of these strengths is important because it tells a lot about how each experience scanner works。

Al then from countriesres originally decryptive these strings。

 he also provided a nice videoja script for that， but some strings weren't successfully decrypted。

 He admitted that the output is not perfect。 Therefore。

 I developed a custom LEDDB script that decryptps all these strings。Here a few decpt results。

 we can say things like。Hushhis， fivepath， text messages and yellow rulesator straw。

After the model in fact0 finishes， the scanner moves to its entry point。 at this stage。

 the scanner creates an instance of its plug graph。 For example， in the case of the a load scanner。

 it creates an instance of the a load plugin。 Most experience scanner define just one plug graph。

Then the scanner creates an instance of the XPA A headquarters birth and passes it as an argument to the program name function。

So what is XPA helpers， it provides access to various types of system information。

 its capability largest of expplanatory based on its property names。For example。

 it provides access to launch these services， network settings。

Network settings and keycha and contents of process memory at atotro。

Let's take a closer look at an interesting property in XB P helpers。

 which is called alert to do property。 This property includes methods that use N alert to display dialogueg to users。

 I was surprised of this property because current Exp limits flat silent entryary without notifying the users。

I haven't observed ExpO making use of this property during my research。

 but the presence of this property suggests that G loss may be introduced in the future update。Now。

 execution eventually goes to the plugin name function。

 This function creates an instance of the XB logo graph。

 measure its performance data using OS samples， remove environment variable。

 which is a fixed the in for vulnerabilities， verifies entitlements of exploited plugin service。

 and inevitably rapid aging。And after ening win rapidid aging， the brain magic begins。

Let me briefly explain what vol rapidpiid aging is， since it doesn't seem to be wateringal。

This feature suppresses the access time a time up on a per process basis。 It can be viewed C control。

XPR appears to be enable this feature， possibly for performance improvement or to preserve forensic artifacts。

This is disabled after the limitation finishes。Next。

 let's take a look at how XPR implements its limitation logic。 Here。

 I'm going introduce a set of domain specific ground gauges called limitation builder。

 which helps describe limitation logic in a cool and concise way。 But before we get into detail。

 I'd like to start with my thought on why Apple decided to introduce their own DSL。

 Why was it needed in the first place。Reimation logic can sometimes get complicated when they do。

 the code tends to become quite ver。 For example， let's say we want to de file only if all the following conditions are met。

The target file is located on the library application support directory。

 itss file size is 2 MB or smaller。 itss format is macro， not notized。

 it matches a specific gallery loop。 and if the scanner is learning as route。

 and additional directory should also be scanned。To implement this logic。

 we might write called something like that。 First， enumerate files under library application support directory。

Then filters5 that are 2 MB smallerer。Then narrowttle them down to only macro fire。

Then checks which ones are not neutralarized， foundries can the file with the ya route。

 and they' read in match。If we also want to scan additional trajectory when learning gets good。

 we need to replicatecate this lo with extra conditions。This approach works。

 but it's not great in terms of leaderability and maintainability。If you want to add another console。

 you have to insert yet another statement， making the code even more cracked。

So how can we describe this logic more query and keeping material。

Our post answer is to use switch resort builders。So what are exactly your suite result builders。

They were introducing C 5。4 to make it easier to write domain specific languages。

 If you've ever worked with3 to U I， you've already used them。

 They you used to describe you are decorly。But they are not just for U More generally。

 they can be used to create a DSL for creating multiple elements and building a single result like Ger structural data such as Json H M L In X。

 P， Apple uses result build to combine multiple conditions into the final limitation decision。😊。

Let's look at a simple example of sweet result Peter， generating HTMLL。

 If you try to generate HTMLL dynamically in plain Swift。

 you'd end up with very verbal code decgrading intermediatement variable for each element。

 nesting the manually and so on。In this example， we want to add a specific H1 element。

 only if the use chapter titles variable is set to true。To describe this。

 we as a valuable div header， depending on the use chapter titles variable。

 It contains a lot of unnecessary data just by looking at the code It's harder to tell what the final engine structure would be。

Now， here's the same example。 But this time， it's implemented using the DSA provided by result build。

It's much more readable。 You don't need to decrease unnecessary intermediatemittted variable。 Also。

 the structure of the generated HTMLmL is clearly reflected in Su code these two blocks directly correspond to the two div elements and even the conditions and which elements are added or easy to understand for example。

 we can easily understand these HTML element is added only if the use chapter titles variable is set to true。

It's almost like writing your own template engine， but you within the DSL using sweet luage feature。

So what happens if we apply this idea to the media logic。

 remember the example from earlier by using a DS provided there by result builders。

 we can express this much more clearlyly。Here's what the same logic looks like。

You can clearly see file functionss that are being checked that target file must be under library application support directory。

 file size must be2 megats or smaller， file format must be Mac must not be nottriized。

 and it must match that specific gallery。When the scanner is running us through。

Additional directory is also scanned。 It's concise， read and easy to maintain， right。

This is exactly what Apple did。 X PRR comes with a set DSL called demediation build。

 It defines several ans， and each one provides a DSL for combining the limitation conditions。

For instance， if you want to describe logic for file related to limitation。

 we can use a DS S provided by file limitation builder。

 Other DS S S are also available such as process limitation builder。

 service limitation builder and software up extension limitation build。

Reitation data isn't used by all Exp scanners， as you can see here it's used a subset of the scanners such as our bygradu cardboard copper and so on。

The rest of the scan rely on a lower level API called X P plugin API for their implementation。

Ive documented the limitation build DS specifications based on my reversing。

 I published it on GiWory。 If you are interested in the details like how the DS works and what kinds of conditions are available。

 feel free to check it out。 Now， let me show you a few rare example to illustrate how this DSO is used in practice。

First， let's take a look at the XR acre。 This is a simple example that uses file limitation better。

As you can on the slide， a file is removed only if the path is 10 acre and the file size is 68 by or more。

 and it matchesed acre yellow room。 Yeah， pretty easy to understand。Next。

 let's take a look at the XPR algorithm， which uses the process limitation builder。

It remedias learning processes only if the process is not notarized and the back and file meets two conditions。

First， the Firepath mass contains certain strings such as library application support or temp。

 and second the F Ma match the Aulu the yellow room。Interestingly。

 the conditions for the backing file can be described using a file limitation builder。

To verify my reversing results， I implemented an open limitation builder。

 an open source implementation of limitation builder。

 It is a minimum implementation that reproducepros the behavior of XPR acre。

 This project can help you understand how swift result buildles are used to implement it。

 You used to implement a limitation builder。He。Alright。

 so let's take a closer look at the limitation logic of each scanner。Let's start with XPR launchf。

 which was introduced at the same time as XPR Rastack XPR Rastack is designed to remove the payrolls using a 36 supply chain tag as full XPR launch flight。

 its decscriptptive strings are just to hush values but for are these useful。

Here the limitation logic described using limitationeration data， as we can see。

 these two hash values are Cd hashhes。And it is designed to secure the process with those specific CD hashs。

The first city has is a no。 This is a second s payload using a 36 approach chain tag commonly different to as upgrade agent。

This sample was unwiseised by Patrick Walterder presented at the Blackett USA two years ago。

The second city hassh remains unknown， but it's likely a violent operate agent。As far as I know。

 only one version of the upgrade agent has been publicly documented。

 which I explained in the previous world。The non version has limited capabilities。

 It only sends system information to the C to server and doesn't perform any further actions。

So Patrick Walterter suggested that there might be other a more functional variance。

This unknown C has could support his hypothesis that other more capableiv version of upgrade agent might exist。

Next up is ExpR bloodgrager。Its dec cryive strings do not seem related to remediation like right click click open。

 So what are these useful。As it turns up， it's related to the marvel I mentioned at the beginning of this talk。

At the beginning， I briefly mentioned that Mari that uses social engineering to disable Gkeeper。

 The background image of this disk contains strings like Light Creekek。And C open。

That's exactly what X body culture is detecting Str in the background image of the disk。

Expl bar en numerous multi disk on the system， then it gets the background images and retrieve their ticket strings using O After that it's search get toke a bypass related strengths like Option click and right click。

 click Op and if it finds such strengths， it will report them to Apple including information about disk image。

So which Marvel family does explain about G detect。Honestly。

 I don't think it's aimed at any specific room。 In fact。

 X bar culture can detect disk images used by multiple modern families， like empire transfer。

And the Chrome router。 So Apple may have desired to expand barul as a threat hunting scan to help identify new and emerging threats。

Interestingly， XP Barcha used to have a mechanism to detect processes without their backing file。

 As you can illustrate， this mechanism is implemented using limitation build like this。

This mechanism has been removed probably due to a high rate of false pause。Clearly。

 this logic itself is also a generic form。 So this supports my hypothesis that X blood is a generic threat on scan。

Lastly， let's talk about Expo redb， which I think is the most interesting scanner。

Although it's now reed， it offers some unique insights。

Its decscriptptive strings a yara row on 45 path。Interestingly。

 Yaraarrow detect a triangle DBIos implant。TriangleDB is a piece of mar using operation trianggress。

 a highly sophisticated attack reported two years ago。

 Research Kaspirsky analyzed this attack and suggested that the macro version of this implant might also exist。

The Maccoos implant has not been publicly documented。

 but it is likely that expo lepi was designed to deject that Macos implant。

Exper that power is granted the entitlement column up system task called Po read。

 which allows it to read arbitrary process memory。It runs the surprise scans。

 one scan the memory of the learning processes and the other scan the loaded library path。

 The letter is called a loaded library scanner。The first first scan checks whether in memory iscutable match yaru and these scan platform processes are excluded from the scan targets。

Why does Exp lead by scan memory directory instead of their on disk backing file。

Normally other Exp scanners perform their scan on their own disk backing file。

 My hypothesis is that the mag implant long entire memory。

 leaving no choices on this to catch it scanning memory directory is necessary that makes sense concerning that IUS implant was deployed entirely in memory as well。

Next， let's take a closer look at the loaded library scanner。 Here， the limitation logic。

 It checks whether a process has a specific loaded libraries based on firepaths。 In this case。

4 part5path as specified F M core framework core location， A foundation live SQ 3 guide。

Here one very common in where they past themselves。Do these actually point to divifies？

When I look at this path， I noticed something strange， except for Lib E 3D。

 the others don't point to verify， for example， quaroation and foundation are just srings when these srings are loaded their path get resolved so the loaded libraries scan actually specify the resolved path。

Whats even more puzzling is that FM core framework is a directory。And clearly。

 no legitimate process a lot trajectory as a di。 So far what's going on here。

There are few possible explanations。Onehy is that this is just a darkling experience。

 Apple may have accidentally included that wrong path。

Another possibility is that the macro was implant somehow bypassic therapy and necessary and use that to depress sims or directly with at control。

But doing something like that would probably mess with a system。 Yeah， and Macs becomes unstable。

 it's pretty unlikely。So I direct to propose a third hypothesis。

The Macs implant used its own defective router， and loadeded library scanner was designed to detect that。

Researchcher at Kasperssky pointed out that I implant used a deflective bloter。

 and it's likely Macs implant implemented it as well。

 but when a dye is loaded through a deflective bloter， the parking file is usually empty。

This was pointed at bipartic water last year， and it is one of the few ways to detect refri loading。

But here's a thought。F with the implant set an arbitrary path as a barking file to meor。To test this。

 I implemented a new defory route based on Patrick's research using it。

 I was able to specify directory as a barking file。 Here's an example。 As we can see here。

 the output of VM map。 The directory path named F M core framework。 is shown as a barking file。

 This suggests that macro implant may have done something similar。

 usings a custom deflective routeer that assign a fake barking file to blending。😊。

But there's still some mysteries。 If you can specify any path at the bargain file。

 why choose a director of siming。Wouldn't it be more natural to use that and use library instead。

Another important thing is that Marcuss Redpoint never perform demediation。Even when it detects Marl。

 it just rips it to Apple， nothing more。 So if it's not designed to remove threads。

 for the actual purpose of deploying this scanner。Unfortunately， since we have obtained a sample。

 the answer remains unclear。Due to the time constraintsstraint I couldn't cover all my findings。

 but my revorcing results are available on GitHub now， please check the departure rate。

 it contains the divorcing result of 15 XR cameras。

 it also contains useful scripts to simulate the limitations of XR I hope they help with your future research。

Now let's talk about the problems and books， this is a cool mechanism that helps identify applications associated with limited files。

 To begin， let me show a simple example to understand how exp you try this mechanism。

Let's look at a case will explore lis scenario that establish persistence。

 say there three items are being limitedd， two Mar payrolls and pify on the launch agent directory。

Now， let's assume the cracked app drops the second stage payroll and the second stage pay rule then drops the third page payroll and the pages swap。

 The problem is just by looking at the limited of files only。

 There's no way to tell which app dropped these。Fs。

This is exactly where provide soundbox comes into play。

 When app is launched for the first time C PC CDD records information about it into the C database as part of that special proven attributes added to the craft app from then on。

 the app runs inside the proven soundbox at  anyifies it creates in the same proven attribute。

So when Exp attempts to remove files， it fetchs the provenance attribute and can ask like， hey。

 which app created these files？Then XPO can trace back to the correct app So through problem soundbox XPO can determine which app originally dropped these limited files In other words。

 Pro soundbox allows X to trace a file back to its source application。

 Think of it like this in the app soundbox files dropped by soundbox app gets tagged with that Q attribute and problem soundbox this type is replaced with the problem attribute。

Just like Appsbox， that Pro sandbox also propagates to child processes。

The problem attributes is set automatically for Southern fire operations as listed here。

 greatly renamed Sa ratio， et。The body of this problem attribute is an 11 Bt integer with 8 bytes being random。

Ss for city， generate this value and assign it to the target application。

So why does Exp correct Pro attribute in the first place because it's useful for discovering marine volumes。

 For example let's say multiple cr app are dropping the same second stage payroll。

 some of these apps are to unknown to Apple when Exp res this payroll to get the proven attribute to identify the source app and label that to Apple。

With this theatre， Apple can uncover previously unknown crack apps。

 This help upgrade exported yaaru and certificatevocationaries， notization status， accordingly。😊。

Proveance attribute also valuable for third party vendornder now limited to Apple。

 It says as a variable for artifact。For example， let's say we want to identify which application established persist。

If we check the proven attribute of the pages file in the launch agent directory。

 we can easily identify which app created。 We can also obtain signing information from the access policy database。

But this is just one example。 I believe there are probably many more ways to take advantage of this。

To demonstrate practical use cases of the provenance attribute， I've developed tool。

 which is now on GitHub， I've also added a proven data collection support to the Mac In frameworkra afterms。

I'll be submitting a product soon after this talk， and so hopefully it will be available for everyone to use。

Finally， I will talk about a bit of vna research。I touch on arbitraryified deletion of vulnerability inspired by talk to bag known as IQ Wir。

 which was discovered in several AD products。 The add is very simple。

 I trigger detection by I then before the detion occurred thus so with another target file。

 This allows the attacker to delete arbitrary file with amin privileges。

This research focuses only on Windows platform， but what about Mac S， It turns out what X had。

 It turns out that X had a similar talk to issue as well。

The export is very simple after yaarrow is matched depress the file using simings to another target file。

 you can determine when the ya rule is matched by checking the unify log as you can see here it logs yellowarrow sheet。

 this allows Atca to trick XBO into deing any file with its full disk primitive。

I also reported several ways to escape the Provience sandbs。 Sur。

 previous apps books bypass worked for Provience sandbs。For example。

 process launch via launch services was an easy way to bypass Proence onboxs。

 Another example was launching application through X PCC。These issues have already been addressed。

 but if you're looking to explore a problem soundboxs bypass。

 previous apps soundboxs bypass is a good place to start。

So let's wrap up with a quick summary of this talk。

 we will explore how to reverse an engineer XO and discuss its internal structure on limitation logic。

 we also looked into a proven sandbox its brief overview overview on how to utilize it and finally I shared a bit of validity to research。

This is a time constraint I couldn't cover everything that include their problems， sandboxs。

 internals， other U cases， other experienced scanners and the various bugs in。It's also。

 it's also worth noting that besides Expia， Apple has introduced a new module called exporting behavior service。

 This is a topic for future research， but I already have some results。 So stay tuned。

 I'm hoping to publish them later this year。 Additionally。

 we found that problems activity is also used by tracking getgi。

 This is another interesting target for future research。

Let me close with a few key takeaways Exp is a treasure tro of Apple's Inence think researchers should keep an eye on newly active scanner in future updatess The Pro at is a valuable foric artwork you can make use of it with tools of reviews。

Vulnerabability experience and provenence sound bookss are surprisingly basic。

 Bs found the other A with app soundbooks are often trajectory applicable。

 there may be still low hanging food to explore。Finally。

 I'd like to explain my gratitude to the people we study here。

I'm deeply grateful hard work and just on the for their insightful feedback on my presentational slides。

As you've already seen， I created a bunch of tools during this research。

 I published this on GitHub Posty， about of Hobins， I put them all together in this laundry poty。

 here's the link。It's also cur on the screen。My games are open on X so if you have any questions feel free to reach out to me through social media。

 I'm currently working on a white paper that will include all the technical details of this research I announce it on ExcelO once it's published so feel free to follow me on X yeah thank you very much。

