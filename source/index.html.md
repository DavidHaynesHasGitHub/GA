---
title: Google Analytics Setup

language_tabs: # must be one of https://git.io/vQNgJ
  - html

toc_footers:
  - <a href='https://api.zaui.io/signup'>Sign Up for a Developer Key</a>
  - <a href='#'>See our Web API Documentation</a>
  - <a href='#'>See our IO API Documentation</a>
  - <a href='#'>See our Webhook and Remote Target Documentation</a>

search: true
---

# Google Analytics & Zaui

## Introduction


Google Analytics provides deep and meaningful insights into how your customers are interacting with your website. How they found you, what they do when they get there, what the user flow is and what areas of the sales funnel they fall off are some of the many pieces of information that can be critical to your business. The following is basic instructions on how to ensure your Google Analytics (‘GA’) is configured properly. This is by no means a comprehensive list of everything you should do with Google Analytics and further personal research and effort in configuring your GA Filters, Goals and Funnels is encouraged.

# Google Analytics

> Google Analytic Script with Cross Domain Linking and Enhanced Ecommerce activated

```html
<!-- Google Analytics -->
<script>
(function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
(i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
})(window,document,'script','https://www.google-analytics.com/analytics.js','ga');
ga('create', 'UA-XXXXX-Y', 'auto', {allowLinker: true});//replace UA-XXXXX-Y with your GA tracking code
ga('require', 'linker'); //enables cross domain tracking
ga('require', 'ec'); //enables enhances ecommerce features
ga('linker:autoLink', ['source.com', 'destination.com']); //sets the two domains for linking replace source with your domain and destination with the domain for your zaui portal
ga('send', 'pageview'); //sends data to google
</script>
<!-- End Google Analytics -->
```

GA uses a small piece of script that you include on your website than enables powerful analysis of user interactions on your domain. As part of your Zaui membership we configure it for you on our portal taking advantage of GA latest technology. To get the best results it is best if on your website on each page also include the script tag which you can see at this link.

# Setup of GA

Now GA captures an overwhelming amount of data that can be a big battle to be able to sift through and get meaningful insights out of, that is why it is key to create 3 separate “Views”. Views in GA are all the data coming in that has been run through the filters you have applied. Unfortunately once you create a filter, any data that has been filtered out is lost forever within that View. This also means that it is important in the order that you create each filter within the Views.

At minimum you should create three different Views.

## Raw View
This View is completely unfiltered and just collects all the data that GA script on your site is collecting. This way you will never not have the data you want for your business.

## Test View
This is the view where you get to experiment with different configurations of filters without disrupting your main View (“Master”) or your Raw Data View. Since filters don’t just hide data but remove it from the View entirely it is a good idea to test configurations before applying them to your main view.

## Master View
This is the main view for your analytics. Where all your filters will be used to make sure the most relevant information is front and centre.


# Enhanced Ecommerce

As part of a new effort on Google they have created a new element to GA called Enhanced Ecommerce (‘EEC’). This feature tracks user information not just from cart information but it is able to follow users through the sales funnel. With the ability to create Goals and Funnels you can see with EEC what products are getting the most attention, when they are leaving your site, how much each area of your site is worth, and much more.

We have already included EEC as part of our script provided in Zaui and if you include the script as linked above on your site it will ensure the most accurate tracking information that GA can provide with cross site linking of your website to the Zaui portal.


## Enabling EEC in Google Analytics

Once you have your script added there is a quick configuration step you need to take within your GA dashboard to enable eCommerce and EEC tracking

1. Login to your GA account
2. On the bottom left click Admin
3. On the far right under Views click Ecommerce Settings
4. Enable all three settings Ecommerce, Related Products and Enhanced Ecommerce Reporting.
5. Repeat for all three Views you have created


# Filters

Filters are created to help break down the data into smaller groups. They can be either Account filters or unique to the View you are working in but you can import filters from other Views. The trick with filters however is to keep them General and not to over filter your data.

You create Filters under:

Admin -> Account -> All Filters
<br>
Or
<br>
Admin -> View -> Filters.

As mentioned earlier, filters are only applicable to future data, not historical and are processed in order so make sure that they are in the proper order for your needs. This is by no means a comprehensive list of filters, just some suggestions.

A few basic filters you should consider are:

## Exclude Internal IP address
To prevent GA from tracking when you or staff are on your site you want to Exclude Internal IP addresses from your View.

To do so:
<br>
1. Admin -> Account -> All Filters
<br>
2. Click + Add Filter
<br>
3. In the Filter Name Field name it something like: “Exclude Internal IP”
<br>
4. Click Filter Type “Predefined”
<br>
5. In the three dropdown boxes that appear select
<br>
6. Exclude
<br>
7. Traffic from the IP addresses
<br>
8. That are equal to
<br>
9. In a new tab go to Google.com and search “Whats my IP Address”
<br>
10. Copy your Public IP Address and paste it into the Filter Pattern Field
<br>
11. Select which views that you want to apply this filter to (“Test View”)
<br>
12. Click Save to create the filter

## Subdirectory Filter
Does your website have a blog?
<br>
Does the url look like www.yourwebsite.com/blog/2018/may/13

What this will do for GA is that because the URL is unique you will get a different tracking result for each blog post even if you only really care about general performance for your blog or other part of your website. So to fix this you need to create a filter that captures all /blog/ URIs into one “bucket”.

1. Admin -> Account -> All Filters
<br>
2. Click + Add Filter
<br>
3. Filter Name “Include only Blog Traffic”
<br>
4. Filter Type click “Predefined”
<br>
5. Include Only
<br>
6. Traffic in Subdirectories
<br>
7. That Begin With
<br>
8. In the Subdirectory field add /blog/
<br>
9. Select which views that you want to apply this filter to (“Test View”)
<br>
10. Click Save to create the filter

There are many more filters you can build but it isn’t necessary to have lots or any at all to make sure you are getting the most out of your GA. With filters less is more.

## Bot & Spider Filtering

Bots and spiders are many times beneficial to your website as they are regularly used by search engines and other platforms to crawl your website to make sure it is indexed properly. That being said, they aren’t customers so you probably don’t want them in your reports.

To filter bots:
<br>
1. Admin -> View
<br>
2. Click the checkbox Bot Filtering
<br>
3. Save the changes

Filter Order
As filters are applied in the order that they are added and so it may be necessary to change the order in which they are triggered.

To do so:
<br>
1. Admin -> View -> Filter
<br>
2. Click “Assign Filter Order”
<br>
3. Adjust position with the “move up” or “move down” buttons
<br>
4. Click Save

*  As a rule when you have 2 or more filters for a single view pay extra special attention to the order that they are in.


### Suppressing HTTP Response codes

Certain technologies either don’t handle or can’t handle HTTP response codes. More specifically, they
don’t handle any non 200 HTTP status codes. For this result our API provides provisioning for this. In
this case, your implementation can send through the suppress_response_codes with the value true .
This will always request a status code of 200 in the response header. The original, and true, status code
is then available within the response body.

### Implementing Exponential Back-off

Exponential back-off is the process of a client periodically retrying a failed request over an increasing amount of time. *It is a standard error handling strategy for network applications.* Besides being “required”, using exponential back-off increases the efficiency of bandwidth usage, reduces the number of requests required to get successful response, and maximizes the throughput of requests in concurrent environments.
The flow of implementing a simple exponential back-off is as follows:

1.  Make a request to the API
2.  Receive the response, check for error that **has a retry-able error code (such as 503)**
3.  Wait 1s + random_number_milliseconds seconds
4.  Retry request
5.  Receive the response, check for error that **has a retry-able error code (such as 503)**
6.  Wait 2s + random_number_milliseconds seconds
7.  Retry request
8.  Receive the response, check for error that **has a retry-able error code (such as 503)**
9.  Wait   seconds
10. Retry request
11. Receive the response, check for error that **has a retry-able error code (such as 503)**
12. Wait 8s + random_number_milliseconds seconds
13. Retry request
14. Receive the response, check for error that **has a retry-able error code (such as 503)**
15. Wait 16s + random_number_milliseconds seconds
16. Retry request
17. If you still get an error, stop and log the error

**Note:** _random_number_milliseconds MUST be redefined after each “Wait”_

In the above flow, random_number_milliseconds is a random number of milliseconds less than or equal to 1000. This is necessary to avoid certain lock errors in some concurrent implementations.

**Note:** _the wait is always (2^n) + random_number_milliseconds, where n is a mono-tonically increasing integer initially defined as 0. N is incremented by 1 for each iteration (each request)_

The algorithm is set to terminate when n == 5. This ceiling is in place to prevent clients from retrying infinitely, and results in a total delay of 32 seconds before a deemed “unrecoverable error”.

# Goals

The misconception with Goals is that they are for tracking pageviews and visitors to see how successful your site is and that is wrong. Unlike filters, Goals are things that get triggered when a user reaches a certain point on your site. This of them like checkpoints in a race. Just like finishing a race can be hard, it can be equally hard to get a customer from the start (your landing page) to the end (processed cart) and using Goals can provide insight into where you are losing them on their way to that end.

GA has 5 different types of Goals you can configure:


## Destination

A user has reached a certain page on your site.


## Duration

A user has been on your site for a set amount of time. You can use past GA data to see how long successful conversions spent on your site to set this goal. As an example, the majority of people needed at least 6min27sec from landing page to cart so you create a goal saying you want to know how many people hit 6min30sec.

## Pages per Session

How many pages does a user need to get from intro to your site to checkout? Are your products shown on a single page or separate? Give some room for browsing but keep it reasonable.

## Events

Events are the most useful and in turn hardest to configure. It measures how a user interacts with the site after the page loads so that means you will have to make some changes to how you track your site.

## Smart Goals

Smart Goals are for people using Google Adwords who have yet to set up goal tracking. Smart Goals uses the data from thousands of other sites to determine whether there was user engagement.

## Setup

Goals are a key asset in your GA toolbox as they help classify each session into two different groups: Converted & Non-Converted. For example, anybody can go to your landing page and that is good information to know, but it would be better to know how many people went to your landing page and watched the promo video you have there for your Zipline tours.

Goals help answer the question of “During this session, what did specific actions did the user make at least once?”

Setting up Goals is similar to filters.

To set up go to:
<br>
1. Admin -> View -> Goal
<br>
2. Click + New Goal
<br>
3. Follow the template instructions for Goals or create a Custom goal

### For Zaui a key goal to have is the following:

1. Goal Name:  Zaui Web Booking Engine
2. Goal Type:  URL Destination
3. Under Goal Details
4. Match Type:  Regular Expression Match
5. Goal URL:  (page=Confirmation)
6. Goal Value: 0.0
7. Toggle Goal Funnel
8. Name: Zaui Web Engine - Home Page
^/modules/webBooking/index\.php$|(^/modules/webBooking/$)
9. Zaui Web Engine - Activity/Product Page
(action=Details)
10. Zaui Web Engine - Payment Page
(action=Contents)  

# Funnel
A goal funnel is a path for the user to take to reach the final goal for example:

Landing page -> Testimonials -> Products Page -> Cart -> Confirmation

Funnels are used to track how far along this path that they get so that you can look into why we are losing them at Testimonials or Products Page. Use this tool to help dig into your analysis by visualizing yourself how you want the user to view your site to get the most sales and then build variations of those Goal Funnels. Keep in mind that you can have max 20 Goals per View, which is more than what you should need but it is always good to remember, less is more.

# UTM Url Codes
There are many reasons why GA can’t track the user especially where they came from. For example if the user comes from a bookmarked link, email link inside a desktop app, going from a HTTPS domain to a HTTP domain, ad blockers preventing javascript and more. Google then declares all of this “Direct Traffic”

The best way to measure campaigns and ensure that all of your traffic is getting triggered by GA is to use UTM Url Codes. Google has created a tool to help build your URLs that adds tracking code directly into the url and so when the user clicks that link GA knows what it is. The best part you can have multiple different ones and name them uniquely. This means that when you have a email campaign or banner ads or facebook promotion you can have unique links so you can see what the most effective tool is.

<a href="https://ga-dev-tools.appspot.com/campaign-url-builder/">Go to Google URL Builder Here</a>

All of your links for sharing should have a UTM code for best accuracy.

# Common issue with GA & Zaui

## Higher Revenue in GA vs Zaui
One big thing we hear from people regarding GA vs Zaui is that their revenue is often higher in GA than it is in Zaui. This is due to the fact that when there is a cancellation in Zaui the data isn’t sent back to GA to adjust the revenue column.

## Lower Revenue in GA vs Zaui

It’s not uncommon for some transactions to be missing from Google Analytics.

In general, analytics should track most transactions.  Some potential reasons for these can include:

1. lack of Javascript support on customers browser, or mobile device
<br>
2. customers abandoning their transaction, after credit card payment, but before the system has completed sending the transaction information to Google.

If you are having further issues with your Google Analytics Account, <a href="https://blog.kissmetrics.com/google-analytics-data-errors/">KissMetric</a> has a great guide on the top 29 issues, causes and fixes for them.

# Further Learning
This is just a scratch in the ground into the deep deep world that Google Analytics can be and is just designed to help get a basic setup for GA going for you in combination with Zaui.

## Recommendations

<a href="https://analytics.google.com/analytics/academy/course/6">Google Analytics for Beginners</a> - Google Analytics Academy Course (1 hour)
<br>
<a href="https://analytics.google.com/analytics/academy/course/7">Advanced Google Analytics</a> - Google Analytics Academy Course (1 hour)
<br>
<a href="https://analytics.google.com/analytics/academy/course/3">eCommerce Analytics</a> - Google Analytics Academy Course (1 hour)
<br>
<a href="https://www.lunametrics.com/services/google-analytics/">Luna Metrics</a> - An Analytics and SEO training and consulting company with excellent blog articles on Google Analytics

# Bibliography

1. “Advanced Google Analytics.” Google, Google, analytics.google.com/analytics/academy/course/7.
2. Agius, Aaron. “A Step-by-Step Guide to Using Google Analytics' Enhanced Ecommerce Features.” A Deep Dive Into Facebook Advertising - Learn How To Make It Work For Your Business!, blog.kissmetrics.com/google-analytics-enhanced-ecommerce-features/.
3. “Ecommerce Analytics.” Google, Google, analytics.google.com/analytics/academy/course/3.
4. “Google Analytics for Beginners.” Google, Google, analytics.google.com/analytics/academy/course/6.
5. “Google Tag Manager Fundamentals.” Google, Google, analytics.google.com/analytics/academy/course/5.
6. Norris, Tyler. “Using Events as Goals in Google Analytics.” LunaMetrics, 26 Jan. 2018, www.lunametrics.com/blog/2018/01/16/events-goals-google-analytics/.
7. Patel, Neil. “The Ultimate Guide to Google Analytics Resources for 2018.” A Deep Dive Into Facebook Advertising - Learn How To Make It Work For Your Business!, blog.kissmetrics.com/google-analytics-resources/.
8. Sharif, Sayf. “Goals & Funnels in Google Analytics: Confusion and Workarounds.” LunaMetrics, 29 Jan. 2015, www.lunametrics.com/blog/2014/10/21/goals-funnels-confusion-google-analytics/.



# Legal

## LEGAL STATEMENT

This guide, in addition, to the software described within, is under the copyright owned by Zaui Software Limited and subject to license. This guide and any included software, whole or in part, cannot be published, downloaded, or reproduced, transmitted, transferred or combined with any other material or be used for any other purpose without written permission from Zaui Software Limited.


## NOTICE OF NON-LIABILITY

Every effort has been made to ensure the accuracy of information published in this guide. However, Zaui Software cannot accept any responsibility for any errors, inaccuracies, or omission that may or may not be published in this guide.

Zaui Software is providing the information in this guide to you “AS-IS” with all faults. Zaui software makes no warranties of any kind (whether express, implied or statutory) with respect to the information contained herein. Zaui Software assumes no liability for damages (whether direct or indirect), caused by errors or omissions, or resulting from the use of this document or the information contained in this document or resulting from the application or use of the product or service described herein. Zaui Software Limited reserves the right to make changes to any information herein within prior notice or consent.
