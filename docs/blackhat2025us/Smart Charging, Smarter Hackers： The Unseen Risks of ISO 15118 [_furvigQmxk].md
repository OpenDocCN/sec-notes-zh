# Smart Charging, Smarter Hackers： The Unseen Risks of ISO 15118 [_furvigQmxk]

So， guys， we are well in to day， too， and I know that the post lunch slots are not the easiest to see through。

 So， 1s of all， thanks for being here。 And I appreciate even considering how many great talks are happening all around us。

 So I promise I'll make it worth your time。I'm Salvare Garrivollo。

 and I'm a senior threat researcher at Tread Micro， and I'm part of a very peculiar team。

 that is call FTR forward looking threat research。 So our job is to look a little ahead。

 So we try to understand how emerging technologies。

 standards and systems are going to reshape the threat landscape in the next few years。

 so we don't simply wait for threats to emerge。 We try to anticipate them。

 We try to get there before a malicious user。 And this brings me to today's topic。 Today。

 we are going to discuss about how the ISO 1，5，1，1。

8 standard is going to change the threat landscape。 Now， when we hear the word standard。

 we feel a natural sense of relief。 because standard means structure Standard means that someone thought things true。

 And this is always the case， even with ISo 1，5，1，1，8。



![](img/8426102bc42fc528b0c095a15867365e_1.png)

![](img/8426102bc42fc528b0c095a15867365e_2.png)

![](img/8426102bc42fc528b0c095a15867365e_3.png)

![](img/8426102bc42fc528b0c095a15867365e_4.png)

![](img/8426102bc42fc528b0c095a15867365e_5.png)

![](img/8426102bc42fc528b0c095a15867365e_6.png)

![](img/8426102bc42fc528b0c095a15867365e_7.png)

![](img/8426102bc42fc528b0c095a15867365e_8.png)

![](img/8426102bc42fc528b0c095a15867365e_9.png)

![](img/8426102bc42fc528b0c095a15867365e_10.png)

![](img/8426102bc42fc528b0c095a15867365e_11.png)

But there's a catch。

![](img/8426102bc42fc528b0c095a15867365e_13.png)

Because a standard can also create a false sense of security。 You might feel secure。

 even if you you are not。 And this happens when a standard changes the playing field in ways that we haven't fully mapped out yet。



![](img/8426102bc42fc528b0c095a15867365e_15.png)

![](img/8426102bc42fc528b0c095a15867365e_16.png)

![](img/8426102bc42fc528b0c095a15867365e_17.png)

So today were not only going to understand how the standard addresses some of the most significant risks in the E recharging ecosystem。

 but we are also going to understand what are the risks that the standard leaves behind and what are the risks that the standard introduces as unintended consequences of its innovations。



![](img/8426102bc42fc528b0c095a15867365e_19.png)

![](img/8426102bc42fc528b0c095a15867365e_20.png)

![](img/8426102bc42fc528b0c095a15867365e_21.png)

![](img/8426102bc42fc528b0c095a15867365e_22.png)

![](img/8426102bc42fc528b0c095a15867365e_23.png)

I split I the presentation in three main parts， so the first part is pretty straightforward。

 we are going to understand what is the ISO 15118 standard and why it was introduced。

In the second part of the presentation instead， we are going to understand how the standard changes the threat landscape。

 as we said。The third and last part of the presentation instead is simply to wrap up the findings of my research。

 so I'm going to leave you with a Kim a main message a miss a main conclusion and three key takeaways that I would like you to leave this room with。

So let's start immediately with what is IO 15118， why the standard was introduced and what type of problems the standard aims to solve。



![](img/8426102bc42fc528b0c095a15867365e_25.png)

![](img/8426102bc42fc528b0c095a15867365e_26.png)

![](img/8426102bc42fc528b0c095a15867365e_27.png)

You can see from the slide， but this is something that you can realize in our own reality there is a steep increase in the number of electric vehicles and some estimates suggest that we might have up to 600 million of electric vehicles by 2040。

 so the electric vehicles by then will make up 30 to 40% of the global vehicle feet。

And this is a fantastic news for decarbonization。But at the same time。

 the steep increase in the number of electric vehicles is also challenge for us。

And this is because our electricity grid， our power grid。

 is not designed to accommodate a large number of electric vehicles charging at the same time in the same location。

When this happens， in fact， the grid faces as a challenging situation that is called grid strain。

 so we might start experiencing voltage drops。The local power transformer might start getting damaged。

 and this might even lead to total disconnection of the local grid， so a total blackout。

But we also have a second problem that we need to understand when we talk about the grid。

And this is not a problem that is caused by the electric vehicles。

 Its a consequence of the way we produce and we consume electricity。Today。

 we are relying even more on renewable energies like solar and wind。 And don't get me wrong。

 This is a fantastic news。 But at the same time， this is also a challenge。😊。

Because our grids work on a fragile balance between the amount of electricity we produce and the amount of electricity we consume。

And there is a risk tied with the renewable energies because the amount of electricity that gets produced with wind and solar can quickly change with the weather。

 So this means the renewable energies can create a surplus of electricity in the grid。

 and we have to find a way to use it or store it。If we are not able to do so。

We might face serious consequences because this situation creates a condition that shifts the frequency of the grid。

 the so called heartbeat of the grid， outside safe operational limits。And when this happens happened。

 something like Spain 2025。 So Spain on certain days。

 is able to produce almost 50% of the electricity through renewables。 Again。

 fantastic news by April 2025， also because a human miscalculation about the amount of electricity that was going to be produced through electricity and wind。

 we had a massive surplus of electricity and the grid。

 And despite the fact that we have safeguards in place to mitigate these situations。

 These safeguards were not able to act quick enough。😊。

And what was the consequence that the entire grid， the national grid in Spain was disconnected。

 And this blackout lasted for almost 48 hours。 You can imagine the consequences of this。

 And we've seen the the effects also in specific location in the south of France and in the country of Portugal because Portugal heavily relies on the electricity imported from Spain。

So we said that we have two problems， one is caused by the electric vehicles and a second problem there up byproduct of the way we deal with electricity on the grid。

But what if I told you that the electric vehicles can be the solution for both of these problems？

Because we estimate today that the vehicles' private vehicles in general are parked on the road 95% of the time。

So we could actually use electric vehicles parked on the road as a decentralized storage battery。

And this would avoided the situation that happened in Spain。

 So what are the technologies behind this， We have two main technologies。

 smart charging and vehicle to grid communication Smart charging is an innovative way of charging allows the charging station to change the pattern the charging pattern so the charging station will make sure that an electric vehicle will pull electricity from the grid only when the grid allows it so we will be able to solve the first problem vehicle to grid communication solves the second problem so the car part on our street might be able to pull electricity from the grid when there is excess of electricity。

And these vehicles might be able to push it back to the grid in case the grid needs it in future。

 in the future。So you can see where I'm going with this conversation the IO 15118 standard was actually introduced to make the grid more efficient because the standard supports these two technologies smart charging and vehicle to grid communication So this is our starting point today as you can see making the grid more efficient is only one of the main objective of the standard。

 because the standard at the same time is also used to make the process of charging an electric vehicle more convenient for the user and this is done through a new functionality。

 this whole plugin charge and we will go back to this later。And at the same time。

 as you can see on the slide， the standard has also been introduced to improve the security of the EV charging ecosystem。

 and this is normal otherwise we wouldn't be here discussing about the standard in Blackout 2025。

Small details these might be useful later。 We have two different versions of the standard。

 a first version of the standard1，5，1，182 that was introduced in 2014。

 which supports all the functionalities that you see on the screen。

 but one vehicle to grid communication， which is fully supported only by the second version of the standard。

151，18 20 that was introduced in 2022。So we now know the foundation， what is the ISO 15118 standard。

 how does the standard change the threat landscape？As we said。

 the standard actually improves the security of the Vcharging ecosystem。

 so the natural point for us to do this analysis is actually this。

 what are the threats that the IO 15118 standard mitigates？

So how does the standard improves the security of the EV charging ecosystem。

 he focuses on the communication link between the electric vehicle and the charging station。

And it protects the communication to a new functionality that we just introduced plug charge。

 so today if we have an electric vehicle that supports the ISO standard。

It is the vehicle that stores a digital certificate。This digital certificate is issued through a PKI。

 a public infrastructure and is tied to the user's account。

 so this means that when we plug our electric vehicle into a charging station that supports the standard。

It is the charging station that identifies automatically the electric vehicle。

 so if you own an electric vehicle you don't have to use your credit card or your physical token anymore to authenticate the charging session this will be done automatically through plugin charge。

And plugin charge also allows the charging station to extract from the digital certificates all the information that that is needed for payment and billing。

 so again， we don't have to use our credit card。And all the information that is flowing between the electric vehicle and the charging station is encrypted through TLS。

So what does this mean in terms of the threats that we have around us？

I tried to summarize this evolution with this table and as you can see。

 one of the most significant threats that once plagued the image chargingging ecosystem was the unauthorized charging session and before the introduction of the standard this was particularly easy to pull off because we said before charging stations were heavily relying on physical tokens like our FiID cards and credit cards so components that could be stolen skimmed or clone today this is not possible anymore because all the information comes from the digital certificate。



![](img/8426102bc42fc528b0c095a15867365e_29.png)

![](img/8426102bc42fc528b0c095a15867365e_30.png)

![](img/8426102bc42fc528b0c095a15867365e_31.png)

![](img/8426102bc42fc528b0c095a15867365e_32.png)

![](img/8426102bc42fc528b0c095a15867365e_33.png)

![](img/8426102bc42fc528b0c095a15867365e_34.png)

So as long as the encryption holds on the communication link between the EV and the charging station for the malicious user there are very limited option。

 they should tamper with the physical hardware of the electric vehicle to extract the private key that is linked to the digital certificate or they should compromise with the certificate authority itself to start issuing fraudulent certificate and as you can imagine this is no easy at all to pull off。



![](img/8426102bc42fc528b0c095a15867365e_36.png)

![](img/8426102bc42fc528b0c095a15867365e_37.png)

![](img/8426102bc42fc528b0c095a15867365e_38.png)

![](img/8426102bc42fc528b0c095a15867365e_39.png)

![](img/8426102bc42fc528b0c095a15867365e_40.png)

![](img/8426102bc42fc528b0c095a15867365e_41.png)

But the standard also improves the security of the EV charging ecosystem in a more indirect way。

Because as we said， all the payment information come from the digital certificate。

 So this means that today with the ISO standard in place。

 it is not the charging station in anymore that is responsible for managing payment。

But we move this responsibility to a new centralized entity in the back end that is called EMSP。

 E mobility service provider， so what does this mean in terms of the threat landscape？

This is particularly significant because before the introduction of the standard we were leaving the responsibility for managing data to the charging stations。

 and this led to a very fragmented ecosystem， Some of the charging stations were actually using robust security measures。

 some of them were not。Today we are moving this responsibility to a centralized entity， the EMSP。

 that can enforce cyberseity policies at scale。It is true， though。

 that this shift in responsibility comes with a trade off。



![](img/8426102bc42fc528b0c095a15867365e_43.png)

Because before the introduction of the standard leak on the charging station level would have exposed hundreds or thousands of electric vehicles on Earth data today a bridge on the EMS emobility service provider level will probably expose million of data at the same time。

 but anyway， despite this tradeoff the improvement in security is still significant because we are moving from a fragmented ecosystem to a more robust and cohesive architecture。



![](img/8426102bc42fc528b0c095a15867365e_45.png)

![](img/8426102bc42fc528b0c095a15867365e_46.png)

![](img/8426102bc42fc528b0c095a15867365e_47.png)

![](img/8426102bc42fc528b0c095a15867365e_48.png)

![](img/8426102bc42fc528b0c095a15867365e_49.png)

![](img/8426102bc42fc528b0c095a15867365e_50.png)

Now we could stop the presentation here and if we stop， we will stop the presentation here。

 you will walk out of this room with a sense of relief。



![](img/8426102bc42fc528b0c095a15867365e_52.png)

![](img/8426102bc42fc528b0c095a15867365e_53.png)

Because if we only focus on the things that the standard does， the improvement are significant。

 So this means that we walk out believing that the EV charging ecosystem is actually secure because of the standard。

 But this is not the case。 And that's why we have to zoom out and also try to understand what happens in the rest of the ecosystem also because of the standard。



![](img/8426102bc42fc528b0c095a15867365e_55.png)

![](img/8426102bc42fc528b0c095a15867365e_56.png)

![](img/8426102bc42fc528b0c095a15867365e_57.png)

![](img/8426102bc42fc528b0c095a15867365e_58.png)

![](img/8426102bc42fc528b0c095a15867365e_59.png)

And the first bit that we are going to look at is the charging station。

 because the charging station is one of the components is is left out of the scope of the standard。

 and here we have to be careful because this is not a flow into the design of the standard。

 this is a deliberate choice because the standard only focuses on the communication link between the vehicle and the charging station。

But the standard makes an assumption， and this is a very dangerous assumption。

 The standard assumes that the charging station is a trust identity。

 that the charging station is secure。 and this is up an assumption that can backfire。In fact。

 some of the recent audit demonstrated that charging stations。

 some of the charging station still rely on of the shelf components like Raspberry Pi。

 they still have open maintenance interface， open the back partss， un secureure remote path access。

 so this means that the charging station is still very vulnerable to persistent compromise。

And we don't have to forget that the charging station is publicly accessible。

 so this means that malicious user could go there and physically tamper with it。

 it could upload a modified version of the operating system。

It could escalate privileges to gain full route access。

 and through this he will be able to pull off various attack。You can see this light。

Red means bad that's normal。 So we would have missed all of this if we would have stopped the presentation before。

 So what does this mean。If a malicious user takes control of a charging station。

 it can easily post full of a denial of service attack because if we have control of the charging session。

 we can simply disable functionalities。And you can start realizing the first contradiction of the standard。

 The standard protects the communication link between the electrical and the charging station。

But at the same time， the charging station might not be usable at all。

Now we can move to the second threat and save power delivery。 So to make things worse。

 we don't have to forget that charging stations are cyber physical system。

 So this means that the ana on the charging station will have consequences on the on the real life or on our own reality。

And this is particularly dangerous now if there is someone that knows a bit more the details about the ISO standard。

 might start arguing with me。Because the IO 15118 standard actually has a negotiation mechanism。

 So the electric vehicle and the charging station have to negotiate together an acceptable power level so a power level they say for the electric vehicle but you know what the standard is lacking is lacking a mechanism to enforce compliance。

 So this means that after this negotiation if the charging station is compromise。

 the charging session could simply overwride the settings and it could start delivering a power level is dangerous for the electric vehicle it can create safety hazard for the user。

And there is a third thread you may recognize it is the threat that we discussed in the first slide in the first table onauor charging session。

 so we believe that this type of threat was completely mitigated through the introduction of the standard and this is not the case so were introducing plug-in charge digital certificates but the standard leaves a loophole because the standard lacks a mechanism for charging station to synchronize their clock with a trusted time source So what does this mean this mean a malicious user could simply take control of the charging station modified the local clock and this will trick the charging station into accepting revoked or expired certificates So something that we believe it was solve is actually there if we look at the bigger picture。

And there is now a fourth and last category of risk that we have to analyze the new risks。

 so this means that these risks were not there before of the introduction of the standard。

And these risks are unintended consequences of the standard innovation。

 smart charging MBA cultivate communication。Why do we talk about these functionalities as attack vectors。

 is because again the charging station is left vulnerable to compromise。

So let's take into account the first risk charging manipulation charging manipulation means that a malicious user will try to charge his vehicle at someone else's expense。



![](img/8426102bc42fc528b0c095a15867365e_61.png)

![](img/8426102bc42fc528b0c095a15867365e_62.png)

![](img/8426102bc42fc528b0c095a15867365e_63.png)

And this becomes possible with the introduction introduction of smart charging。

 because smart charging， as we said， is an innovative way of charging that allows the electric vehicle。



![](img/8426102bc42fc528b0c095a15867365e_65.png)

![](img/8426102bc42fc528b0c095a15867365e_66.png)

To stop the charging if the grid is in a situation of strain。



![](img/8426102bc42fc528b0c095a15867365e_68.png)

So if we have a compromised charging station， a malicious user could simply simulate congestion on the grid。

 this will inflate artificially the prices at the charging station and it will force a lot of electric vehicles to stop charging so you can imagine the frustration of the owner of an electric vehicle that goes back to its vehicle and finds that the battery is completely uncharged。



![](img/8426102bc42fc528b0c095a15867365e_70.png)

![](img/8426102bc42fc528b0c095a15867365e_71.png)

![](img/8426102bc42fc528b0c095a15867365e_72.png)

![](img/8426102bc42fc528b0c095a15867365e_73.png)

![](img/8426102bc42fc528b0c095a15867365e_74.png)

But there are also way more complex attacks that can be pursued by a malicious actor and one of this for example。

 is the grid attack and this is an attack that is enabled by a vehicle to grid communication So we said that vehicle to grid communication allows the charging station。



![](img/8426102bc42fc528b0c095a15867365e_76.png)

![](img/8426102bc42fc528b0c095a15867365e_77.png)

![](img/8426102bc42fc528b0c095a15867365e_78.png)

Actually allows the electric vehicle to pull and to push electricity in the grid。



![](img/8426102bc42fc528b0c095a15867365e_80.png)

So let's imagine a malicious user that takes control of multiple charging stations at the same time。

 obviously this is particularly difficult to pull off but this doesn't write this type of attacks off is simply narrows the type of attackers to people that have the budget。

 the skills and the means to act so if we have multiple compromised charging stations and they are all connected to the same local power transformer。

 this charging session can be synchronized to to continuous charging and discharging。



![](img/8426102bc42fc528b0c095a15867365e_82.png)

![](img/8426102bc42fc528b0c095a15867365e_83.png)

![](img/8426102bc42fc528b0c095a15867365e_84.png)

![](img/8426102bc42fc528b0c095a15867365e_85.png)

![](img/8426102bc42fc528b0c095a15867365e_86.png)

![](img/8426102bc42fc528b0c095a15867365e_87.png)

![](img/8426102bc42fc528b0c095a15867365e_88.png)

And this is particularly dangerous for our own reality because this type of attack has the potential to shift the frequency of the grid out of safe operational limit。

 so we will be able to artificially induce a blackout day similar to what happened in Spain in earlier this year。

 so we will be able obviously with skill and budget to act to disconnect part or the entire national grid。



![](img/8426102bc42fc528b0c095a15867365e_90.png)

![](img/8426102bc42fc528b0c095a15867365e_91.png)

![](img/8426102bc42fc528b0c095a15867365e_92.png)

![](img/8426102bc42fc528b0c095a15867365e_93.png)

![](img/8426102bc42fc528b0c095a15867365e_94.png)

So what is the main message I want to leave you with today day。So first of all。

 we have to be careful。 We have to。Understand that the ISO 15118 standard is actually a standard that significantly improves the security of the EV charging ecosystem。

 this cannot be denied。

![](img/8426102bc42fc528b0c095a15867365e_96.png)

But if we zoom out， we also realize that the standard leaves some of the risks behind and some of these risks are particularly dangerous。

 and the standard may also introduce new risks as unintended consequences of the standard innovation。



![](img/8426102bc42fc528b0c095a15867365e_98.png)

![](img/8426102bc42fc528b0c095a15867365e_99.png)

说。When we have a system or when we have an ecosystem like Dvy charging ecosystem that is label compliant with a standard。

 we may feel a sense of security， but as we said， this might be a false sense of security。

 you may feel secure， even if the ecosystem is not。

So we have to realize that true security is not simply complying with a standard。We。

 through security requires a shift in mindset。So instead of asking ourselves。

 what does the standard address， so this would have meant stopping at the first two tables that we discussed today。

We should ask ourselves what does the standard change， so we have to zoom out。

 We have to understand what is happening in the entire ecosystem because of the standard。

 and we should also ask ourselves what does the standard leaves behind。

 because often new risks of vulnerability emerge in the gaps that no one is looking at。

So the three key messages I want you to leave this room with are the following， first of all。

 and this is quite straightforward。Standard can improve security， but they can also create loopholes。

 can also create security blind spot。But probably it's even more important to take into consideration the second key message。

Because as we've seen today， usually an entire ecosystem is based on a web of different standards。

And the IO 15118 standard is only one piece of the ecosystem we are talking about。

 but today we realize that when even one component of the ecosystem is left what is left out one of the standards standards。

 the security of the entire ecosystem is at risk and this is。

Despite of how well secure the individual components might be。

So this leads me directly to the third key takeaway。We need action beyond compliance。

 so it is good to have standards in place because these standards gives us guidelines to proceed。

 but we also have to understand that usually a standard is not enough so my advice would be for all the stakeholders working in the EV charging ecosystem to come together like EV OEs charging station manufacturer。

 immobility service provider， grid operators they all have to come together and they have to understand that we need to implement security measures that go beyond the security measures provided by the standard。

And this is the only way we can guarantee through security。

 and this is the only way we can avoid the full sense of security that comes with simply complying with a standard。

Thanks a lot for your time and for your attention。 And I hope you enjoyed the talk so we can actually。

Open the floor for questions。