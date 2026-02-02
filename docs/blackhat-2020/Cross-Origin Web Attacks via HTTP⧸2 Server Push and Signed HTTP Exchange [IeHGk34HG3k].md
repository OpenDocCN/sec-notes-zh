# Cross-Origin Web Attacks via HTTP⧸2 Server Push and Signed HTTP Exchange [IeHGk34HG3k]

So hello， everyone。 I'm Ping Jichen from Xinhua University。 Today。

 I'm very delighted to present our research across origin Webt will HB2 server push and sign H P exchange。

 This work is also contributed by Jianjun， Qi Ming Min and Haixin。😊，So today。

 our talker map will cover three aspects， and each aspect will address a quick question。

In the first question， we will introduce what is the same origin policy and what has been changed in today's origin definition。

Second， we will introduce what novel threats or a text would this change bring to the web。

 And also in this detection， we will present our cross push and cross SSG attack。Alas。

 we will address the real world practicality of these attacks。

 highlighting some practical attack techniques and also introduce the real world case that we found。

😊，Okay， let's start with the same policy。I believe that all of the audience here are familiar with the same origin policy。

 We all know that the same origin policy is a cornerstone of Web security。

 which is designed to safeguarding user data against cross origin text。

But what I want to emphasize that here is that the origin region in same in same policy is U I based。

 which means that two resources are considered to have the same origin only if their scheme host and port U I Ta are the same。

However， we found that this is not the only definition of our region。

So do you know other definition of our region。One day。

 when I read some of these specifications to learn A B 2。

 I discovered that recent investment in the H T TP protocol undermined the same origin policy。

 broadening the origin to I CN based。😊，Specifically， H T P 2 and H T P 3。

 consider any hosts listed in the subject alternative name of the certificate are same region。

For example， this is the World car certificate of Google dot com。

 Its shared with many hosts like Android dot com， YouTube dot com。

 and also some domains that we are not familiar with。😊，But in A TP 2 and A TP 3。

 all of them are considered as the same region。So this motivates me to further investigate the situation of multi domain certificates。

Actually， such multi domain shared certificate is a common practice。 Pra research publish as the C。

 S conference has shown that 96% of certificates contain multiple domains in their essence list。😊。

And notably，3。2% include domains belonging to different organizations。

This implies that I N based origin may contain different entities。

 So I C M based origin is more permissive than U I based origin。As a result。

 it's quite natural to ask， what novel threat would this more permissive origin bring to the web。

 Actually， this is our work， the cross push and cross S G attack。😊，Well， just like their name。

 this cross origin Web attacks are based on two features。 H T B to server push and sign exchange。😊。

These two features are two server delivery mechanism designed to improve by performance。 Actually。

 you don't need to know their details or you can just learn it later。 But in this talk。

 you need to remember their characteristics。😊，First， in their specification。

 they both comply with the I CM based origin。Second， server push and SSG can indicate。

 or in another word，poof other regions in the shared certificate。 For instance。

 we are the authority personal heater and request URL signature heater。😊，So we summarize。

 combine these two characteristics。 Aers can push or provide access to other origins listed in the I CN list。

Even if those origins are held by other organizations。And based on these findings。

 we propose two novel attack vectors， cross push and cross S G。

An overview of this tax is illustrated in the fear。At first。

 we assume that the attacker can acquire a certificate that is shared between the victim's website and the attacker's website。

 just like the figure show。Keep in mind that share certificate are the foundation of all of our attack。

 We can only launch the attack if we get a certificate that includes the victim's domain in its essay。

 because that's what allow us to take advantage of the relaxed S based origin。😊。

So at the start of the attack， the attacker will lure the victim into clicking a malicious link。

When the victim visit the attacker's website， The attacker will use Sarah P or SSG to deliver a malicious script。

 and setss its origin to the victim's website。Due to the relaxation of our region that we previously introduced。

 they are I CN based。 So the browser will accept malicious cross our region resources as same region and execute them when loading the victim' website。

😊，Notably， our attacks introduce new security implications in prior work。

 Aters who obtained a certificate typically could only launch men in the middle attacks。In contrast。

 our attacks enable offpa attackers to launch practical web attacks using a shared certificate。

This consequently leads to k size scripting， cooking manipulation and other exploitations。Next。

 we will introduce some exploitation details of these attacks。

Since the re and SG are both like a H T TP response， which contain H T TP heater and the H T TP body。

 So first， we can leverage H T B body to launch cross S scripting by inserting some malicia script into H T B body。

 just like the figure show。😊，So surprisingly， as we control the whole HC B response。

 this this cross size scripting are universal。 That means whether the target website has vulnerabilities is offline or no longer exist。

😊，Our attacks deal works。 Also， security policies like content security policy cannot prevent such attack because attackers will not set CP heater in the malicious response。

And here we credit daddy and asking for their blog to introduce this exploitation。

 Their explanation and implementation details inspires me a lot in my research to better demonstrate the attack effect。

 we record a demo video。 So less' is。😊，You can see that one website is controlled by attacker。

 Another is the victim website。So when a user request the attacker's website。

 the attacker can push the alert cookie script and cite a reion。

 Then browser execute the malicious script in the context of victim website。Besides H TP body。

 we can also use H T TP heater to launch attacks， such as use said cookie heater to manipulate others cookie。

 In this case， we can set the victim's cookie on the target website to an arbitrary value。

 For example， the attacker's own cookie。 thus causing a sex session fixation attack。😊，InAnother case。

 we can use the strict transport security heater to bypass HS T， S。

 This can be done by setting the HS T S max age to 0， effectively removing the HST S protection。😊。

And this is a demo demonstration of site arbitrary cookie。So first， we set a normal cookie。

 a task cookie。Second， we launch ourware attacks。So you can see the cookie victim website is changed to your hack。

Moreover， we can also leverage H T B body and H T TP heater together to launch malicious file downloads。

 where use content disposition heater to control the H TB body to be treated as an attachment。

 then put malicious binary content into the body。😊，Here' is the demo。So first。

 attacker can push a malicious file to victims website。Then when you request normal website again。

 browser are accept across our resource， and the Maic file is successfully downloaded。😊。

And if the victim trust this file， his computer may be hacked just like that Colly to run this file。

So， you are hacked。Okay， up till now， our attacks seem to be great。 But I know that someone may ask。

 are these attacks practical in the real world。In the next part。

 we will discuss the techniques to make our attack practical。

We will address the following answer questions first。

How to acquire the shared certificate as an attack condition。Second， how to extend attack duration。

And third， how to bypass potential countermeasures such as the victim revoking the shared certificate。

First， acquire a share certificate is the primary pre request for executing our attack。

Though some accidental flows in H T， T，P， DN S and email may help may help attackers to acquire an illegal certificate。

 But our focus is the inherent vulnerabilities in the certificate ecosystem。😊。

We discovered that there are no mechanisms to ensure that the the certificate owner and the domain owner remain aligned。

Therefore， we propose two measures to create attack conditions， domain resing and domain takeover。

In terms of domain reling， attackers can buy numerous domains and issue a multi domain certificate for them and then resell part of the included domains to victims while retaining the shared certificate。

 Then the attacker easily create the attack condition because the attacker obtain the certificate But the domains are sold to others。

In terms of domain takeover， it occurs when domain names point to deprivation V S I P and registered C Inpoint or just expire domain names。

 Since these target are publicly allow available。 An attacker can register this abandoned services。

 We configure their D S records to point to attacker control infrastructure。

And issue the certificates via H T TP mode。Finally。

 the attacker can use this method to obtain a share certificate that includes the hijack domain。😊。

Another critical factor influencing attack practicality is the attack duration。

We all know that traditional domesticover attacks will be invalid after the Don D N S record is removed by the victims。

In contrast， our attack is still valid after the dongle and DN S record is removed until the certificate expires。

 which typically takes 398 days。However， our findings revealed that a user friendly mechanism。

 known as domain validation reuse can potentially extend the attack duration to 796 days。

This mechanism allows a certificate applicant who has previously passed domain ownership validation to skip the revalidation when applying for a new certificate within a specific time frame。

 and this time frame can last up to 398 days。😊，So the attacker can reapply a new certificate on the last day just before certificate expiration。

 extending the attack duration from 398 days to a maximum of 796 days。😊。

So our attack is still valid for more than two years， even after dling DN S record is removed。Well。

 someone may find that certificate is very crucial in our attacks。

 So maybe a potential countermeasure against our attack is to detect an author certificate via city locks and just revoke them。

So do attackers have method to bypass this countermeasures。 The answer is， yes。

 we introduce a strategy that makes an unauthized certificate irrevocable。 As shown in。

 let's encrypt documentation to revokeke a certificate。

 One of the following two conditions must be met。😊。

The issuer should pass domain ownership validation for all domains included in the certificate or possess the private key of the certificate。

However， consider what happens if an attacker issues a share certificate that includes both the attacker's domain and the victim's domain。

 just like the illustration on the left。Unfortunately。

 the victim does not meet aile revocation condition。 Such shared thick are revoccable by victims。

We also conducted an experiment on0 S L to report an illegimate certificate shared with our domains and the official problem reporting platform。

 but receive no reply。😊，To evaluate the impact of our attack in the real world。

 we conducted a large scale measurement on the client side。

 we measure which browser accept server push and SSG on the server side we measure which website allow attackers to acquire a shared certificate。

If both conditions are met， it may enable an attacker to launch over attacks。

Our client side test target includes top used browsers on sit counter。

 different browsers on leading mobiles and celebrity applications on the AP store。 For serverrsside。

 We measure previously discussed resient domains and the dling domains in tranl top 1 million domains。

😊，In addition， we measure the existing third sharing domains in trinangle top 1000。

To evaluate browsers behavior， considering the wide variety of browsers used in our daily lives。

 we cannot manually collect all of them。 So we developed an automatic measurement system called P S checker。

😊，The mechanism of this system is quite simple。We just use a high track high traffic website under our control to direct traffic to a feature measurement server。

 We are a frame。This server then uses server push and SSG to cross our region push script。 Finally。

 by checking whether the script is executed， we can determine whether the browser supports server push and SSG。

😊，After running P S checker for month， measurement results show that v browsers are widespread。

 with the latest version of 11 top used browsers and  five default mobile browsers are vulnerable to at least one of our attacks。

😊，Meanwhile， was surprisingly found that some celebrity applications used built browsers based on the mobile OS S Web view component。

 which inherited the same vulnerabilities as web views。😊。

This significantly extends the attack surface from browsers to third party applications。😊。

If you want to know more about the details， please refer to our W paper and in the S。

 S conference paper。ETo measure the impact on server side website， we first address resient domains。

Which is one of the attacker' potential methods for obtaining a shared vacate we previously discussed。

We use who is history data to identify domains that have been restore to different owners。

For the other method， dling domains， we utilize a state of the art tool hosting checker。

 which is published at the Smetric conference to detect such vulnerable domains。😊，樊正伟。

We consider th sharing domains。Although these domains do not allow attackers to directly obtain a shared certificate。

 they reveal a form of security dependency。 that is。If any of these domains are compromised。

 the top ranked domains sharing the certificate with them may become vulnerable to our attacks。

Our measurement methodology is as follows。 We first scrap all domain names listed in the I of certificate from the top 1000 website to identify their related domains。

 We then extract sub domainomas of this related domains from A TP responses C logs and passive DN S data。

 Finally， we check whether this associated domains share certificate with the top 1000 website1000 website。

😊，evaluationvalu results shows that numerous website are affected。

 Some cases even affect notable website。 For instance， F T static dot com， which ranks 3895。

 was once resold from an Australian food company to an American advertising agency。

And could potentially be exploited by the original domain owner。

A subtleway of Windows update dot com from Microsoft is dangling。

 which can be registered by any attackers。Moreover。

 many top ranked domains share certificates with low ranked domains。

 These low rank domains are even from different organizations that have the potential to launch our attack to the top 1000 website。

 And this situation is confirmed by Baidu dot com。 of course， I know that talk is cheap。

 So in the next part， I will show a real world case that would be found in Microsoft。😊。

According to the measurements， we found that the same name record of a Microsoft domain。

 which is related to Windows update， was pointing to an registered domain。

 Aer can just buy and register this Q T L CDN net dot com。

 then configure the DN S A record to point to an attacker service I address。As a result。

 domain ownership can be verified and the attacker can issue a shared certificate than launch travel attacks。

And this is a certificate I previously obtained that shared with Microsoft domain。😊，Of course。

 the vulnerability has been reported。 So the certificate has been revoked by us。

 and Microsoft has already fixed this this this issue。

We recorded a video of launching crossside scripting on Microsoft domain in the past。Le is。

So after obtaining a shift the case through domain takeover， we set up an H T TP2 a tech server。

We then simulate a victim clicking a Meliss's link。

Our tech controlled server pushed an alert domain equipped to Microsoft website。

 and trigger the redra。Similar to the previous cross scripting demo。

 we successfully ex executed the alert domain script on the Microsoft domain。😊，Finally。

 let's discuss how to mitigate our attacks。We proposed the following countermeasures for browser vendors。

 They should enforce consistent authority checks in browsers to mitigate cross push and enforce single domain certificate to prevent cross SG。

For certificate authorities， we suggest they provide a mechanism allowing domain owners to remove their domains from issued shared certificate。

😊，For users， we advise them to inspect certificate status in domain registration to detect andauize share certificate。

So as you can see， our attack involves multiple parties， Bs， certificate other authorities。

 domain name systems。 therefore， addressing this security issue requires collaborative efforts from all stakeholders。

We also responsibly disclose our findings to affected vendors。

7 vendors confirmed the vulnerability and five vendors have fixed this issue， including Huawei。

 Baidu， Microsoft， etc cetera。We also launched a discussion on say B For Network security working group to discuss the weakness in P K I。

 Please feel free to join us discussion if you are interested in these issues。😊，So to sum up。

 we observed that H T T P 2 and H T P 3 ICN based origin is more person permissive than browser U I based origin based on this finding we propose two new cross origin web attacks cross push and cross SG。

 This attacks enable of pass attackers to launch practical web attacks with shared certificate。😊。

We also demonstrated how to leverage the weakness in Y P K I to facilitate our attacks。

We present how to leverage the misalignment between domain owner and the certificate owner to create attack condition。

 For example， by domain reing。 And we introduce， how can validation reuse extend certificate lifetime。

 thus extend attack duration。Also， we reveal that although you control the domain。

 you may not be able to easily revoke a certificate that contains your domain names。

This enable attackers to bypass some countermeasures， like revoking the unauthorized certificate。

And that's the end of my presentationation。 Thanks for listening。

 And everyone will now take question from the audience。😊。

