Benefits of cloud computing (discussed in “overview of AWS web services whitepaper”) (**)
1. Trading large capital investment for variable expenses - only pay for what you use
2. Benefit from economies of scale - you can get access to resources through AWS that
you never would usually be able to on your own
3. Stop guessing about capacity - you don’t buy to much and waste $, but you don’t buy to
little and have bad service levels
4. Increase speed and agility - you offload a bunch of your server admin responsibilities,
provisioning servers, data centers, etc. can POC things very quickly
5. Stop wasting money and time on infrastructure when you should be developing
applications instead
6. Go global in minutes
3 types of cloud computing (**)
1. IAAS - you manage the server that’s hosted by someone else (AWS)
2. PAAS - you just write your application and the vendor manages the hardware and OS
software, etc.
3. SAAS - the vendor provides the software and everything else, including hardware and
supporting software (think Google Apps)
3 types of cloud deployments (**)
1. Public cloud - everything is in the cloud
2. Hybrid - some of your company’s data/apps are in the public cloud and some are on your
own internal network
3. Private cloud - you maintain your internal network and your own data centers/servers.
For cloud practitioner, you only need to know the following services: compute ,
migration/transfer, security/identity/compliance, network/content delivery, storage, AWS cost
management, and databases.
AWS global infrastructure (**) is broken up into
- regions - is a geographic region, with 2 or more availability zones.
- availability zones - each has a separate data center that’s filled with servers. Idea is to
mitigate local natural disasters.
- Edge locations are more prolific than regions, and are basically just places where AWS
caches content CloudFront. For example, S3 content can be replicated at an edge
location using CloudFront.
Support plans (**)
● Basic. Forums, that’s it
● Developer. $29/mo. Experimenting. Ask technical question and 12-24 hr response
● Business. $100/mo. Production use of AWS. 24x7 phone, 1 hr response for urgent
support
● Enterprise. $15k/mo. Mission critical software. Assigned a technical account manager.
15 min response to urgent issues.
All new services come out in the N virginia region.
Billing alarms (**) are setup in CloudWatch, but must first be turned ON in Account Billing
console > billing preferences. Unlike the video, to setup a billing alarm, you can’t just enter an
email address. You have to go to SNS (simple notification service). There you create a TOPIC.
Once you create the TOPIC, then you create a SUBSCRIPTION. The SUBSCRIPTION has a
protocol, such as HTTP or email, or calling a Lambda. You then attach the SUBSCRIPTION to
the TOPIC, and the SNS TOPIC is specified in the Billing Alert. Within CloudWatch, they give
you the ability to create a TOPIC right then and there, without having to segue over to the SNS
section.
When you create an EC2 keypair, it is unique to the region for which you created it in.
IAM
● (**) IAM settings apply to ALL regions (is global), not just the region in which you setup
the IAM policy/user/group.
● When you first start with IAM, you have to do a few things: activate multi-factor auth for
root, create individual IAM users, use groups to assign permissions, and apply an IAM
password policy.
● (**) The “root account” is the email that you used to setup with the AWS account initially
-- root has access to everything in the AWS console.
● You should secure root with MFA (**) -- You use Google Authenticator to setup MFA with
MFA device, requires you do download an app, scan the presented QR code, and then
enter the PIN.
● On the main IAM page, you can adjust the link page that’s used for you users to signing
to the AWS console -- to make is more friendly than just
[accountnumber].signin.aws.amazon.com
● There are 3 ways to access AWS (**): console access, programmatic access (for CLI),
and SDK.
● (**) When you create a new user, it’s best to assign them to a group. When you create a
group, you add policies to the group. In the create group, you can Filter Policies by IT
function (such as power user or admin or data scientist) to make it easier. The actually
policy is written in JSON format.
S3
● Just a place to put your files.
● (**) Is object based storage, which is just a key/value store - you use it to upload files,
and not install OS’s.
○ The key for the object is just the name of the file.
○ The value is the file data itself.
● (**) Files can be 0 to 5TB
● (**) storage is unlimited.
● You can version files (optionally)
● (**) Buckets contain the files, and although each bucket is set in a specific region, and
the NAME of a bucket is GLOBAL -- must be unique across ALL of AWS (not just your
account), because you access s3 buckets via http url (which is always
https://s3-[region_name].amazonaws.com/[bucket_name]
● (**) buckets themselves
● (**) You know that a file upload is successful because you get a 200 response
● (**) Consistency
○ If you PUT a new file, you can then read it immediately.
○ BUT if you DELETE a file, or PUT an existing file it might take a while to see the
changes (you might be the old version of the file or still be able to access the file
after you DELETE) -- this is called Eventual Consistency.
● Different guarantees for the durability (it won’t go away) and availability (it will be there
when you want it) for objects
● (**) Setting access
○ For specific file - use Access Control Lists
○ For entire bucket - use Bucket Policies
○ By default when you create a bucket, the permission is to block ALL public
access. In that screen, note that you are only ENABLING the feature to make
objects in that bucket as public -- even after you do that, you still have to actually
set the OBJECT/FILE permissions to public using Access Control List or by
making entire bucket public by using Bucket Policies
● (**) Storage classes, can change in real-time. You change the storage class for SINGLE
OBJECTS, not the entire bucket. However, “You can direct Amazon S3 to change the
storage class of objects by adding a lifecycle configuration to a bucket”
○ Standard - highest availability and durability, most expensive
○ IA infrequent access. Lower fee, rapid access.
○ One zone - IA. cheap storage, like IA but only used one availability zone.
○ Intelligent tiering - moves files to cheapest storage automatically
○ Glacier - cheaper still. Very durable, but availability is in the minutes to hours
○ Glacier Deep Archive - lowest cost and availability is up to 12 hours.
● Charged for: storage, requests, storage management, data transfer, transfer
acceleration, cross region replication
○ (**) Xfer acceleration: say you have a user in africa that wants to upload a file to
a bucket in US. the file is uploaded to an edge location in africa, and THEN then
file is transferred to the US bucket using amazons OWN network, not the general
internet.
○ (**) Cross region replication: resources are automatically replicated from one
bucket to a bucket in another region for quicker access
● (**) S3 storage scales automatically
CloudFront
● Typically, requests go directly to the files ORIGIN (the bucket/region where the file is
hosted)
● To make it faster, the file can be pushed to an EDGE LOCATION where it is cached for
subsequent requests for a specific TTL.
● (**) 2 types of DISTRIBUTIONS (basically the CDN name which consists of the edge
locations to which the ORIGIN is cached)
○ Web distribution for websites
○ RTMP used to be used for Adobe Flash media streaming, but most people
anymore just stick with web distribution.
● When you create a cloudfront distribution, you can check “restrict bucket access” so that
the ONLY way users can access the content is via cloudfront url and NOT using the s3
bucket url. Can also set min TTYL in the creation screen.
● (**) EDGE locations are both readable and writable, ie using s3 transfer acceleration,
you can send objects from EDGE to s3.
Creating an S3 website.
● After you create an s3 website and then upload your files, you then go to properties >
static website hosting in order to specify your index and error html pages.
● (**) if the website you are trying to host is “dynamic” (ie requires a database connection
like wordpress), then you CANNOT use S3 to host it.
● There’s two ways to make the files public
○ 1. You can click on the index.html in the object list and do actions>make public.
Have to do for each file. Or you can click on the file itself, go to Permissions, and
set Read for Public Access for Everyone.
○ (**) 2. OR you can go to bucket-name > permissions > bucket policy, and add a
fancy json object to allow permission for PublicReadGetObject policy. This
makes the ENTIRE BUCKET public!
○ Either way, neither of the above two will work unless you go to permissions >
block public access, and make sure that “block public access” checkboxes all say
OFF.
○ Then, on the main bucket list, you see the “access” change from “bucket and
objects not public” to “objects may be public” -- note that it says MAY be public
because you still have to do step 1 OR 2 above to actually make your files public.
It’s like a double check lock.
● If you go back to properties > static website hosting, you can see the “endpoint” which
tells the url for the website.
● (**) above in the s3 section, we said that the url for an s3 bucket is
https://s3-[region_name].amazonaws.com/[bucket_name]. However, when you use static
website hosting, the url to access the static website is
https://[bucket_name].s3-website-[region_name].amazonaws.com
STOOOP!!! That previous bullet doesn’t make sense. The lecture gave one url in one section,
but then gave that second url in the last section where we setup “static website hosting”. Turns
out there are names for these, either virtual-host style or path-style ways to reference s3
buckets. Based on my confusion about this, I found the following out while doing my own trials:
****These are for a bucket with PUBLIC enabled and STATIC WEBSITE HOSTING property
turned ON. These are my results as of 7-9-2019
* tests
- prefix with bucket (virtual-hosted style)
- including region
+ using index.html suffix - WORKS -
http://gg-website-1.s3-website-us-east-1.amazonaws.com/index.html
+ NOT using index.html suffix - WORKS -
http://gg-website-1.s3-website-us-east-1.amazonaws.com
- NOT including region
+ using index.html suffix - WORKS - http://gg-website-1.s3.amazonaws.com/index.html
+ NOT using index.html suffix - DOESNT WORK (Access Denied) -
http://gg-website-1.s3.amazonaws.com
- bucket name as path parameter (path-style)
- including region
+ using index.html suffix - DOESNT WORK ("Site can't be reached") -
http://s3-us-east-1.amazonaws.com/gg-website-1/index.html
+ NOT using index.html suffix - DOESNT WORK ("Site can't be reached") -
http://s3-us-east-1.amazonaws.com/gg-website-1/
- NOT including region
+ using index.html suffix - WORKS - http://s3.amazonaws.com/gg-website-1/index.html
+ NOT using index.html suffix - DOESNT WORK (Access Denied) -
http://s3.amazonaws.com/gg-website-1
* other notes
* whether or not the region is included does not matter as far as categorizing as vitual-hosted
or path-style
* path-style is going away in 2020
(https://aws.amazon.com/blogs/aws/amazon-s3-path-deprecation-plan-the-rest-of-the-story/)
EC2 intro
● (*) Just a virtual server, and can scale up/down based on usage.
● Pricing models (*)
○ On demand - fixed rate per hour of compute time. Best for low cost and highly
variable workload applications, or proof-of-concept apps.
○ Reserved - locked in for an annual contract (1 to 3 years), but much reduced the
per hour charge that you would otherwise pay. Best for apps with low degree of
variability in usaged, you save money by essentially getting a “bulk discount” on
compute time.
■ Standard reserved - the basic one where you pay for a contract and get a
discount -- you are locked into the EC2 instance type.
■ Convertible reserved - not as much of a discount, but you are allowed to
switch between EC2 instance types.
■ Scheduled reserved - you launch these in certain time frames (just for,
say, 8-9pm when you have a batch job that you know you have to run an
intensive processing load)
○ Spot - like stock, price changes all of the time. So if you are flexible on when the
instance runs, you can say “only give me an instance when the compute cost
drops below XYZ price”. Engineering applications -- doesn’t matter WHEN it’s
run, just needs to be cheap.
■ (*) “If your Spot instance is terminated or stopped by Amazon EC2 in the
first instance hour, you will not be charged for that usage. However, if you
terminate the instance yourself, you will be charged to the nearest
second. If the Spot instance is terminated or stopped by Amazon EC2 in
any subsequent hour, you will be charged for your usage to the nearest
second. If you are running on Windows and you terminate the instance
yourself, you will be charged for an entire hour.”
○ Dedicated hosts - you just buy your own dedicated servers. Usually only used if
you have existing software licenses or regulations predicates that you need to
have your own dedicated host.
● Don’t need to know specific EC2 instance for cloud practitioner, hopefully not for DA. did
say that need to know for sysops associate.
● Amazon Elastic Block Store (Amazon EBS)
○ Basically virtual disks that attach to EC2 instances and needs to be in same
availability zone as the EC2 instance to which the EBS volume is attached.
○ In specific availability zones, but replicate automatically
○ Types (*)
■ SSD
● GP2 - general purpose
● IO1 - IOPS (input / output per second) - for high performance
■ Magnetic
● ST1 - throughput optimized - low cost
● SC1 - cold HDD - lowest cost - file server ie low workload
● Magnetic - previous gen - don’t worry about
EC2 lab notes
● EC2 is a per region service
● When you are creating an EC2 instance and adding EBS, you can only boot off of GP2,
IO1, and magnetic. Only after you add the second volume do you also then see SC1 and
ST1.
● Security group - basically a virtual firewall in the cloud
○ To let all traffic in, you would specify “0.0.0.0/0”
○ To list a specific IP you would write “X.X.X.X/32”
● Keys
○ Public key is just like a padlock - can have one anywhere
○ Private key is like the key that unlocks the padlock -- you want to keep it secure
and not hand it out. Use to ssh into ec2 instance.
● Ports
○ 22 ssh
○ 3389 windows rdp
● Always design for failure - should have at least 2 ec2 instances in 2 separate availability
zones.
●
AWS CLI
● After creating user, you need to check “programmatic access” to allow to use with CLI
and/or SDK
● To get access key ID and secret access key, click on user and go to “Security
Credentials”
● Demo is showing how to use AWS CLI from within ec2 instance, but can do on local as
well.
● Reiterating, can interact with AWS
○ Console
○ CLI
○ SDK
Roles
● Creating role
○ Select service that is going to use the role - in the demo we pick EC2.
○ Then you pick a policy - in demo we pick S3 access. This allows an EC2 instance
to have access to S3
● The apply the role -- demo applies the new role to a running ec2 instance
○ Find the instance > Actions > instance settings > attach/replace IAM role
○ Select the role and apply
● One way to use CLI is to download set your users secret access key and access id in
the “.aws/credentials” file and use cli from local machine. A safer way is to create ec2
instance, apply LEAST needed privileges to that ec2 using a role, download the private
key for ssh-ing into the ec2 instance, and then use aws cli from that ec2 instance. That
way, you don’t need to save your credentials file on the ec2 instance, nor on your local
(other than the .pem file that is saved on your machine in order to ssh into the ec2
instance).
● Exam tips
○ Use role wherever possible instead of user credentials
○ Applying roles to ec2 instance in instantaneous
○ Roles are global and not applicable to only certain regions.
Elastic Load balancers
● Wherever possible, put your ec2 instances behind a load balancer. You can then easily
add more ec2 instances behind the load balancer for fault folerance
● Creating ELB
○ Select Type (*)
■ ALB - application load balancer - provide routing on the request level
■ NLB - network load balancer - operates at the connection level, for
ultra-high performance when you have static IPs.
■ “Classic” load balancers - phasing out, use in non-prod to keep cost down
○ Select VPC and availability zones
○ Select security group
■ In the demo, we select “WebDMZ security group” which if I remember
opened all IPs to port 22 and all IPs to port 80.
○ select/create “target group” -- is basically the collection of web servers that the
balancer balanced between
○ Register targets - select the servers within the Target Group that you want in
rotation
● At this point, we would have one EC2 instance hooked up to the ALB. When you create
a second ec2 instance, select a DIFFERENT availability zone than the first instance, to
enhance reliability.
● To add the second ec2 instance, you go back and click “target groups” on the ec2
screen -- “target groups” is under the “load balancers” link in the “load balancing” section
RDS 101
● 2 important featuers
○ Multiple Availability Zone - for durability and disaster recovery
■ Rds instance is replicated in another availability zone.
■ Traffic hitting the hostname will automatically fail over to the replicated
instance if the primary bites the dust.
○ Read replicas - for performance
■ If the primary fails, then the whole system fails
■ But you can setup so that your primary receives the writes, and then
reads come from the duplicated “read replica” rds instances.
● Db processing
○ Oline transaction processing (OLTP) - your basic CRUD ops, only getting one
record at a time
○ Online analytics processing (OLAP) - you want to slice and dice data and do
aggregates/metrics/etc.
■ This is best done in a datawarehouse to minimize performance impact to
transactional database
■ AWS DW is called REDSHIFT
● Elasticache
○ Improve database performance by hitting an in memory cache for most common
queries, instead of hitting db directly
○ Types
■ Memcached
■ Redis
● Demo
○ When you create an rds instance, you have the option to check “create replica in
a different zone” -- THIS IS ACTUALLY THE MULTIPLE AVAILABILITY ZONE
FEATURE OF RDS, **NOT** THE READ REPLICA! Ie, it’s a “misnomer”
○ When creating rds instance, you select VPC, availability zone, and security group
○ If you allowed aws to create the security group, then you go back to rds instances
> security groups (under the ‘network and security’ section) and edit the security
group to allow inbound connection on MYSQL port.
Autoscaling
● Autoscaling and EC2
○ Image existing ec2. Go to ec2 > click the desired instance > Actions > image >
create image
○ After creating image, you should see it in ec2 > image > AMIs
○
