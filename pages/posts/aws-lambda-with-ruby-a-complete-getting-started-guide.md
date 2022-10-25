---
  type: posts
  title: AWS Lambda with Ruby: A Complete Getting Started Guide
  date: 2019-11-11
---
  
It's five o'clock on a Friday afternoon. There are no new bug reports and everything is looking smooth. Your plan of a relaxing weekend is in sight when you get a call—the website you look after isn't responding. Yikes.

AWS Lambda aims to minimize the chances of those truly terrifying events happening by taking care of server maintenance while you focus on coding robust applications.

AWS Lambda is a <a href="https://en.wikipedia.org/wiki/Function_as_a_service">function-as-a-service</a> platform that stores and executes your code when triggered by an Amazon Web Service (AWS) event. These events range from making an API call to saving a file to updating a database. You only pay for the execution of the function—there's no other associated running costs or server maintenance. And you don't need to worry about server load because AWS will handle scaling. That gives you a consistent experience, no matter how much your function is used.

This post introduces AWS Lambda, discusses when you should and shouldn't use it, and takes an in-depth look at how to create your first Ruby function using the AWS console.
<h2>The Origins of AWS Lambda</h2>
At the <a href="https://www.youtube.com/watch?v=UFj27laTWQA">2014 AWS re:Invent conference</a>, Amazon introduced Lambda to the world. Dr. Tim Wagner, general manager of AWS Lambda at the time, explained the benefits of using Lambda over <a href="https://aws.amazon.com/ec2/">EC2</a>, the popular compute engine. Renting virtual machines in the form of <a href="https://aws.amazon.com/ec2/">EC2 instances</a> from AWS brings flexibility and choice, but the tradeoff is that there's an unnecessary cost when idle. Plus, there's the additional complexity maintaining the infrastructure even when it's not in use.

<h2>The Benefits of Lambda</h2>
The benefits of <a href="https://stackify.com/aws-lambda-serverless/">AWS Lambda</a>, as explained by Tim Wagner at <a href="https://www.youtube.com/watch?v=UFj27laTWQA">re:Invent 2014</a>, are
<ul>
 	<li>Reduced complexity. There's a fixed operating system, fixed software language, and a fixed version.</li>
 	<li>No infrastructure to manage. AWS owns and manages the infrastructure. You only pay for the time your code runs.</li>
 	<li>Implicit scaling. AWS will handle scaling, no matter how much you use your functions.</li>
</ul>
<h2>An Explanation of AWS Events</h2>
An AWS event is a JSON message containing the origin and associated event information, depending on the service. For example, if you add a file to the <a href="https://docs.aws.amazon.com/lambda/latest/dg/with-s3.html">Amazon Simple Storage Service (S3)</a>, this will create an event containing the origin as the Simple Storage Service, the location of the file, and information that an item was added.

The services that can raise events that can trigger a function are
<ul>
 	<li class="listitem"><a href="https://docs.aws.amazon.com/lambda/latest/dg/with-ddb.html">DynamoDB</a></li>
 	<li class="listitem"><a href="https://docs.aws.amazon.com/lambda/latest/dg/with-sqs.html">Simple Queue Service</a></li>
 	<li class="listitem"><a href="https://docs.aws.amazon.com/lambda/latest/dg/services-alb.html">Elastic Load Balancing (Application Load Balancer)</a></li>
 	<li class="listitem"><a href="https://docs.aws.amazon.com/lambda/latest/dg/services-cognito.html">Cognito</a></li>
 	<li class="listitem"><a href="https://docs.aws.amazon.com/lambda/latest/dg/services-lex.html">Lex</a></li>
 	<li class="listitem"><a href="https://docs.aws.amazon.com/lambda/latest/dg/services-alexa.html">Alexa</a></li>
 	<li class="listitem"><a href="https://docs.aws.amazon.com/lambda/latest/dg/with-on-demand-https.html">API Gateway</a></li>
 	<li class="listitem"><a href="https://docs.aws.amazon.com/lambda/latest/dg/lambda-edge.html">CloudFront (Lambda@Edge)</a></li>
 	<li class="listitem"><a href="https://docs.aws.amazon.com/lambda/latest/dg/services-kinesisfirehose.html">Kinesis Data Firehose</a></li>
 	<li class="listitem"><a href="https://docs.aws.amazon.com/lambda/latest/dg/with-s3.html">AWS Simple Storage Service</a></li>
 	<li class="listitem"><a href="https://docs.aws.amazon.com/lambda/latest/dg/with-sns.html">Amazon Simple Notification Service</a></li>
 	<li class="listitem"><a href="https://docs.aws.amazon.com/lambda/latest/dg/services-ses.html">Simple Email Service</a></li>
 	<li class="listitem"><a href="https://docs.aws.amazon.com/lambda/latest/dg/services-cloudformation.html">CloudFormation</a></li>
 	<li class="listitem"><a href="https://docs.aws.amazon.com/lambda/latest/dg/services-cloudwatchlogs.html">CloudWatch Logs</a></li>
 	<li class="listitem"><a href="https://docs.aws.amazon.com/lambda/latest/dg/with-scheduled-events.html">CloudWatch Events</a></li>
 	<li class="listitem"><a href="https://docs.aws.amazon.com/lambda/latest/dg/services-codecommit.html">CodeCommit</a></li>
 	<li class="listitem"><a href="https://docs.aws.amazon.com/lambda/latest/dg/services-config.html">Config</a></li>
</ul>
<h3>Events Trigger a Lambda Function Execution</h3>
Events trigger a Lambda function to execute. It's executed either directly or is added to a queue to be executed in the future. If you'd like the full list of all how services create events, you can check out the <a href="https://docs.aws.amazon.com/lambda/latest/dg/lambda-services.html#intro-core-components-event-sources">AWS documentation</a>. But in short, a Lambda function subscribes to the notification-specific event from a specific source and will ignore all other events from any other source.
<h2>Programming Language Support</h2>
AWS periodically introduces support for new programming languages and currently supports seven different languages. Lambda supports writing code in <a href="https://stackify.com/java-tutorials/">Java</a>, Go, PowerShell, <a href="https://stackify.com/node-js-performance-tuning/">Node.js</a>, C#, Ruby and <a href="https://stackify.com/learn-python-tutorials/">Python</a>. As for the latest supported language, that's <a href="https://aws.amazon.com/about-aws/whats-new/2018/11/aws-lambda-supports-ruby/">Ruby support</a> for version 2.5.
<h2>Use Cases Where Lambda Excels</h2>
Lambda excels when the task has one or more of the following characteristics:
<ul>
 	<li><em>It's isolated.</em> Tasks that perform only one function—for example, updating a database or storing a file—are great.</li>
 	<li><em>It's asynchronous.</em> We're talking about low priority tasks that don't expect an immediate response. This could be sending an email or SMS.</li>
 	<li><em>It broadcasts. </em>That means it's updating lots of different systems from one action, like creating a repository on GitHub.</li>
 	<li><em>There are big peaks in demand.</em> Lambda works well when, during different times of the year or day, the demand is different. This could occur during peak holiday season.</li>
 	<li><em>There's low throughput. </em>This occurs when a task happens rarely, such as a sensor that only reports on change (like a door).</li>
 	<li><em>It's a prototype or mock. </em>Lambda shines when you're working with a proof of concept before committing more time to a project.</li>
 	<li><em>It's stateless. </em>Nothing needs to be stored in memory.</li>
</ul>
<h2>Use Cases Where Lambda Is Not so Useful</h2>
Lambda doesn't excel where the task has the following characteristics:
<ul>
 	<li><em>It's long-running.</em> Functions are limited to 15 minutes, so any tasks that exceed that will fail.</li>
 	<li><em>It has a consistent load.</em> It may be cheaper to use a dedicated instance if the load is predictable.</li>
 	<li><em>It's intensive.</em> Functions can only allocate 3GB of memory, so any intensive tasks will fail.</li>
 	<li><em>It requires a specific operating system or language.</em> If tasks are tied to a specific operating system and have limited language support, Lambda might not help.</li>
 	<li><em>It has a stateful system.</em> Here, we're talking about tasks that rely on a state to be stored between functions—for example, storing and retrieving files locally on the server.</li>
</ul>
<h2>A Use Case for Writing a Lambda Function</h2>
Imagine you're building a solution for users to store files. You choose AWS S3 as the location of your files because it's easy to manage and scale. The users can name the images anything they want, but you want to shorten the names to fit with the user interface design of a new app.

Renaming files is an ideal task for a Lambda function because it's a small isolated function, and it doesn't have to be run immediately. Plus, it's stateless.
<h2>The Starting Point for Your Ruby Lambda Function</h2>
The starting point for creating this function is a Lambda concept known as a handler. A handler is a method written in a Ruby file. In <a href="https://stackify.com/ruby-tutorials/">Ruby</a>, file names are normally all lower case and separated by an underscore. For this example, we'll use the default file name when creating a new Lambda, <strong>lambda_function.rb.</strong>

The <strong>lambda_function.rb</strong> file contains a method, and its default name is <strong>lambda_handler</strong>. This method contains the logic, and it will be run on an event trigger.

The first named argument for the <strong>lambda_handler</strong> method is the <strong>event</strong>. The JSON AWS event converts into a Ruby object and is assigned to this argument.

The second method argument is the <strong>context</strong>. The <strong>context</strong> is assigned to a hash, which contains methods that provide information about the function, such as the name and any limits. A full list of all the information <strong>context</strong> holds can be found in the <a href="https://docs.aws.amazon.com/lambda/latest/dg/ruby-context.html">AWS docs</a>.

The below code shows my <strong>lambda_function.rb</strong> file and the method, <strong>lambda_handler.</strong>
<pre class="nums:false lang:ruby decode:true" title="Handler function">#lambda_function.rb
def lambda_handler(event:, context:)
end</pre>
The above code is the base of a Lambda function and the minimum required for the Lambda to execute.
<h2>Reading Information From an Event</h2>
The main method that handles the processing of our event, named <strong>lambda_handler</strong> above, accepts <strong>event </strong>as an argument. This parameter is a Ruby hash converted from a JSON string containing the origin of the event and any contextual information. For our example, this will contain the name of the bucket and key of the file stored in S3.

The bucket name and key in S3 uniquely reference the image we're storing. To retrieve these values from the event hash, update the <strong>lambda_handler</strong> method to reference the <strong>bucket</strong> <strong>name</strong> and <strong>key</strong> values from the event. See the code below for how this looks in the <strong>lambda_handler</strong> function:
<pre class="nums:false lang:ruby decode:true " title="Handler function">  def lambda_handler(event:, context:)
     first_record = event["Records"].first
     bucket_name = first_record["s3"]["bucket"]["name"]
     file_name= first_record["s3"]["object"]["key"] 
  end
</pre>
<div>
<div>The above code shows retrieving the first record of the <strong>event</strong>. There will only be one record in total when adding a file to an S3 bucket. The <strong>bucket_name</strong> and <strong>file_name</strong> are then extracted from the record.</div>
</div>
<div></div>
<h2>Shorten the Name of the File</h2>
To shorten the name of the file, we'll use standard Ruby libraries to extract the first 21 characters of the original file name.

One of the great aspects of using Ruby for writing Lambda functions is the simplicity of string manipulation. The below code will assign the first 21 characters of the variable <strong>file_name</strong> to the variable <strong>short_name</strong>:
<pre class="nums:false lang:ruby decode:true" title="Handler function">short_name = file_name[0..20]</pre>
<div>
<div>This code uses the fact that a string can be accessed the same way as arrays and selects the range from zero to 20.</div>
</div>
<div>
<h2>Using the AWS SDK</h2>
<div>Lambda functions are already configured to use the <a href="https://aws.amazon.com/sdk-for-ruby/">AWS SDK for Ruby</a>, so no gems need to be installed before we can use the library. To reference the SDK, add a <strong>require</strong> statement to the top of your <strong>lambda_function.rb </strong>file. The below code shows the <strong>require</strong> statement at the top of the <strong>lambda_function.rb</strong> file:</div>
</div>
<div>
<pre class="nums:false lang:ruby decode:true" title="Handler function"> require "aws-sdk-s3"
</pre>
<div>
<div>The code above loads the gem containing helper functions to communicate with the S3 service.</div>
</div>
</div>
<div></div>
<div>
<h3>Moving Your File From One Bucket to Another</h3>
</div>
To move your files from one bucket to another, reference the original image and call the <strong>move_to </strong>function on that object. The code below shows how to use the built-in SDK functions to move the file:
<div>
    
```
# Get reference to the file in S3
client = Aws::S3::Client.new(region: 'eu-west-1')
s3 = Aws::S3::Resource.new(client: client)
object = s3.bucket(bucket_name).object(file_name)

# Move the file to a new bucket
object.move_to(bucket: 'upload-image-short-name', key: short_name)
```
    
</div>
<div>This code will create a new client in the <strong>eu-west-1</strong> region, create a new <strong>s3</strong> resource, and use the resource to reference a specific object: in this case, a file. The next action is to move the object to a new location. The <strong>move_to </strong>method calls the S3 service to copy the original file to the new location. Then, it deletes the original.</div>
<h3>Putting the Code Together</h3>
When put together, the above code will achieve our goal of moving a file from one bucket to another, shortening the name to 21 characters.
<div>

```
require "aws-sdk-s3"

def lambda_handler(event:, context:)
  # Get the record
  first_record = event["Records"].first
  bucket_name = first_record["s3"]["bucket"]["name"]
  file_name = first_record["s3"]["object"]["key"]

  # shorten the name
  short_name = file_name[0..20]

  # Get reference to the file in S3
  client = Aws::S3::Client.new(region: 'eu-west-1')
  s3 = Aws::S3::Resource.new(client: client)
  object = s3.bucket(bucket_name).object(file_name)

  # Move the file to a new bucket
  object.move_to(bucket: 'upload-image-short-name', key: short_name)

end
```
    
</div>
<div>The above is a working Lambda function containing the code from the previous sections of this post.</div>
<div></div>
<div>Now that we have our function, we can configure AWS to run it in a Lambda when we upload a file. To achieve this we need</div>
<ul>
 	<li>An AWS account.</li>
 	<li>Two buckets—one for the initial upload and the other for storing the files once the name is changed.</li>
 	<li>A Lambda function triggered by upload of a file.</li>
</ul>
<h2>Setting Up an AWS Account</h2>
If you don't already have an AWS account, you'll need to set one up with Amazon. They have a generous free tier, which will cover creating this function and testing it. Amazon has a great guide on <a href="https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/">how to create and activate an AWS account</a>, so follow this guide if you don't already have one.
<h2>Create Two S3 Buckets</h2>
Create two buckets: one that will allow initial upload of the files and another to store the files once they have been renamed. Amazon has a great guide on how to <a href="https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html">create a bucket</a>, if you wind up needing help.
<h2>Creating Your Lambda Function Using the AWS Console</h2>
You can create a function by writing the code directly into the AWS console or by writing the code on your computer and then uploading to AWS. The rest of this post covers creating your function via the AWS console.

It's important to note that functions are located in a specific region. A region relates to a specific datacenter owned and ran by AWS. Services can't communicate between regions easily, so make sure the Lambda is in the same region as the S3 buckets.

If you're choosing a region for the first time, choose the region closest to you. You should also consider that some regions don't have all the features that others do. Concurrency Labs has a great blog post called "<a href="https://www.concurrencylabs.com/blog/choose-your-aws-region-wisely/">Save yourself a lot of pain (and money) by choosing your AWS Region wisely</a>," and it'll help you choose.
<h3>Create Your Function Using the Console</h3>
To create a Lambda function through the AWS console, navigate to <strong>https://console.aws.amazon.com/lambda</strong> in your browser. By default, this will open the console in your default region. My default region is <strong>eu-west-2</strong>, located in the nearest location to me, London.

Select <strong>Create function</strong> to create a new Ruby function in the <strong>eu-west-2</strong> region.

<img class="aligncenter size-large wp-image-18830" src="https://www.hitsubscribe.com/wp-content/uploads/2019/06/lambda_config-1024x427.png" alt="configuring lambda" width="1024" height="427" />

The above image shows the values you should select to create a new Ruby function successfully. Once you have selected these values, click <strong>Create function</strong>. This will display the designer view shown below:

<img class="aligncenter size-large wp-image-18828" src="https://www.hitsubscribe.com/wp-content/uploads/2019/06/designer_view_1-1024x272.png" alt="designer view" width="1024" height="272" />

This is a visual representation of your Lambda workflow. On the left is the trigger and on the right is the flow from the trigger to the output of the function.
<h3>Trigger Your Lambda Function From an Event</h3>
To set up a trigger for your Lambda function, select S3 from the designer view. This will open the configuration menu for the S3 trigger, as shown below:

<img class="aligncenter size-large wp-image-18832" src="https://www.hitsubscribe.com/wp-content/uploads/2019/06/trigger_view-1024x414.png" alt="configuring trigger" width="1024" height="414" />

The above trigger view shows the configuration options for S3. Select the bucket that contains the uploaded images in the <strong>Bucket</strong> dropdown and press <strong>Add</strong><em>. </em>Your Lambda function will now run whenever an image is uploaded to that bucket.
<h3>Configure Your Lambda Function</h3>
Select the Lambda function box from the designer view. This will open the <strong>Function</strong> code view, shown below the designer, already populated with an example function. Copy the code we created earlier in this blog post into the <strong>lambda_handler</strong> method in the <strong>lambda_function.rb</strong> file:

<img class="aligncenter size-large wp-image-18827" src="https://www.hitsubscribe.com/wp-content/uploads/2019/06/code_in_lambda-1024x445.png" alt="shows code in lambda" width="1024" height="445" />
<h3>Give Lambda Permissions to Write to S3</h3>
To allow your function to copy the file from one bucket to another the function requires some extra privileges. Select the Lambda function and scroll down to the configuration section shown below:

<img class="aligncenter size-full wp-image-18831" src="https://www.hitsubscribe.com/wp-content/uploads/2019/06/role_in_lambda.png" alt="change role in lambda" width="782" height="344" />

The above image shows the Lambda configuration section, focused on <strong>Execution role. </strong>Click the link named <strong>View the RenameFile-role-&lt;unique value&gt;</strong><em>.</em> The link will redirect you to the AWS IAM service. Click<strong> Attach policy</strong>.

<img class="aligncenter size-large wp-image-18826" src="https://www.hitsubscribe.com/wp-content/uploads/2019/06/attaching_policy-1024x487.png" alt="attaching s3 policy" width="1024" height="487" />

The above shows the attach policy view of the IAM service.

Now, search for <strong>s3</strong>. This will show all the policies relevant to S3. Select <strong>AmazonS3FullAccess</strong> and press the <strong>Attach policy</strong> button. You should now see an extra box on the designer view, and Lambda will now have access to all buckets in S3.

<img class="aligncenter size-large wp-image-18829" src="https://www.hitsubscribe.com/wp-content/uploads/2019/06/designer_view_with_permissions-1024x268.png" alt="designer view access to bucket" width="1024" height="268" />
<h3>Test Your Function</h3>
Upload a file with a name longer than 21 characters to your image bucket. The file will shortly be deleted and transferred to the small names bucket and be 21 characters long. And there you go—test passed!
<h3>Writing Lambda Functions in Ruby</h3>
Ruby is a great language, and I'm really happy AWS is now supporting Ruby functions in Lambda. Once you're comfortable with the basics and want more advanced functions, <a href="https://serverless.com/">Serverless Framework</a> is a great next step. Serverless Framework allows you to package up gems to use in your function outside of those provided by Lambda. It also helps with versioning and organizing your code, along with storing the configuration for your functions.

Another tool that could speed up your development is <a href="https://rubyonjets.com/">Ruby on Jets</a>, an easy-to-use Rails-like framework to create API endpoints and recurring jobs. These tools provide an abstraction on top of Lambda, and the generated functions can always be verified via the AWS console.

<a href="https://aws.amazon.com/cloudwatch/">CloudWatch</a> is the default monitoring tool created by AWS to keep track of errors in your Lambda functions. It's a good tool, but it's very labor intensive because looking through the CloudWatch logs is a manual process. Using a tool like <a href="https://stackify.com/retrace/">Retrace</a> could help monitor how and when each of your functions are being executed and <a href="https://stackify.com/ruby-performance-monitoring/">track any errors</a> together with context in a convenient dashboard.

Lambda is not the solution to every problem, and overuse might lead to unnecessary complexity. Take each use case one at a time and decide if Lambda is the correct solution for you.