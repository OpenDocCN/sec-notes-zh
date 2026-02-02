# Protecting Small Organizations in the Era of AI Bots [S5DJtN1FDYo]

Thank you for having me。 So I'm gonna to talk about protecting small organizations in the era of AI bots。

 And I think a good place to start this discussion is to talk about the fact that like 51% of the Internet is now nonhuman traffic。

 So this is a report by Ireva 2025， the bad bot report。 51% is nonhuman。

 which means that we've kind of passed a threshold where machines are now a major part of the Internet。

 they are kind of taking over the Internet in a sense。

 and 80% of malicious bot Is were not listed in popular I blocklists。 This is by Chiiago in 2021。

 they observe that these blocklists that are publicly available。

 a lot of the malicious Is are not not available because they're changing so rapidly and changing so frequently。

 So I want to talk about this kind of bring this down a level。 This is kind of a global perspective。

 B this down to kind of local organization perspective。

 So I work with a community science Institutesitute。 It's a public nonprofit promote scientific。

Literacy， volunteer water quality monitoring and certified lab analysis for Central New York。

 The Community Science Institute is a small organization。 They provide a few servers。

 not a large not a lot of number of servers。 and they provide this data for free to the public。

 So they provide this kind of public service。 They encouraged citizen science。

 And they provided a database lab for stream lake chemistry for harmfulgal blooms and biomodoring。

 And what we observed is that a single server was receiving 150000 page hits over 20 days corresponding to over 7000 hits per day。

 So they were getting a lot of traffic。 We knew that this couldn't all be attributed to humans。

 of course， because this was kind of a lot more than anticipated。

 The traffic was so severe that was degrading server performance for Ci for known users and clients。

 So it was kind of degrading the performance of the server。

 and we clearly could could attribute it mostly to AI bos an early investigation kind of just looking at log logs。

Kin of directly noticing that visitor traffic is from the entire world。

 despite the fact that the CsI database is entirely data for central New York State。

 So even though their data is provided for central New York State。

 we're getting traffic from all over the world up to thousands of machines touching this one server to try and gather data and gather information。

 So it's pretty easily attributable to AI crawlers to crawlers trying to gather data to train AI machines。

 What tools are available。 I'm going to very briefly mention them throttling is ineffective because actually most of these crawlers observe rate limits。

 We found that they actually observe the throd wheeling limits。 Public block lists are ineffective。

 up to 87% are not listed， Gr is ineffective because it's very low level。

 You're just getting single Is and doing searches like this。

 Go access W stats are's not effective because you're getting summary statistics that hide those details about whether something is a machine or non-human。

😊，Real time monitoring do not examine historic log patterns。

 So we wanted to really understand what was going on by looking at logs。

 And there are newer techniques that are focused on AI using AI to detect activity。

 but they often depend on having really good pretraining data。

 on having really good separation between human and machine， so。

That's a challenge because you need that kind of baseline in order to train AI models to do this。

 So we're taking a non AI approach to handling an AI problem。If we look at， for example。

 just a quick look at go access， you can see that it tells you by day what kind of traffic you have。

 It tells you like here's the traffic you go ball you would get on particular days。

 but it doesn't distinguish。 You can see right there。 It says including spiders。

 So it's telling you right there in the description of their interface that actually it's include spiders。

 So it's not distinguishing between human and nonhuman users。

So one of the methods that we're gonna approach this with， How do we。

 How do I solve the problem with a client？ So I the question I think that really comes down to is。

 how do we distinguish human access patterns from machines。

 Can we distinguish the limit the way that people access machine people access servers versus computers。



![](img/5e84d347fb22878174f9c0082cc6c31a_1.png)

And I， I've never been in a submarine。 but I think that there's probably like。

 from I watch enough movies that there's probably a radio operator either in there。

 like listening for traffic， like， okay， is this a enemy sub， What does it sound like。

 hass that mechanical pattern？ It's very repetitive， right？

 So you're listening as a radio operator in a submarine versus a whale or something natural， right。

 So that's， I think the approach that we're taking is， does it sound like machine。



![](img/5e84d347fb22878174f9c0082cc6c31a_3.png)

Does it sound like it's mechanical， And that's an approach that we can take to looking at traffic。

I was inspired by a visualization that was done by Jung Yi Kim in a paper called Web server log Visualization in 2018。

 in which he plotted time versus host I P。 And that gives you kind of an overall perspective。

 So the first thing we did was to visualize this for the CsI data and plot time versus host I P And the beneficial is visualization is that you get the entire log in one snapshot。

😊，The other benefit is there's no statistical summary。 There's no hiding of any data。

 You're not getting a statistical like averaging of the data。

 You're seeing the entire picture of all the log in that one image。

 and it's easy for humans to see patterns in that data。

 It's easy for us to recognize what's going on。 So I ask you， if you look at this image。

 This is kind of like the sonar guy listening to the radio， listening to the Internet。

 What do you think is human here。What do you think is human， right。Yes， that's right。

 The the things that are very regular and repetitive。

 remember the time is on the horizontal axis over 20 days。

 So what's human here is the things that are not regular， not mechanical， right。

 Those mechanical things are the repeated dots， the dash patterns， right。

 So we can say that these things are all probably not human。

 and there's some interesting patterns here。 Of course。

 there's a lot of other stuff here that's not human。

 But these ones are particularly interesting because they have patterns that are that are still mechanical。

 but they don't follow a straight line across right So the straight line across is just your attacker who's trying to grab things just continuously throughout the day from one I。

 but these other ones are more interesting because they have they're still mechanical。

 They show these kind of sound features of being mechanical， but theyre they're more complex。

 So we're interested in distinguishing mechanical access patterns。

 regardless of whether they are benign or malicious。

And the reason for that is that we found that a lot of them are observing rate limits， right。

 A lot of these crawlers are observing rate limits。 so they're not acting maliciously necessarily。

 How fast are you， no more than 20 pages per minute is like a common rate limiting approach that you would take。

 We found that most traffic was observing the rate limits。

 Most traffic was because they basically knew if you think of it promo policy perspective。

 they knew that if they tried to access faster than that。

 then the classical throtling would come in and block them right。

 So these data centers and crawlers essentially observing rate limits。

 They're following the rate limits so that they can try to get access。

 So that's not enough to just try to do rate limiting。 If you do rate limiting。

 it only reduces the traffic by 33%。 and those red dots there are where rate limiting is happening。

 those are ones that are kind of still acting very aggressively。

 much they're faster than 20 pages per minute。 and it only reduced the traffic by roughly 33%。

 And that's actually a kind of a little bit over inflated because they're actually a large number of hits。

Happening from those， right， you can see that all that other traffic is still happening。

So what other patterns that humans would follow， That's throtling。 How fast are you， right。

 We saw that one already。Con how often do you visit is an interesting one。

 So this is like this is starting to get into human computer interaction and borrowing some ideas from human commit interaction。

 That field studies a relationships between computers and humans。

 And I think there's a lot of interesting overlaps between that field and security。

 human computer action says， how do humans use machines。 How do people use machines。

 And if you think that machines are basically the inverse of that。 So we don't operate very fast。

 we we are slower at navigating pages。 But there's other things。

 We don't access a machine for more than five days in a row， perhaps。

 or especially if you combine that with daily ranges。 That is。

 most people can't work more than six hours a day。 And if you're browsing one page for three or four hours a day。

 that's pretty unusual， right？ And especially if you're doing that consecutively over multiple days。

 So you combine these human factors and say these human factors actually help us to understand which things are machines。

😊，Daily hits。 How many pages do you look at in a single day。

 If you're looking at more than 400 pages a day， youre at our single server。

 you're probably not human because that's just really a lot of traffic。

So this brings in behavioral science and human computer action into the equation and says there's this interesting relationship between HCI and security。

So we developed a kind of metric based on this， and that metric uses human behavioral metrics to develop a scoring algorithm based on IP hashing of the key value map to raw pages。

 we sort the pages by day and time， so we sort those pages because we want to look at things over a daily range。

 humans operate on a daily basis so it's kind of useful to look at days and then we apply these behavioral metrics to the IP numbers on a daily range。

 The scoring is based on a weighted contribution of these different metrics。

So here's some intermediate results。 Here's the original traffic。

Here' is blocked by consecutive days with daily ranges。 and you can see that the blocking is really。

 really pretty good at getting those kind of long， continuous things that are accessing the server over a continuous period of the day。

 a large daily range。Even things that are kind of like dotted repetitive。

 It's still getting those things like those that one in the middle。

 that's like a dashed line is like smart enough to get different time periods the day。

 But then doing it for a significant kind of like focused time。

This is blocking daily range with frequency。 So you have to be。

 this is kind of what we call smart smart throtling， basically like pages per minute。

 but you also have to be over an average number of pages。And we block daily maximum。

 There's very few that are trying to do this because they're kind of very。

 there's very specific and trying to get lots of traffic very quickly。

 And then commudo filtered results。 This is， this is the conclusion of all of those things combined together。

 And you compare this。 This is the original traffic。

And this is the filtered community filtered results。 You see that it's a much better picture。 right。

 If you think of it it as a human， like analyzing this visually。

 we've probably gotten rid a lot of mechanical access patterns here。

 But there's still some things happening here at the bottom， right， It didn't get rid of this。

 It didn't get rid of these dashed lines here。So how do we clean that up。So， what we notice。

Is I'm gonna to take you zoom in on a little picture of here。 Remember。

 this vertical is the total Ip range。 So you're looking at a 4 billion。

 The vertical axis is 4 billion， right， The entire I range。 So it's very dense vertically， right。

 horizontally， it's dense as well because you have seconds down to the second across 20 days。

 vertically， it's very dense。 But let's take a close up of a section here。

 If we zoom in on this section。 What we see is kind of these very each one of those little dots is a hit。

😊，And if you look at just this part here， these two different lines。

 they're actually distinct in the way that they appear close up。 This is a single I。

 It's being hit multiple times。 It's from one I number。 Remember， vertical access is I number。

 And this is what multiple Is look like。 it's scattered across a line。 Now。

 if you zoom out and look at the full picture。 it's hard to distinguish that。

 because it's just a data center。 But this is a data center attack。

 because it's happening across many Is within a small subnet， a class C subnet。

 right And so this is what a data center attack look like looks like in this visualization。

We can if you look at the raw data， it's very easy to see that because you basically see this group of IP numbers like 00。

1。23。 that whole section has very consistent traffic statistics that is are hitting a certain number of pages at a circuit frequency。

 And so this is clearly a data center attack， basically trying to gather data from this site。

 So it's pretty easy to identify once you know what to look for。

 you look for a certain frequency across a subnet。The question was how to automate this a bit and how to make this more practical。

 And so what we realized we could do is basically collapse down all of the I Ps in a subnet and then do our scoring on that。

 So what we do is take those I P numbers。 We collapse them down to a single to a single subnet and aggregate and score that subnet separately from the individual I Ps。

This is the entire algorithm just in brief for those that are technical。

 The entire algorithm basically takes in the access log。

 It performs structured hits to get the access。 the log data into a structured format performs hierarchicalical I hashing。

 So we I hash the first stage and do metrics on that to get the I level blocking。

 Then we do an hierarchical version of that where we mask off the lowest byte。

 And that gives us the masking at the next level。 We then score that level and then go to the last level class B subnets and also do scoring at that level。

 So it's the process of basically aggregating by subnet。

 and then doing that hierarchically and aggregating at higher level subnets。Then finally。

 we take the total commmative result， and we output a number of output files， helpful。

 useful products that are used for that， that purpose。 So the final result。😊，Is basically this。

 So the filtered result， prior to subnet hashing。This is a filtered result prior to doing subnet hashing。

 And this is blocking class C subnets。 So these are subnets that are within within a 255 range And this is blocking class B subnets that also show patterns。

 So remember， you do scoring on the subnets。 It's not just saying， okay， we block subnets， right。

 You do scoring on those subnets similar to the way we do scoring on individual I。

And the result is this。 This is the final result。And you see the difference between this。

 the original traffic on the left and the final result on the right。

RightThis is the goal was to try to distinguish human access patterns from machine access patterns。

 And we got most of the way there with the behavioral scoring and then the subnet hashing。

 hiarchco hashing guesses the rest of the way there because we are basically able to block those subnets that are pursuing that mechanical access patterns。

 So this is a really， I think a big improvement from what we had before。

 And it shows that we can actually kind of like just using behavioral scoring techniques extract out what is human versus nonhuman to a much greater degree than just throttling alone can achieve some of you are probably noticing this vertical blip there。

 And I'm going to get back to that you guys who are really hackers in this community can probably guess what that vertical blip is there。

 I'm going to come back to that。So that we also do estimated load analysis。

 And this is just using a very simple kind of fixed rate。

 you assume an average page hit like response time。

 like maybe it takes 20 seconds or 60 seconds for a server to respond to something。

 So we assume that kind of some average。 And that allows us to create an estimate of log sorry bit of load analysis over that time period。

 So the original traffic is the gray stuff you see in the background there。

 That's the original load traffic。 Class C filtering takes out those things that are purple。

 So things that are purple or kind of lots lots of class C traffic happening at that time。

 Class B is the blue stuff is also being taken out。

 And the final filtered server load is the bright green So we're going from what's in the background to the server load being what's in the foreground。

 that green that bright green。We observed a 94% reduction in traffic using this technique。

 which is significantly greater than throttling alone。

 So with throtling and those estimates of the workloads are there averagever request per minute。

 you get a stage reduction of 33%。 if you add the consecutive filtering， you get another 9%。

 if you add daily ranges， you get another 9%。 they daily maximum ads another 3%。

 And then when you do C net blocking， you get another 14% and B subnet blocking gives you another 26%。

 So you get a total 94% traffic。 this corresponds very well to what we estimated for the community science Institute in terms of how much AI generated traffic there was And if you look at the results。

 you'll see that that is kind of corresponds pretty well to what we see right this looks much more like now human access patterns on the right than what we had on the left。



![](img/5e84d347fb22878174f9c0082cc6c31a_5.png)

![](img/5e84d347fb22878174f9c0082cc6c31a_6.png)

So we found that even when well behavehaved and observing rate limits。

 the sheer volume of AI bot requests can overwhelm suburbs for small organizations。

 This is why we didnt we don't really look at all at request at the request at all。

 right So there is there an SQL injection， Is there an attacker， We just don't look at that at all。

 because the problem is really on a different level。

 it's observing the fact that now there's so much traffic that even benign well- behavehaved crawlers。

Cras that are just observing rate limits， not doing injections， not trying to attack。

 but just trying to gather data in a reasonable way， but using data centers to do it。

There's so much of that volume of traffic on the Internet now that it can overwhelm small servers and small organizations。

 So if you're big organization， it's not a problem because you have lots of I Ps and a technical staff to deal with this。

 But if you're a small organization， you have just a few servers。

 then you need a way to just deal with that volume of traffic， regardless of where it's coming from。

So the policy is of the Community Science Institute。

 I want to get to policy a little bit because it does relate very much to this。

 The water quality data is available for free to the public， at community Science Institute。

 the client that I work with。We they say that we prefer to have a human in the loop and discourage AI crawlers so that our servers remain responsive to human users。

 So it's not about data protection。 Like this organization that we work with is not trying to block the traffic because they're trying to protect their data。

 Their data is free and even provide like a free downloads page where you can literally download in bulk the entirety of the data。

 If you're a human and you did that， and you would just get the data right away。 But instead。

 these AI crawlers are trying to hit every single page and trying to grab all these different pages when really。

 if it was just one human that contacted us and said we'd like your data。

 you would just get it right away for free， right， So the purpose is to actually keep our servers responsive to human users and to allow the human users to really use the servers well and just discourage that kind of like。

Blind grabbing of data by AI crawlers and data centers。

Another reason for looking at this from a policy perspective for small organizations is that grants for nonprofits and small orgs are often depend on the viewership statistics。

 statistics for newer renewed funding， right for small organizations。

ll often ask like what are your statistics for viewership。

 And if you were to say those those original numbers。

 you would say you're getting 12000 hits per day。 That's a nice number。

 but it's not accurate in any way。 because 90% of that is due to AI bots。 So we can with this tool。

 we can get an upper bound on human use。 And I think there's still probably a lot of AI traffic in there as well。

 right， But at least gives a more realistic upper bound。

 a more realistic upper bound of around 500 pages per day，500 hits per day for a single server。

 So that's a 95% reduction in traffic， which is very different than just basic throttling。😊。

So the conclusions are understanding the extent of AI and crawler bot activity。

Defending small organizations。 So we want to understand the extent of that activity and try to understand like。

 what are AI crawlers and bots and how do they behave that is human， you know， versus machine。

 And the goal is to defend small organizations。 That is single machines from large organizations that have many machines and data centers。

 And that prevents presents an interesting challenge because。

You want to be able to find the tools cheaply and easily， easy to use。 And also， you know。

 you don't have a huge I T group that's going to solve these things。 So how do you solve it。

 So you want to be able to able to specify defense policy。 How or how strongly do you want to defend。

 No the extent possible the implications of those policies。 And do all of this easily。

 cheaply and open source。 And so that's kind of where we focused our efforts on。 I。

 this is being done by Q sciences， which is a knowledge AI and data visualization startup。

We developed loggriip as a tool to do this， and so I'll go into a little bit what that tool does。

 It's a simple lightweight open source tool for generating block lists and policy visualizations based on access logs。

So that new tool is log grip。 And here's an example of it running。

 It's now we just made it available this week on Github。 you can download it。 It's open source tool。

 Apache2 license。 so free to try it out。 and it runs entirely from the command line and add outputs visual products from the command line as well。

 so you don't need a graphical interface。 You can just run it。

 you can batch it if you need to It only takes two input。

 the input is the access log that you're looking at。

 So something generated from like journal control。 and it config file that tells that the log format and the policy you'd like to apply。

 I'll show some examples of this in a minute。 and the outputs are all of these different products that you get output it at once。

 It gives you all of these things at one time。 It's fast， it can do 150 k log in less than a minute。

 so you can handle large logs and put them into the tool and get out all of these different output products that you want to take a look at。

These are all the products。 They' are all the things that I kind of mentioned throughout the talk。

 It outputs the observed traffic as a P G file。 So the graphical visualizations are just done as a straight Png。

 you can just look at the images。 It also outputs a colored one。

 which shows you the blocking actions that were taken。

 it was able to filter And then it shows you the filtered traffic which is a result of that on the bottom left。

 it shows you the difference between the original and filtered。

 so you can very easily as a human look at it and say here's the traffic that we started with。

 Here's the traffic that we ended up with if we applied this filter。

 It outputs the metrics by I as a CV file。 So you can load that into your favorite spreadsheet and then just look at what's happening with individual Is。

 and basically the statisticstik for that。 And the metrics by v sububnet and the page hits as the kind of total data。

 What pages were being hit。 There's a couple more products like courseet outputs the blacklist as well。

 And you can use that blacklist and drop it into to I tables or some other filter that you'd like to use to。

😊，Do the actual real time blocking。So limitations of the tool， I just want to mention these briefly。

 some limitations， you probably guessed if you're a technical person that that vertical thing that spreads across many Is is a distributed dental service attack。

And that is more a proper hacking approach because basically what they're doing is grabbing random Is across the entire vertical spectrum of Is and applying them all at a single short duration time。

 And so this is what a DD attack looks like in this visualization。

 It looks like this very short thing happening across the entire spectrum。

 And you can't really approach it from I blocking because they're randomized each time。

 So the next time they might be on a different a different range across the spectrum。

 So this tool is not really meant to service that。 So if you want to do blocking a DD attacks。

 you kind use a different tool because this is blocklist。 So that's one limitation of the tool。

 another limitation is that many Acros are still present。

 but they're well describedised and more random。 So if we look at this thing on the right。

 it's random and infrequent， it's maybe across many Is because you can see they're kind of slightly distributed there。

 It is random and infrequent。 And it's below the limits of the policy that was set。

 That's probably still a machine because you can see how regular it is。 And it。

Happens somewhat late in the evening。 You can adjust your policy to maybe grab that。

 But at this point， it's starting to look very much like a human in a way， right。

 It's behaving more like a human in the sense that it's not that often。

 It's maybe two or five times a day，10 times a day， maybe。 and it's very slow and somewhat random。

We would basically say， I mean， looking at this， I would say， well。

 maybe you don't block that because it's acting more like a human。 It's slow。 It's methodical。

 It's taking its time。 So maybe that's okay。 Maybe it's okay to let that machine have that data。

 It's acting not in a way that's aggressively bogging down the servers。

 So you can see that there's probably still a lot of traffic like that。

 It's acting less aggressively。 That's you know， grabbing data more like a human would。

 And that's maybe desirable or what we're trying to achieve。

 So at some point you have to say it's close enough。

And the human and machine gets kind of harder to distinguish at that point， right。

 Whoever designed that server， Whoever designed that policy for that AI crawler did a good job。

 because they said， let's just make it literally look a lot like a human。

 So it's really interesting approach from the attack perspective of like。

 how do we design it so that it really behaves like its machine。 I mean， like a human。Future goals。

So briefly mention some future goals。 It's now in use。 It' it's now in use by the CSI。

 So we're using it to actively block。 and just very recently。

 So in the past month we started using it。 So one of our goals is to measure the post- blocklocking activity with the client and understand the realtime implications。

 So we spend a lot of time doing the log analysis for several different month periods 20 days at a time。

 and developing block lists and now those lists are in place and we want to understand how it's actually affecting the server and what's happening with the usage of it。

 another thing that's really interesting and challenging is the difference between ground truth data for humans and non-human activity both are very difficult to replicate。

 If you think about replicating a human， you have to really kind of act like a human and be really kind of random and it depends on sleep and time of the day depends a lot of human factors。

 circadian rhythms。 So that can be difficult to replicate and also people are very specific about what they're grabbing right。

They're grabbing certain pages to try to understand something， to gain knowledge。

That's hard to replicate。 The other thing that's hard to replicate is the nonhuman activity。

 And that's the machine that is basically， you know。

 there's many different subtle ways to approach that。 The very obvious one is， you know。

 just rapid access to everything。 But there's a spectrum in there where there's a boundary， right。

 And so getting the two different data sets is very challenging because there's overlap rate in the middle。

And it would be nice。 So we did not have ground truth data with this because this was a live server。

 And so we collected data from this and did this analysis visually。

 but it would be really nice to have ground truth data for this going forward in the future and doing kind developing honeypots or some way to kind of gather that ground truth data。

 the other thing is to study policy parameter sensitivity and or optimize for that。

 So now has like 12 parameters that you can adjust。 And that gives you quite a big parameters range。

 And so I've experimented a little bit with it。 like if you adjust the parameters up and down how how much more strongly does it block certain patterns。

 and it would be nice to optimize for that。 But that optimize depends on there being ground truth data。

 So being able to optimize for parameters for that。

 say you want to kind of take a machine learning approach。

 you really need some ground truth data to be able to say， yes。

 that's a correct block or not correct block in order to optimize the policy parameters。

 So there's interesting future goals。😊，I want to show a very quick video of how easy this is to set up。

 So， so for some of you the are technical in the audience， I have a video of setting this up。

 It's only two minutes。 So you can do full setup and running of this in two minutes。

 That includes building it。 So I'm gonna show you building it， too。



![](img/5e84d347fb22878174f9c0082cc6c31a_8.png)

![](img/5e84d347fb22878174f9c0082cc6c31a_9.png)

So here it's cloning the repository。 There's two dependencies， Libmin。

 both are on our Github repository called Qantasai。Liibmin and loggri are both the two dependencies。

 So we've downloaded them， cloned them， doing build on Libmin。takes just a few seconds。

 It's a pretty small kind of lightweight project。We get to watch a bill during a talk。

Now we're going to build log grip。So， that's done。Now， we're going to need some inputs， right。

 So we look at assets， which comes from a repository。 There's an example log。

 and we'll take a look at an example log。So this example log is a ruby on rails log actually。

 and it's basically your journal control rubbyion railils log for a website for a server。

 statistics the hits are all there for get queries with all the traffic data。

 your typical log and we also need a configure policy， the config files， the other input。

 So there's examples here for rubyion railils and for Apache2 and you can see that the first thing in the log is the format string。

 which is a dynamic parser， we build a dynamic parser that basically does this regular expression parsing。

 So you can put in kind of whatever log format you want。

 there's capture groups that get the different information from the log about the statistics you're looking for like the page。

 the I number， the date and time is really all our tool and needs because we're not analyzing the request in any more depth than that and then you can see there's also the policy settings are also in config file So the policy settings for the daily limits。

 the daily range。Concutive range， like what how much？What is a consecutive range that's in minutes。

 So 2，40 is like 4 hours， roughly， I think。 So it's telling it the policy limits based on the discussion earlier。

Other visualization things like how do you want to do the visualization？

And then we continue from there。So now we run it。 we're going to find make sure that it was built。

 So log grip is there。Now， we just run log grip， and we provided those two inputs。 We input the log。

 and we input the config file。And now it's actually running。 It gives you some good feedback。

 It tells you that it's loaded the settings from your config file。 It's got your config file。 Ok。

 It's now reading the logs。H00% red， and then it does all that stuff very quickly。

 This is a very small example。 So it ran extremely fast。Writing processing Is。

 building that hierarchical patch that I talked about。

 constructing the B subnets and the C subnets from your log data。

 writing out the Is for those different subnets， writing out the pages and the hits and giving all those output products that I talk about。

It shows you kind of a total summary there that you basically on each day。

 it gives you a summary and the reduction for that day based on the policy that you've set。And now。

 if we take a look at the output products and output all these out files and figure1 P And G is your kind of like pre filtered。

 Here's the traffic that you have。

![](img/5e84d347fb22878174f9c0082cc6c31a_11.png)

The blocking activity was the colored one。 And then this is the post filtered results。

 So these PGs are output by the tool。It also outputs the load visualization。



![](img/5e84d347fb22878174f9c0082cc6c31a_13.png)

And we'll just take a look at a couple of our outputs， the Is。

 This is a CSV file you can load to in spreadsheet that shows you all the Is and the statistics for each of them。

 So there's a header at the top of this that tells you like the frequency。

 the durations and the page limits for each of the Is and an example of one of the pages that it tried to hit。

And finally， the last thing is。The block list， which is what you want。

 this is the block list that you generated， it's hierarchical。

 so it's as short as possible right it does from the top down and says let's block the B nets and the C nets and then if anything that was on a specific IP is included in those。

 it avoids including that because it's already covered by the higher level so it's as minimal as possible block list。



![](img/5e84d347fb22878174f9c0082cc6c31a_15.png)

U。

![](img/5e84d347fb22878174f9c0082cc6c31a_17.png)

That's it for that。Go back to the slides。So I think I have seven minutes left。

 I think we'll use that for questions。

![](img/5e84d347fb22878174f9c0082cc6c31a_19.png)

This is how you can get and talk to me or find out more information Quaantas sciences is now on GiHub at Qantasai loggriip is there as well so you can download that and give it a try。

 there's just been an archive paper this week that we posted that talks about this and a lot more technical detail and gives you kind of a more historical and kind of review of prior work in relation to what we're doing and then if you want to find out more about me。

 you can find me by my website and I also hang out a lot on LinkedIn so you can find me there too okay。

All right， so thank you very much。And we'll do questions。

Realizing the SMBs and the variety of different informations， website， sales。

 informational utility that are out there that could typically be geofd especially for certain times a day who's going to be looking at their water data at three in the morning looking at the regularity of that would it be possible to include geofencing compared to time of day Yeah which would be another unique identifier or somebody from some weird IP address accessing consistently go Yeah。

 I think it would I think it makes sense to include geoI So one of the things that you know in our future goals is to be able to do it a little bit more fine- grained and so to add more like looking at Is and doing geofencing and having a database that can say oh these Is are from a certain area or region and blocking by that So yeah。

 definitely we'd want to be doing that the geofencing information is already openly available and yes。

Yes， yeah，' that's one thing we'd like to do。 The other thing we'd like to do， too。

 is to start looking at the request in a little bit more detail because I mentioned our goal was to like do this purely at a statistical perspective for now。

 but I think it would be nice to also start investigating the request a little bit more detail。

 And just using that information to say this is definitely a machine or non-machine that gets you into the whole question of you is this an attacker or not。

 but this was a good starting point for us， but we realized that， yes。

 we can also go in and get more information from lots of different sources like such as publicly available lists。

 not so much block list， but things like geofencing are a good example。Okay， any other questions？

Okay， thank you very much for coming。And。So how frequently do you see yourself happen to do a scan like this for a particular small business in order to set up or refresh the block list？

That's a good question。 So this， we're still very early in doing this。

 So it's not really known yet how often we need to do it。 But what's nice about doing it。

 if if you set it up well with your I tables， you can actually continue to grab traffic that would be incoming even。

Like before it's blocked， right， So you still do the blocking， but continue to grab the I traffic。

 And we'd like to keep collecting it continuously and then see how often we need it for refresh。

 I don't have the answer yet to that yet because we've been doing this pretty， pretty recently。

 But once we get enough， then we'll have a better sense of like。

 how often we need to refresh the block list yeah。😊，Okay。You。

