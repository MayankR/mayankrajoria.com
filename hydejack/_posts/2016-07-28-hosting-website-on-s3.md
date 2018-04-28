---
layout: post
title:  Hosting a website on AWS using S3 and Route 53
<!-- date:   2016-07-28 18:16:01 +0530 -->
categories: [hosting]
tags: [aws, s3, static site]
comments: true
---

After purchasing a domain, comes the dilema of finding a good place to host it. Same was the case with my domain <http://mayankrajoria.com>. Since this is my personal website, I decided it would essentially be a static one. Now for such websites, AWS S3 is pretty much the cheapest and most reliable way of hosting. The S3 service provided by Amazon gives you “buckets”, into which you can store files. We make use of the fact that these files can then be served to the internet easily. So here are the steps for getting you started with S3 hosting for your own static website.

The steps: Get an AWS account :p. I can’t really help you much here because its a simple procedure and you only need a credit card. You also are elligible for the free teir for 1 year when you sign up. So essentially, you will be hostng your site for free for an year, ain’t that cool!! Also purchase a domain that you would like to host the website for. The domain **mayankrajoria.com** is purchased by me from **GoDaddy**.

{% lightbox /assets/images/s3/aws_home.tiff.png --thumb="/assets/images/s3/aws_home.tiff.png" --data="appfoundry_image_set" --title="AWS Home" --alt="AWS Home" --img-style="max-width:100%;" %}
*AWS Home*

Now click on S3 and the following home for the S3 service should open up.

{% lightbox /assets/images/s3/s3.tiff.png --thumb="/assets/images/s3/s3.tiff.png" --data="appfoundry_image_set" --title="AWS Home" --alt="AWS Home" --img-style="max-width:100%;" %}
*S3 Home*

Now we’ll have to create a bucket. No, not the one you take a bath with, but a container used for storage in AWS S3. We shall be putting our website code into a bucket. As you see I already have a bucket for mayankr.me. Lets go ahead and create a bucket for my new site, [mayankrajoria.com](http://mayankrajoria.com). Remember to use your domain name as the name of your bucket. Just select a region closest to your place.

{% lightbox /assets/images/s3/create_bucket.tiff.png --thumb="/assets/images/s3/create_bucket.tiff.png" --data="appfoundry_image_set" --title="AWS Home" --alt="AWS Home" --img-style="max-width:100%;" %}
*Create S3 Bucket*

Click create and you should see your bucket created. Wohoo!! Now click on your bucket to open it. It should be empty right now. Just create an index.html file and upload it to your bucket. If you don’t know how to go about it, simply use this code

{% highlight html %}
<html>
<head>
	<meta name="description" content="Personal website of Mayank Rajoria. 
	Here are my projects, vision, work and personal blog." />
	<title>Mayank Rajoria</title>
</head>
<body>
	<h2>Hi this is Mayank Rajoria</h2>
	<p>Welcome to my home on the internet</p>
	<p>My url is <a href="http://mayankrajoria.com">mayankrajoria.com</a></p>
</body>
</html>
{% endhighlight %}

Simple enough for our first page, right?

After uploading, the file index.html will appear at the root of your bucket. Click Properties on top right and click Enable website Hosting and enter `index.html` as Index Document. You don’t need to use an Error Document right now. See the below picture for reference, then click Save. Make note of the Endpoint, this is the url at which your bucket will be public at initially.

{% lightbox /assets/images/s3/enable_hosting.tiff.png --thumb="/assets/images/s3/enable_hosting.tiff.png" --data="appfoundry_image_set" --title="AWS Home" --alt="AWS Home" --img-style="max-width:100%;" %}
*Enable Hosting*

Next we need to edit the permissions of your bucket so that it serves your website to anyone accessing it over the internet. Just go to the S3 home page and select the bucket of your website. Go to **Properties > Permissions > Add Bucket Policy** and add the following text in the text box that comes up.

{% highlight html %}
{  
	"Version": "2012-10-17",  
	"Statement": [  
		{  
			"Sid": "AddPerm",  
			"Effect": "Allow",  
			"Principal": "*",  
			"Action": "s3:GetObject",  
			"Resource": "arn:aws:s3:::mayankrajoria.com/*"  
		}  
	]  
}
{% endhighlight %}

{% lightbox /assets/images/s3/bucket_policy.tiff.png --thumb="/assets/images/s3/bucket_policy.tiff.png" --data="appfoundry_image_set" --title="AWS Home" --alt="AWS Home" --img-style="max-width:100%;" %}
*Bucket Policy*

{% lightbox /assets/images/s3/policy_screen_shot.tiff.png --thumb="/assets/images/s3/policy_screen_shot.tiff.png" --data="appfoundry_image_set" --title="AWS Home" --alt="AWS Home" --img-style="max-width:100%;" %}
*Policy Text*

Cool, now our bucket is public and accessible over the internet!! Go to the url we noted as Endpoint and you shall see the website, mightly hosted on AWS.

{% lightbox /assets/images/s3/bucket_public.tiff.png --thumb="/assets/images/s3/bucket_public.tiff.png" --data="appfoundry_image_set" --title="AWS Home" --alt="AWS Home" --img-style="max-width:100%;" %}
*Bucket public*

Now to redirect our url `mayankrajoria.com` to the bucket, we hop over to Route 53 in AWS. You can see, I already have a hosted zone created for `mayankr.me`.

{% lightbox /assets/images/s3/route_53.tiff.png --thumb="/assets/images/s3/route_53.tiff.png" --data="appfoundry_image_set" --title="AWS Home" --alt="AWS Home" --img-style="max-width:100%;" %}
*Route 53*

We’ll now create a new Hosted Zone by clicking on **Create Hosted Zone**. Enter your domain name on the left and click on **Create**.

{% lightbox /assets/images/s3/create_hosted_zone.tiff.png --thumb="/assets/images/s3/create_hosted_zone.tiff.png" --data="appfoundry_image_set" --title="AWS Home" --alt="AWS Home" --img-style="max-width:100%;" %}
*Create Hosted Zone*

Open the newly created hosted zone and and create a **Type A** record. Under alias target, use the bucket you created for your website. Leave all rest as it is and click **Create**.

{% lightbox /assets/images/s3/create_record.tiff.png --thumb="/assets/images/s3/create_record.tiff.png" --data="appfoundry_image_set" --title="AWS Home" --alt="AWS Home" --img-style="max-width:100%;" %}
*Create Record*

I had registered my domain using **GoDaddy**, so I’ll have to modify the domain properties in my GoDaddy account so that the AWS nameserver is used to serve my site. Make note of the 4 NS record URLs for your website.

{% lightbox /assets/images/s3/name_server_record.tiff.png --thumb="/assets/images/s3/name_server_record.tiff.png" --data="appfoundry_image_set" --title="AWS Home" --alt="AWS Home" --img-style="max-width:100%;" %}
*NS Record*

Now go over to GoDaddy or your domain provider and change the Nameservers for your domain to the new ones.

{% lightbox /assets/images/s3/godaddy_nameserver.tiff.png --thumb="/assets/images/s3/godaddy_nameserver.tiff.png" --data="appfoundry_image_set" --title="AWS Home" --alt="AWS Home" --img-style="max-width:100%;" %}
*NS Record Old*

{% lightbox /assets/images/s3/godaddy_nameserver_changed.tiff.png --thumb="/assets/images/s3/godaddy_nameserver_changed.tiff.png" --data="appfoundry_image_set" --title="AWS Home" --alt="AWS Home" --img-style="max-width:100%;" %}
*NS Record New*

Awesome!, now your website is all hosted up and accessible from the domain you purchased. You may have to wait for sometime, but soon your domain should lead you to the website hosted in your S3 container.
If you still face any issues, shoot up your questions in the comments section below! Happy website building :)