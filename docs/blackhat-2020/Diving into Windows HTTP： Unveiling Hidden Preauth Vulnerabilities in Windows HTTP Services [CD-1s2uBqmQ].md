# Diving into Windows HTTP： Unveiling Hidden Preauth Vulnerabilities in Windows HTTP Services [CD-1s2uBqmQ]

Hello， everyone， I'm Ko。 I'm glad to share our research with you today。

 diving into Windows H TP on Wing hidden Pra vulnerabilities in Windows H T P services。😊。

We work for Cy Kul and Huaazhong University of Science and Technology。

 We have been focusing on cybersecur research， for years。Here's today's agenda， first。

 we will introduce the background of our research and give an overview of the H TP service framework。

 Then we will share the details of several logic bases。D， O。

 S vulnerabilities and the tricks we used to find them。 After that。

 we will take talk about the details of remote cost execution vulnerabilities of H T P services。

 Finally， we will end with a brief summary。😊，First， let me introduce the background of our research。

We focus on researching Windows H T services， mainly because almost all of them can be access without。

They don't require user interaction and extra configuration。 Most importantly。

 many default services are built on top of H API。 This means if there are any vulnerabilities。

 they could lead to serious problem。Let me briefly introduce H P services。Services based on H T P。

 A use functions like H T create server S function and H T P S URL function to register various H P service information。

After these H T P services are registered， we can use H P query service configuration function to get various information about them。

😊。

![](img/bb255093dd3f06fbce8a814d5f4dbef5_1.png)

We can easily use the net message command to get information about OH P services。

This helps us quickly identify targets for one of research。



![](img/bb255093dd3f06fbce8a814d5f4dbef5_3.png)

In the second part， I will share the framework of HTTP services。

Web applications based on H P A S interact with the kernel component H TP。

 through interfaces provided by H TP A S。This interfaces use device air control function codes with specific I control codes。

😊，Finally， ST pieces handles cut like。Receiving and sending H， TP protocol data。

Here's a simple framework of H TV services。All Windows H P services based on H P A work under this score。

This is the simplest and most common HP service processing floor。

It's worth mentioning that almost all HP APII services don't require pre authentication。

 only a very small number of services authenticated。

Clients after receiving the H A P headers using H， N， T。

 L M or other authentication data in H A P headers。 But even in those cases。

 the receiving stage itself doesn't require authentication。H TP services involve two important H。

 T B API functions。The first one is H T P receive H TP request function。

 which is responsible for receiving H T P header data。

The service calls H P receive request entity Buing function to receive the H。

Such as in receiving cost data situations。To summarize。

 here's the general H TP service processing for。Now that we understand the H T B service framework。

 I will share some logic based D O S vulnerabilities caused by incorrect use of H T B A S。

For D O S tech vulnerabilities， crashes caused by memory corruption are not the only concern。

For an HP service， if the server stops handling normal client requests。

 that's also a form of E O and high impact。First， I focus on the receiving stage of H TP services。

Different services use different receiving methods， which I grouped into three types。

 one think receiving method and two I think receiving methods。Here's an example of think receiving。

 Its main feature is using a single thread。I will introduce C E 2024，4，3，5，1。

2 in the avoidable function。 H P receive H T P request is said to receive a fixed length of 0 x 1，3。

6，0 B for the H P header When an attacker sends an H T P header longer than 0 x 1，3，6，0 B。

 The function returns 0 X E A， which means the receive buffer is too small。Normally。

 the service should update the buffer and length。Seircle the receive function again。

But this service doesn't update the receive buffer size。 It stays at 0 x 1，3，6，0。 As a result。

 the service keeps looking， trying to receive data， but never accept data from other clients。

After the vulnerability is triggered， normal plans only receive timeout errors。

The second type is Ithink receiving in this method。

 receiving is still done by a threat within the service， but it doesn't wait inside H TP。

 receive H TP request function。Next， I will introduce CE to the 2025。2，7，4，7，1。

The U PN P service is a typical example of this kind of ethnicthink H P service。Since H。

 T B receive H T B request function is handled through a myself。

 This causes causes a time gap issue When a attacker sends multiple malicious requests。

 at the same time， H T B receive H T B request function doesn't return 0 or 0 x 3 E5 as usual。

But instead， returns an error early。 This means number of bikes re transferred。

 Its not updated after it's initialized as 0。If the error returned is。0 X E A。

 the receive is set to 0 instead of the correct value， which is said with。

That overlap the result function。We can observe this logic issue by setting a conditional breakpoint to print the return value of H P receive H T P request function。

 When the problem is triggered， the function keeps returning anarrow forever。

Even if the attacker disconnects。The function will continue looking endlessly。As you can see。

 normal UPNP clients requests receive proper H T B responses。

 but once the vulnerability is triggered， clients can no longer communicate with the server。

The third I think method， which uses register codeback。

This is the most common way to handle HP requests in Windows services。With this method。

 multiple threats can handle requests from multiple clients at the same time。

This is a common cobe handling pattern。 I have listed some commonque here。

 It's important to know that some cos are optional。

 These codes are registered based on the services needs。In the first two receiving methods。

 which use a single processing thread。 If the receive function stops being called to get new requests。

It leads to a D issue for the multi threaded codeback method。 After a function finishes processing。

 it creates a new thread in the thread pool and codes the receive function again。

But here I want to mention another D S technique to consider。

Since the number of threats in the thread pool is fixed。

 If each thread after finishing processing doesn't call start threat pool I function to create a new receive thread in the pool。

 but instead just returns， that threat will end。When there are no receive threats left in the threat pool。

 the H TP service will stop handling normal client requests。Next。

 I will introduce a vulnerability in the function resource。Soervice。In this service。

 I O result is the return value of H P receive H T B request function when it when it is 0。

 it means the H P request was received successfully。

The service processes the processes the request normallyly and finally cause issue receive request function In this function star thread to I O function is called to create a new thread in the thread pool。

 which then cause the receive function to wait for a new request， However。

 when I O results it not zero， meaning there was an error receiving data。

 The function returns immediately without creating a new thread。

This means that by repeatedly sending cracked packets， causing oil result errors。

 all threats in the threat pool will excite。As you can see， before the vulnerability is triggered。

 normal requests receive H T P response packets， but once the vulnerability is triggered and all threats in the receive threat to exit。

 all normal requests will time out。I reported this vulnerability last year。

 M myselfLC considered that although it pre B O， the service is thought to be enabled only with trusted networks。

 Therefore， they rated it as a low severityty。In the receiving stage。

 I introduced three cases using these tricks。 I found many similar issues in Windows H P services。

From studying these vulnerabilities， I believe that for H A B services。

 the handling functions should never stop receiving， in other words。After any request。

 whether valid or invalidd， the service must call H P receive H P request function with request I D set to 0 to wait for new client request。

It's especially important to carefully check the functions return values and to properly update and validate the receive functions parameters。

Next， we focus on the response stage Here。 Two functions are involve。

 The first one is H T P send H T P response function， which sends the H T P response packet。

H TP console H T P request function。 foribly it connects the client。

 causing the client to receive an error。The connection between plant and server is actually established in kernel driver。

 It creates a launch to through allocate connection for look site function and initialize。

Some required structures。Similarly， when the server actively disconnects。

 it calls free connection for look set function to close the connection。

 release related structures and decrease the reference count of connections。😊。

When reached this point in my analysis， I wondered what happens if the server doesn't actively close the connection。

 In other words， what if it never cause H TP send H TP response function or H T P console H P request function to close the connection。

I found this situation in some services。 After receiving and person data。

 they don't call HP send H Ps response function or HP console H P request function and instead immediately start receiving new data。

When I debugged the tunnel， I saw that H PC doesn't close the connection or decrease the connections reference count。

This can lead to non pitch pool exhaustion in kernel。Here。

 I take the branch cake service as an example。 The related function has documentation available on MSDN。

 which defines the structures for different types of post data。



![](img/bb255093dd3f06fbce8a814d5f4dbef5_5.png)

In the post data structure， we can specify what kind of data the service should handle When we send credit data。

 the service throw an exception using the through helper function。

Then the survey calls H T P receive H T P request function in the registered call back to receive new data。

 but it doesn't call H T P send H T P response function or H T P cancel H T P request function to close the connection。

This， causes the connections in H PC to remain on release。Here is a simple demo。First， using Mac S。

 we can see that the bread cake service is running along with its URL and the servers I P address。

Next， we ran the QC from an authenticated client。

![](img/bb255093dd3f06fbce8a814d5f4dbef5_7.png)

You can observe the lumpage growing， which eventually leads to a full system B， O S。

These types of vulnerabilities cause the lumpage pool to be exhausted。Leading to system how or B。

 S O D， I believe the survey must always respond after processing whether success or fail。

By calling a response function or actively close the connection using a register disconnect call back。

Finally， let's talk about I S。 I S is a framework， and it itself is also a service service built on top of H T P API。

😊，I S provides a basic framework for some Web services。

 helping them handle receiving and responding to requests。

Web services only need to process data through S API extensions。

 It's worth mentioning that common web servers using C shop or dot AP X are also supported at the core by Windows building S API extensions。

Through S APII and CG CGI restrictions， we can find the S APII extensions used by different services。



![](img/bb255093dd3f06fbce8a814d5f4dbef5_9.png)

For services running on I S， when a client sends an H TP request。

 I S uses a command to start W 3 WP process for the corresponding service。

Once the service is running， we can also see its process via net。😊。



![](img/bb255093dd3f06fbce8a814d5f4dbef5_11.png)

In this process， a very important data structure is I API contact。For each service on I S。

 when W 3 WP process is created， an I APII context structure is also created。

With the I S service accesss， the I S API context structure is released。For this structure。

 each time a request arrives for the specific service。

 I S checks the reference count of the structure。I S set a reference limit of 0 x 1，3，6，6。

If the reference count exceed 0， x 1，3，6，6， I S returns a 5，0，3 service on available error。

Uly receiving I S can handle the data reception， but the I S API extension De must decide when to send the H P response to support this。

I S provides the server support function， which is a dispatch function with different branches for various needs。

To ensure the S API contact the reference count remains a balance。

Some branches in several support function will decrease the reference count of S API contact。

Because server support function manages the S A API contact reference count。

 and this function is used by different services on S API extensions。

 The S extensions must be very careful when using。Incorrect use of server support function can cause problems with the reference card。

Such as premature release， leading to use after free or denial service。Next， I will share C V E 2024。

3，8，0，6，7。 O， C SP is a service running under the I S framework。 O C S P I API is its E I。

A PI extension， it processes post data from client and responses result back to client。So finally。

 OCSP calls server function within send response to client function to send the H P response。😊。

In the branch of server support function， the server is first code the flash function。 Finally。

 in post completion function， the reference count of I APII contact is decreased。However。

 if an attackers plant disconnects early after sending the poster data。

 But before receiving the server response， the flash function returns an error。 As a result。

 post completion function is not code， and the S APII contacts reference count is not decrease。

When the reference count reaches。0 x 1，3，6，6。 The O C SP service will stop processing any requests。

Before the B O S is triggered， normal requests receive responses。After its trigger。

 clients will get a 5，0，3 service on available error。😊。

These types of vulnerabilities also cause permanent D O S。 More seriously， you suffer for free。

 but caused by reference count issues are common。If S APII extensions use server support function carelessly。

 similar problems can lead to remote code execution。When developing as API extensions。

 we must pay close attention to the reference counting of structures when using as support interfaces。

I have introduced logic based D S issues in the receiving and response stages of H P services。 Next。

 victory will present the L CE issue in H P services。Thanks for Ca's wonderful speech。 Now。

 I will take over to continue the presentation， discussing fascinating essay C cases。😊，First。

 I will introduce the H T TP service of KTC Proxy service abbreviated as KPS。

 It provides a mechanism for client to use a proxy server to change passwords and securely obtain curbu server tickets from a K server。

The client established the connection with the K， P， S server， via H， T P， S。The。

 the server resolves the I P address of the domain provided by the client。Subsequently。

 K P S establish the soet connection with the K T S server indicated by the I P acts as a tune forwarding messages between the client and the K T C server。

Here are three columns of content。 The first column lists the H T TP APIs used in the server。

 The second column shows the callback functions the server involves after events registered by the H T TP APIs occur。

The third column displays the events that trigger these callbacks。 As you can see。

 this is an aynchronized callback base H TP architecture。

When server calls H T B receive H T B request。If a client initiates a connection request。The server。

 the server process triggers the KPS S HB receive request I completion Co back function to handle the new connection request。

If the server is configured with a client certificate。

 it waits for the client to send a request with the certificate。Then， a server calls。

H TP receive request， entity body。To await the client's message。 If the client sends the message。

It activates K， P， S， receive request and。I O completion function。

 which continues processing the client's message。Since the service does not register H T B wait for disconnect。

 a server does not respond when the client disconnects the H T B's connection。Leing to new issues。

 though this is not our main focus。What server itself。Cause HV cancel H V request。

It subsequently triggers KPS HV cancel request I competition call back function in a new thread。

A server can use HP send response。To send messages to the client and unend completion to callback functions are triggered。

Once the connection is established， the server calls D S get D C name function。

 which carries the DN S server to obtain the I P corresponding to the domain。

Suppose the client provides the domain test yours do Z Z。It will carry two S RV records。

 assume the server returns ABC dot test your dot Z Z。

Then it assess this address using LD A P protocol to cur the Kber server address。

 say ABC D do't test your dot。After obtaining this address in a subsequent process。

 the server calls the MSFD interfaces socket to connect to our smoothed curber server。😊。

At this point， the K P server acts as a client。When it receives messages from the fake cable server。

3 corporate functions are triggered to process the received message。Next， let's discuss the case。

 C V E 20，24，4，3，6，3，9。In the KPS so receive data I completion callback process。

It encodedes messages through several functions， since a socket does not limit the length of received messages。

The message lens can be a full by lens。When calling the A S N1 in check function。

 as shown in the diagram on right， A2 is the current message lens and line 10。

 if  a2 is greater than the world 18。Is value is assigned sign to reign。In the next line。

Divward 18 and V 9 are added， and sum is assigned to return1。

 which is then used as a size to allocate memory。 If the message lens is， for example， F， F， F， F， F。

 F， F， B and Dward 18 is 5。Then wait will overflow to 0， resulting in a very small memory allocation。

 This leads to an overflow when writing to this small memory in the subsequent process with proper condition and layout。

 This can cause R C problem。Here is a crash stack trace。

 We can see that R C X points to a nonriable address。After discussing the KFC C proxy server。

Let's move to another case， The remote desktop protocol service。And its gateway service。

The remote desktop service is a built in component in Windows。

 enabling users to remotely control Windows systems of network。

This functionality is achieved through the RDP protocol using port 3，3，8，9。However。

 Windows remote desktop service also use code 3，3，8，7。

 which implements an H T TP service supporting web socket。

It allows client to complete remote desktop proof requests， via H TP。Now。

 let's explore the remote desktop gateway service。 As shown in the diagram。

 the client connects with the remote desktop gateway service via H T B， S on port 4，4，3。

 or detail S on port 3，3，9，1。😊，After a series of negotiation and authentics。

 the gateway service forwards the client's remote desktop protocol messages to a corresponding remote desktop server。

Now， let's examine the H TP architecture of the remote desktop gateateway service。

The remote desktop server and its gateway both employ a similar asynchronized callback based H T B architectures。

As you can see， it's similar to the K P architecture。

With the differences being that it has a registered callback function for handling connection disconnection。

And call back functions for processing both web socket and now web socket requests。Now。

 let's work through a normal connection process using this flow chart。First。

 the client initiates a website request to the server with a connection I D con I D 1。

 This triggers the servers。HTP receive request Comp callback function。During processing。

 it calls process out the channel or web So request function。

 which allocates the new connection structure for this connection and stores it in a h table。 Later。

 it calls H TP send the H P response to send the messages to a client。Upon completion。

 the server callback function， H D B send response completion function is triggered。

Which allocates H server connection structure。And binds it to connection 1。

In the subsequent processing， it calls receive data。

 where argument 3 is a pointer with an offset from the server connection structure。

Inside receive data function， it retrieves the connection 1。

From the hh table by using connection I D con I D1。

 and assigns argument 3 to the buffer field of a member in the structure。In later calls。

 through Web can receive raw data and cause H D P receive requests at its body to register a new callback。

When server receive the message from the client， it triggers this callback。During processing。

 it calls Web so receive loop。Performing a critical step。

 copying the received message to the address pointed by the bar field of Connect one member。

Which is the offset of HTV server connection structure。The additional operations follow。

So what issue might this architecture have。Can a client to connect to a server using the same connection I D。

In fact， this is not okay。As process of channel or web So request。

 checks if the I D already exists in the hash table。But。

Could we release the connection before the get operation。

 then have plan to connect using connection I D1。It seems possible。Let's deduce what would happen。

First， client 1 connects to the server with can I D1， and server creates connection1 structure。

 After sending， it creates H P server connection 1。Then it enters， receive data。At this time point。

We disconnect client 1。Since the disconnect callback is registered。

 the server automatically triggers handle disconnected。

Which de references H TV server connection structure。And removes connection1 from hash table。

HV server connection may not be released at this point as a reference to it exists until handle send response competition finishes。

Then we connect with client2 using the same connection I D and server。

 create connection tool and sting it into the hash table。Now， return to the receive data thread。

 It continues running， retrieves connection 2 from hash table using a connection I D1。

And assign A 3 to the above field of connection to structure。

Then it calls HB receive request antibody body。To register a call back function。嗯。人。

The handle center response completion function finishes。The reference C， A， H， P server collection 1。

So， there's no references。And argument 3 will be a dangling pointer。Later。

 when client2 sends the message to a server， it activates handle web soet。

 receive raw data completion callback function。😊，Inside the function。

 it retrieves connection2 from the hash table using call I D1。And write data to the dangling pointer。

Thus， we can overflow。To a daangling memory。Here is the stack at the crash site。

 As should it crashes in the memory copy function with R X pointing to a nonex address。Finally。

 let me briefly summarize。😊，Looking ahead。M， S RRC updated their S steel servicing bar for those related vulnerabilities。

 So no moneyty for resource exhaustion bugs。Second。

 logic based doors when a Beijing is still in scope for high value assets。Including these targets。

Third， R E vulnerabilities are also common in H T TP services。

 especially during the passing of the post data。 So try to fuzz it。Takeaways。First。

 apply useful technique across the entire attack service to uncover similar issues。Second。

Thoseors don't require crashes。 Lo flooraws in request handling alone can also permanently block services。

Third。Further reflection， the potential for doors and even R E may lie in the deeper。

 More fundamental logical of the target。

![](img/bb255093dd3f06fbce8a814d5f4dbef5_13.png)

Thankk you for your listening。那そ。