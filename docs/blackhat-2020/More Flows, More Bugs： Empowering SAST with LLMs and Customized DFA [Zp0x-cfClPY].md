# More Flows, More Bugs： Empowering SAST with LLMs and Customized DFA [Zp0x-cfClPY]

Good afternoon， everyone。 Thank you for being here。 I'm Yuan from Tencent Security Reading Lab。

 And today， I will show you how we are enhancing static code analysis。

 using large language models and customized data flow analysis。😊，So first。

 I will introduce what su is。 Next， how we can use L Ms to automatically identify source and think functions。

Then our customized enhancement to data flow analysis。 And finally。

 the results demonstrating the effect effectiveness of our approach。What is SA。

 Sas means static application security testing using SA tools。

 We can analyze source code without running the application。 For example。

 detecting SQL injection bugs。From the picture on the right。

 we can see that sa is now an essential part of Devack ops， automating security checks。

Popular tools include code Q L， fortify and so on。What is called Q L。

 It's now a product under Github。queries and libraries are open source under the M I T license。

 including DA DFA part。But the core engine is proprietary。 The workflow of code L has four steps。

First， download the source code of the project， then create a database using code L command and then write a Q L query and run it against a database。

 And finally， the query results show whether there are any bugs。And on the wall of fame of code Co L。

 we can see all the bugs uncovered by code Co L。 It's not a complete list， but so far。

 it has found 418 bugs。 and the numbers is still increasing。 Also on Github。

 we can set up our projects to run code Co L scans。

Either on a schedule or whenever this a pull request。 So does it mean no bugs anymore。

We tried using code L to detect recent high risk bugs。 like the ones listed in this table。

 we found there were a lot of false negatives。 We analyzed the root causes as listed in the last column and found two main reasons。

 First is in complete source and think coverage in built in publicationation rules。

 Second is disruptions in data flow due to insufficient support for certain language features。

For the first problem， I will introduce how to use L Ms to recognize source and think functions。

We have noticed that current methods rely heavily on manual work。

 One approach is manual definition where developers have to manually review the source code to identify sources and things。

 Another way is through community contributions like code Q L has default rules from the picture。

 We can see that key information includes library， module， function name。

But we have found both methods are pretty labor intensive。

 So is there a way to automate this process。First problem is。

 where should we look for sources and things。We have found that source and think functions are actually API functions from third party frameworks from the picture on the left。

 we can see that S S I think actually is an API in resty framework and implement implementation code for this API functions can be found in open source frameworks So we can detect sources and things by scanning those frameworks。

😊，The second problem is how A Ms can help。 On the right is the workflow of our method。

 It has three AI agents。 We first use the discover agent to identify potential source and think functions in the framework。

 Next， the judge agent applies expert rules to check the functions and remove results that don't align with expert experience。

😊，Finally， the validation agent wants queries to verify that the functions are used in real world projects。

😊，First， we run a cross grain detection at the file level。

 feeding the source code file and per to the model in the per。

 when many describe the characteristics of the functions we need to identify。

 which are related to the programming language and the bug types， for example， for SSF bugs。

 the sync functions are typically those sending H T T P requests。 And on top of that。

 we can set constraints on the format and style of the response。

 And you can try different pers to get the best results。 And this is just a reference。😊。

And for the L M's result， will remove results with low confidence scores。

 and the threshold can be adjusted depending on the per and the model you use。And in the end。

 we'll get a list of source and sync functions。To remove the false positives from the previous step。

 we move on on to function level filtering。 We check both the function names and function code。

 For example， with is I F。 the sync is a function that sends an H TP request。

 while for sQL injection， the sink is executing an sQL statement。

 And we have noticed that long thinking models tend to perform better at this stage。 And again。

 any functions that don't meet the criteria will be removed。 In the end。

 we collect all the source and sync functions present in each framework。😊。

Besides having the L O M directly scan， we also need to integrate expert experience。

 The experience comes from the community's longstanding practice。 For example。

 the function should be publicly accessible。 The function should have return values that propagate tented data。

And the function should， the return value of the function should not be of the boolean type。

 And there are plenty of these expert rules。And again。

 they can be added or revised based on different bug types or 10 types。

 And we'll use the L M to apply these expert rules to check the function name and function body here and removing functions that don't match the expert rules。

The final step is confirming that the source and think we have noticed identified are actually used in real projects。

First， we have to head to the framework project homep page and find the dependent page。

And this page lists most of the open source projects using that framework。And next。

 download the source code of these projects， Soing them by star count from highest to lowest。

And finally， run a Sa tool like code Co L with the new source Think rules applied to scan these projects。

 And based on the scan results， verify if the newly added source and sync is actually used and any source or think that isn't used will be removed。

😊，And for the second problem， I' will introduce our enhancements to DFA data flow analysis。

What is D FA in code Co L and other Sa tools on the left is an example of data flow analysis in code Co L。

 It works by extending data flow configure here。And implementing the source interface at line 10。

And is think interface at line 16。And then calling flowpath to perform a global10 analysis at line 27 here。

So on the right are the results From the results， we can see a list of numbered path nodes。

 And this list represents a complete data flow path。And each path node has two parts。

 the node and the access path。So how does the whole process work。

 And this diagram shows the workflow of DFA。 First， code Q L provides the confi user interface。

 and you can call the flow path API just the last previous slide。

 And then there are two key concepts node and access path。

Notode is calculated through forwardward flow and reverse flow。

 Forward flow is the forward publication from the source point。

 While reverse flow goes the other way， tracing backward from the sync point。 And together。

 they form a complete source to sync path。😊，An access path is calculated in five stages from stage 1 to stage 5。

 Each stage adds further details to produce the final result。

We use forward flow to illustrate how node is calculated。

 On the right is the diagram of codeQ L source code， which consists of different steps such as jump。

 store， and details are in the table。First is the source step。

 which sets the starting point for data flow。 Next， the local flow step。

 which handles intra procedural analysis。And then the jump step。

 it provides the interface for custom propagation from one node to another。 There are two interfaces。

 A user level interface through its additional flow step and a system level interface through additional value step。

😊，And then comes the store and load step。 Store is writing a ten node into a field， array。

 collection or map content where load is reading a ten node from them。

 And this is the core of access path。And finally， call in， call out and go through steps。

 which deal with interprocedural analysis。😊，Co in handles propagation from actual arguments to formal parameters。

 Co out and go through， deal with propagating the return values。

And the difference is whether the return value is propagated from external sources or parameters。

And notice that post updated notes here represents notes that arelic implic updated as a result of a function call。

So what is access path， It consists of a content list and a type。

 Con is basically a way to describe how data can be stored inside an object。

 and details are in the table on the left here。 So for a field type。

 its content is simply the field name。 for other types。

 it's more of generalized result as shown in the table here。

An access path keeps track of the content propagation relationships for the four types of nodes。

 field array， collection and map during store and load steps。

 And the table here shows examples of access path。So， for example， in the， this role。

 the content is name with the type string。Then how to calculate the access path。So on the right here。

 we write a sample code from the table。 You can see how the interry value。

Access path changes at each stage at the beginning at in stage 1， there's no A P at all。

 And at stage 2， A P becomes a bullolean type， indicating whether a content per operation exists。

 And at the last， we find out the complete access path is a content list here and a type。

And after an overview of D FA， now， we introduce about some of the challenges it faces using Java as an example。

So static analysis toolss tools include code Q L face challenges such as cross thread issues。

 reflection and value passing。So here is an example of cost threat analysis。

The parameters of the static static main method are the source point。

And the print line method parameters are the think point。So on the left here is the code。

And on the right is the intermediate results of the static analysis。

And as we can see in the default analysis， we can't find a path from source to sync。

 It breaks at constructor call of roundable demo at line 14。So how do we connect it？

 Remember the jump step we previously introduced， we can use jump step。

To jump from the runable demo call to the instance parameter of the wrong method。

 namely the this parameter。 And after the patch， we get the complete path。

 So to summarize when the tent is propagated through the constructor over runable instance。

 our approach is to jump from the constructor call to the wrong method。

And this is the implementation using the additional value step interface。

 And we complete the jump step here。But what if the10 isn't passed through the constructor。

 such as through assignment statement in a function call in at line 8， How do we handle this。

 Our solution is that if the ten0 exists before the start interface is called like line 17。

 we take the color of the start method and jump to the instance parameter of the wrong method。

 So from here to here。So in， in this case， jump from the line 18 to 10。 And after the patch。

 we can get the complete path。And what if the 10 comes after the stock interface， in this case。

 line 23， our approach is to find a node matches three conditions。 First。

 this node is a post update note。 Second， this node is a roundable instance。 Third。

 this node has a store operation。😊，So we jump from the that node to the instance parameter of the wrong method。

 So in this case， jump from line 8 to line 10。And after the patch you can see the complete flow is detected。

Next， we'll introduce the second challenge， Java reflection。

And take the invoke method in this code as an example， which is line 13。

 we are facing two difficulties here。 First， how do we pinpoint the method that invoke actually correspond to。

 So namely， what is the method at line 13。 Second， how do we fix the propagation relationships for interprocedural cause like call in and call through。

 So from the red line， we can see that the parameter propagation logic is different from the normal core process。

For challenge1， there are two solutions。 One approach is to track the method instance。

 as illustrated in this diagram。And the other is to determine the method based on the number and types of parameters in the invoke method。

And we have implemented both methods and note that this tracking requires global data flow analysis。

The second challenge， how do we make reflection analysis work within code L's data flow。

We mentioned earlier that additional value step can define edge to connect to nodes。

 But here's the problem。 You can't use data flow inside additional value step because doing so would cause a non monoonic recursion issue。

Basically， you need data flow interface in additional value step。

 but the data flow interface depends on additional value step。 So as illustrated in the diagram。

So how to fix this， we can， we can create a copy of the data flow analysis implementation and have our reflection analysis rely on the copied version as illustrated here。

 And then we patch the original data flow here to avoid any dependency issues。

And to support the invoke method， we fix the parameter propagation logic for co in and co through。

 For example， for co in， augmented propagates to parameter and object propagates to the parameter this。

And here's an example for reflection。Firstly， the data flow breaks other invoke method at line 13 here。

And by adding reflection analysis as shown in steps 12 4。

 we can propagate from 10 S here to 10 object here。 And thus we get the complete path。And next。

 pass by value in Java。So first， what is a pass by value in programming languages。

 parameters are generally passed either by value or by reference。 And in Java。

 the way actual parameters are passed to methods is passed by value， not pass by reference。

 So if the parameter is a primitive type。 and what gets passed is a copy of the actual value of the primitive type and a copy is created。

 And if its a reference type， what gets passed is a copy of the address value in the heap of the object referenced by the actual parameter。

 And just like with parametersitives， a copy is created。So what's the problem。

 And on the right is a case where a copy is stored in a field。

 But the problem is there are many copies like a person P and D dot A here in this example。

 So the core issue is that when the value is modified in one copy， all copies need to be updated。

So in this case， it is the pair parametermeter person P in the constructor of demo field instance here。

 the copies are indicated with the。Blue line here。And here's our solution。So first。

 we have to locate the field， which is of a non primitive type。 And in this example。

 it correspond to field A for this field， finance store operations identifying both non post update node and post update node。

 So in this example。Sorry。it， it correspond to line 7 and line 11。 line 7 is a non post update node。

 While line 11 is a post update node。 And for non post update node。

 store operations use global data flow to locate the parameter and the actual argument。

 So in this example， and as shown it in the red dotted line here。

 we located the parameter P here in the demo field constructor。

And for another store operation of a post update node， we add a mapping from this node to this node。

So this is the jump step we have previous introduced。

And we have to use global data flow because we might encounter scenarios like interprocedural calls。

 store and load operations and so on。And then Ill introduce the results of our research。

We have found about 190 source and sync functions across 18 goal frameworks。

 We also scanned over 5000 projects and found that the detected data flows increased by more than 15%。

And we use this CE we found to show how a new sink could lead to a detection。

 And this is a sQL injection bug in traffic ops part of Apache traffic control project。

 This project has Q code Q L enabled here。 So which means it is a force negative for code Q L。😊。

And here's the data flow of the bug。 First step is to read the parameters from user input here。

 which is a user's comment， and then process and validate the requester parameters as illustrate here and this info dot go file。

 And finally， when inserting the user's comment into the database here。Here。

 it caused the query row X function。But when we checked code Q L's detection rules。

 we noticed that query Ro X function isn't included in the sync model。

And we scanned the simple X function， framework and found this sink as illustrated here。 And thus。

 we detected the bug。And with our enhancement， we can detect historical CV Es that were previously undetectable。

 And in this case， adding the source function enables the detection of three CV Es。

 Several open source projects are affected。😊，And for this C，E。

 we fixed cross thread and missing sync role。 And thus we can reproduce this CE。😊。

And this C proves the effectiveness of a reflection enhancement。And we also found many new bugs。

 And here are some cases below。And finally， are our takeaways。

 So first is that semantic analysis of code in is particularly suitable for LO M assisted analysis。

 And their combination is a research direction。 Se is that code Q L's data flow analysis mechanism is highly representative。

 serving as a good start for learning data flow analysis。😊，And third。

 code Q L data analysis is not perfect and can be studied， modified and improved。So any questions。

 And also， please feel free to reach us through the emails here。Okay， thank you。

