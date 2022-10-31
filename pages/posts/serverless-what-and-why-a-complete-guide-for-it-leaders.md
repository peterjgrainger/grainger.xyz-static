---
  
  title: 'Serverless: What and Why? A Complete Guide for IT Leaders'
  date: 2020-01-13
---
  
Has your laptop, phone, or projector ever stopped working when you needed it most? If so, you aren't alone. It's happened to me more times than I care to remember, and more often than not at the worst possible time. We expect these little glitches in consumer tech, but web applications come from a team of talented engineers and should be more reliable—right?

Unfortunately, web applications are often no more reliable than your laptop or phone.

You may have some vivid memories of failed demos of web applications. You begin the demo. The team want to show all their hard work, and your clients want to see their new product. The site starts to load, and . . . nothing happens. There's a blank screen. Reloading the page doesn't work either.

You stall for as long as you can, blame the WiFi, the laptop, and anything you can think of, but finally it dawns on you something serious must have happened. The server is down, the code has a bug, or there's one of a hundred other possible scenarios that means you can't access that page today. All you can do is apologize, bow your head, and walk out meekly.

What if you could solve these kinds of server maintenance problems by hiding the complexities of server maintenance?

That's what serverless aims to do. It handles the headaches of keeping the server up and running. It scales up and down to meet demand while you focus on your business.

This post describes what the term serverless means, where you can find these services, and how you can maximize their value. We'll look at the benefits of making some of your infrastructure serverless, and we'll consider some of the trade-offs you should know about.
<h2>Serverless Runs on Servers!</h2>
The term serverless is a little misleading. It contains the word "less," which suggests that no servers are involved. However, serverless runs on servers. The word "less" in this context means spending <em>less</em> time thinking about servers and more time developing new features for your customers.
<h2>What Are the Must-Have Characteristics of Serverless Services?</h2>
A service must have the following characteristics to be classed as truly serverless:
<ul>
 	<li>The provider handles installation, configuration, and upgrades of servers, operating system, and installed software.</li>
 	<li>There's no configuration or manual intervention, whether one person or a thousand uses the service.</li>
 	<li>If you don't use the service, you don't pay.</li>
 	<li>The more you use service, the more it costs.</li>
</ul>
Services may have more advanced features than this, but they have to at least contain these features to be part of the serverless category.
<h2>What Are the Most Popular Serverless Platforms?</h2>
The most popular serverless platforms fall into three categories:
<ul>
 	<li><a href="https://en.wikipedia.org/wiki/Function_as_a_service">Functions as a service (FAAS)</a>.</li>
 	<li><a href="https://www.techopedia.com/definition/24900/storage-as-a-service-saas">Storage as a service (STaaS)</a>.</li>
 	<li><a href="https://www.techopedia.com/definition/29431/database-as-a-service-dbaas">Database as a service (DBaaS)</a>.</li>
</ul>
Let's look at each one and consider its characteristics.
<h3>1. Functions as a Service (FaaS)</h3>
Functions as a service is the most popular use of a serverless platform. This kind of service allows you to upload your code, specify when to run it, and manage provisioning and maintenance of servers. <a href="https://aws.amazon.com/lambda/">AWS Lambda </a>was the first FaaS, and it's the most popular one available today. Other examples of FaaS are <a title="Google Cloud Platform" href="https://en.wikipedia.org/wiki/Google_Cloud_Platform">Google Cloud Functions</a> and <a title="Microsoft Azure" href="https://en.wikipedia.org/wiki/Microsoft_Azure">Microsoft Azure Functions</a>.
<h3>2. Storage as a Service (SaaS)</h3>
Storage as a service was one of the first uses of a serverless platform. This kind of service allows you to store any kind of file, no matter what type or size, with effectively unlimited storage capacity. <a href="https://docs.aws.amazon.com/AmazonS3/latest/dev/Welcome.html">AWS Simple Storage Service</a> was the second service that AWS introduced, and it's still one of the most popular storage solutions. Other examples of SaaS are <a href="https://azure.microsoft.com/en-us/services/storage/">Azure Storage</a> and <a href="https://cloud.google.com/storage/">Google Cloud Storage</a>.
<h3>3. Database as a Service (DBaaS)</h3>
Database as a service is one of the newer categories of serverless platforms. This service configures and maintains the database and is accessible through an application programming interface for any interaction. The provider handles configuration, maintenance, and performance optimization. Types of DBaaS include variations of NoSQL, SQL, and in-memory key-value stores. Examples of popular DBaaS include <a href="https://aws.amazon.com/dynamodb/">AWS DynamoDB</a>,<a href="https://aws.amazon.com/rds/aurora/serverless/"> AWS Aurora Serverless</a>, and <a href="https://docs.microsoft.com/en-us/azure/sql-database/sql-database-serverless">Azure SQL Database Serverless</a>.
<h2>Serverless Helps You Focus on Business Logic</h2>
The trend among cloud providers Amazon, Google, and Microsoft is to help customers focus more on business logic and less on managing infrastructure. Serverless is the natural evolution of cloud computing, enabling you to write business logic or store the data that's vital to running your company without worrying about maintaining the underlying hardware.

At first, cloud providers offered the rental of entire servers or a rack of servers. Configuring bare hardware is time-consuming and prone to error, but at the time there were no other options.

Next came the ability to rent a virtual machine (VM). Multiple VMs can run on a server, saving money and adding an extra layer of abstraction that saves you time.

More recently, hosting containers have become a new, more cost-efficient way of hosting multiple services on a VM. Containers encapsulate the applications running in them, with no need to understand the underlying hardware.

Skip to the present day, and serverless is the next level of abstraction on top of containers. Serverless platforms handle the creation and scaling of containers and VMs.

Below is a graph of the evolution of cloud computing. It starts with full servers, which require significant configuration, and proceeds to the minimal configuration required for serverless.

<img class="aligncenter size-large wp-image-21058" src="https://www.hitsubscribe.com/wp-content/uploads/2019/07/untitled_page-2-1024x716.png" alt="Evolution of Cloud Computing" width="1024" height="716" />
<h2>What Are the Benefits of Serverless?</h2>
Serverless doesn't solve every problem. How can you evaluate whether serverless is right for your use case? Convert a small portion of your infrastructure. Look for quick wins, such as file storage or encapsulated, stateless logic that's easy to extract and run independently.

When you're trying out your proof of concept, keep these factors in mind: the amount of time you're spending on configuration and maintenance, and the amount you're spending as compared with a traditional server.

Here are four significant advantages of serverless.
<h3>1. You'll Reduce the Time You Spend on Configuration and Maintenance</h3>
Reducing the time you need to configure and maintain your system is arguably the greatest benefit of serverless. The salaries of operations engineers are always rising. Even more important, the demand for skilled engineers is increasing. For these reasons, finding staff who can configure and maintain complex systems is increasingly difficult.

With the introduction of serverless, you can empower developers to build and manage services. This change gives your software developers the visibility and power to deploy and monitor production code easily. Your operations staff can focus on making deployments as easy as possible by breaking down the barriers between operations and development made famous by <a href="https://www.plutora.com/blog/agile-devops-effect-6-keys-software-delivery-evolution">DevOps practices</a>.
<h3>2. You May Save Money Using Serverless Instead of Traditional Servers</h3>
The cost benefits of using serverless over traditional servers are the reason many companies convert some or all of their architecture to serverless. This diagram shows how serverless removes the upfront cost of using the traditional server route, thereby lowering the barrier to entry.

<img class="aligncenter size-large wp-image-21112" src="https://www.hitsubscribe.com/wp-content/uploads/2019/07/serverlessvslegacy-1024x716.png" alt="Serverless vs Legacy" width="1024" height="716" />

The initial cost of serverless is zero. Compare that with the high initial cost of using traditional servers (because you'd need to decide the expected volume upfront). That cost is flat until the server is fully used, but then you'd need more servers to handle the higher traffic.

The cost-benefit will vary from business to business. The savings for businesses that are growing rapidly will far outweigh those where the growth is slower or flat. If the infrastructure isn't growing, then the cost isn't really much of a benefit over traditional servers.
<h3>3. FaaS Encourages Small, Encapsulated, and Independently Deployable Services</h3>
<a href="https://en.wikipedia.org/wiki/Function_as_a_service">Functions as a service (FaaS)</a> has these key characteristics:
<ul>
 	<li><strong>Only one entry point:</strong> This is an HTTP request or notification from another server, helping you to encapsulate your logic.</li>
 	<li><strong>Limited memory:</strong> Assigning more memory means a higher cost, encouraging you to focus on keeping the function small.</li>
 	<li><strong>Limited runtime:</strong> Each function has a maximum time it will be allowed to run for, which also helps you keep the function small.</li>
 	<li><strong>State isn't shared between functions:</strong> State can be stored in other services only, which makes each function independently deployable.</li>
</ul>
Making your services small, encapsulated, and independently deployable are key features of <a href="https://www.martinfowler.com/microservices/">effective microservice design</a>, allowing you to keep adding new features in a maintainable way.
<h3>4. With Serverless, Your Services Are More Reliable.</h3>
Serverless can help you avoid some of the most common issues with servers, such as:
<ul>
 	<li>A full file system.</li>
 	<li>The server restarting.</li>
 	<li>A corrupt file system.</li>
 	<li>Incorrectly configured operating system or software.</li>
 	<li>A bug in the software that causes the server to hang.</li>
</ul>
When a platform handles all the server management, these issues no longer exist. However, your code is still running on servers provided by the cloud provider, and outages still happen. A famous example of this is the <a href="https://eu.usatoday.com/story/tech/news/2017/02/28/amazons-cloud-service-goes-down-sites-scramble/98530914/">four-hour outage of AWS S3 in 2017</a>. Thankfully, these events are rare, and they happen far less often than they would if you were to manage your servers. Most cloud providers use advanced techniques and have specialist teams looking just at maintaining reliability. You can't sustain this dedicated effort through in-house IT teams without significant effort and cost.
<h2>What Are the Most Important Trade-Offs of Using Serverless?</h2>
What trade-offs should you expect to see when using serverless over traditional VMs or servers? Let's go through them one by one. Consider any limits before committing too heavily to a major restructure.
<h3>1. It's More Difficult to Monitor and Debug</h3>
One of the main complaints I hear after converting a monolith architecture to serverless is the increased difficulty of monitoring and debugging. The most noticeable side effect of the conversion is an increase in the number of interconnected services. These services are all either communicating with one another or with the user. Monitoring output from a monolith is easy to read because the context of the application is all in one place. Tracing issues between many small services is much harder.

Some excellent resources on <a href="https://rigor.com/blog/how-to-monitor-your-microservices">how to monitor your microservices</a> are available, but it's important when creating your services to take monitoring into account. Consider new log events carefully before adding them to the code. Too many logs make it difficult to track down an issue. Too few logs make it difficult to see the context of a problem. There are some effective tools out there to help with monitoring, such as <a href="https://www.datadoghq.com/">Datadog</a> and <a href="https://www.splunk.com/">Splunk</a>. These tools help funnel logs from all your different services into one place. They also allow some prefiltering and let you link services, so you can understand why something failed.
<h3>2. It's Harder to Coordinate Deployment</h3>
Difficulties in coordinating deployment also come from having more services with serverless architecture than with a monolith. You may not need to manage the underlying servers with serverless, but you do have to manage how you <a href="https://www.plutora.com/resources/videos/deployment-pipeline">deploy the code</a> and how you manage the communication between services. Unless your system is trivially simple, managing deployment of many services manually is difficult.

If you get to the point where you need better visibility and control of deployment, then using a service may help. <a href="https://www.plutora.com/">Plutora</a> is a <a href="https://www.plutora.com/platform/value-stream-management">value stream management</a> service that helps you build, test, and deploy your new features. Then it monitors the deploy to make sure the new service is working as expected.

From my experience, the businesses that have the biggest problems with deployment are those that have too many reliant services. Keep this in mind during the initial design phase, and you'll avoid lots of headaches later!
<h3>3. It's Harder to Run the Full Architecture Locally</h3>
A common complaint from developers when moving to serverless is that it's difficult to recreate the full architecture locally. Make a special effort to seek a solution that works for the whole team. You and your developers both want as little friction as possible when creating new features. I've seen many ways to approach this issue, and different solutions work for different teams. Here are the solutions I've had the most success with.
<ul>
 	<li><strong>A framework to recreate the architecture locally:</strong> <a href="https://docs.aws.amazon.com/lambda/latest/dg/serverless_app.html">AWS Serverless Application Model (SAM)</a> and <a href="https://serverless.com/">serverless framework</a> are two options to run the entire architecture locally.</li>
 	<li><strong>A cloud development environment for each developer:</strong> This approach allows each developer to create an identical environment to production in its own sandbox.</li>
 	<li><strong>Using container technology:</strong> This approach uses containers that replicate each service and run the entire architecture locally.</li>
</ul>
The best solution for your team may be one of the above or a mixture. The only way to find the most productive way of working is for the team of developers to try each one.
<h3>4. Cold Starts Can Have a Huge Impact on Speed of Processing</h3>
One of the major limitations of functions as a service (one category of serverless platforms) is the concept of cold starts. They can affect the speed with which you can respond to a user's request.

A cold start happens when your function takes much longer than usual to respond because it's no longer running on a VM. Your cloud provider will increase or decrease the number of VMs running your function automatically depending on the number of requests. However, if no one is using your service, then you have no requests coming in—and your cloud provider will stop all the servers.

When your service receives a steady stream of requests, your cloud provider will have one or more VMs running a version of your function. Responses from these already running functions are quick because the VM doesn't have to start before processing.

There are <a href="https://medium.com/thundra/dealing-with-cold-starts-in-aws-lambda-a5e3aa8f532">ways to avoid cold starts</a>. However, nothing is foolproof. Most of the workarounds rely on how the cloud providers scale—and that's out of your control. The best solution is to choose services with a steady load or those where the response time of the service isn't crucial to the business.
<h2>Serverless Architecture Is Becoming More Mainstream</h2>
There's no denying that serverless is becoming a more mainstream way of architecting your system. It's easy to get caught up in the hype and listen only to the amazing benefits. But if you do that, you'll find out about limitations only after implementation. Consider each service in isolation, create proof of concepts, and test with some real data. I can't give you a solution that will work for all use cases. However, I hope I've helped you spot some opportunities for improving some of your services and informed you about pitfalls of working in this new paradigm.