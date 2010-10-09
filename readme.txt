=== Plugin Name ===
Contributors: maraboustorkltd
Donate link: 
Tags: Twitter, ReTweets, RT, Blogger.js, json, include_rts
Requires at least: 2.0.2
Tested up to: 3.0.1
Stable tag: trunk

Currently the method being adopted by many web masters to display tweets on their web sites does not support retweets (RT).
Recently there was an update to allow the parameter include_rts=true to be inluded within the url used to retrieve the tweets
but unfortunately this is not compatible with Blogger.js as it only effects the .rss and .atom feeds (and not the .json feed).
This plugin resolves this issue and enables you to easily replace the code you might currently be using with a 1 line change 
that will add retweets to your pages.

== Description ==

Allows you to add tweets to your page including any retweets (which are not supported by the json feed provided by twitter.com)

[Demo](http://maraboustork.co.uk "Demo") 

= Credits =
* Some js used from http://twitter.com/javascripts/blogger.js
* Google API's http://ajax.googleapis.com/ajax/services/feed
* Anyone who had blogged about the problems with displaying RT's 

If you have followed the advise in one of the many blogs on the internet that instruct you how to add twitter feeds to your
site then you have probably been advised to add a <ul id="twitter_update_list"></ul> element to your code, along with two
javascript includes:

<script type="text/javascript" src="http://twitter.com/javascripts/blogger.js"></script>
<script type="text/javascript" src="http://twitter.com/statuses/user_timeline/[yourUsername].json?callback=twitterCallback2&amp;count=10"></script>

This approach retrieves your tweets using the twitter json feed which then calls a method called twitterCallback2 in 
blogger.js that then inserts your tweets into the <UL> element with the id "twitter_update_list".

The problem here is that the .json feed does not currently return retweets. When you start looking around for a solution
to this you will notice that twitter responded to this problem by introducing an include_rts parameter that can be passed
with the url, and you could be forgiven for thinking that this would work with the json feed.

Unfortunatly you would be dissapointed. The include_rts parameter is only supported by the .rss and .atom feeds 
(http://twitter.com/statuses/user_timeline/[yourUsername].atom/.rss). And to make matters worse you cannot simply replace 
the url used in the second of the script includes above to use the .atom or .rss feeds because these are not 
supported by Blogger.js.

This solution addresses this issue by using your twitter username and the number of status' you wish to retrieve to 
contruct the url to the atom feed which is then converted to json using the google api. This json feed is then used
in conjunction with a customised version of the code in blogger.js to give you the same output that you would normally
have expected but including your retweets.

This code also improves on the standard approach because it automatically allows you to support multiple feeds on the same
page because the id of the element that is populated with the tweets is passed to the procedure which retrieves them.

== Installation ==

You can either install it automatically from the WordPress admin, or do it manually:

1. Upload the whole `TwitterRSSWithRT` directory into your plugins folder(`/wp-content/plugins/`)
1. Activate the plugin through the 'Plugins' menu in WordPress

Once installed take the following steps to set it up:

1. Open /wp-content/themes/[yourTheme]/footer.php in a text editor
1. Locate the bottom of footer.php just before </body>
1. Insert the following lines:
	`<?php include_once("twitter-rss-with-rt.php"); ?>`
	`<?php get_tweets_with_rt('[your_twitter_username]', [number_of_tweets], '[control_id]'); ?>` 
1. Remove the two script lines you may already have for blogger.js and twitter.com/statuses/user_timeline/[yourUsername].json.
1. Ensure you have placed <ul id="[elementId]"></ul> where you want your feeds to appear eg <ul id="twitter_update_list"></ul>.
   If you have already followed other blogs and included the two script files mentioned above you may already have this <ul>
   element in your code.

For an example of the above inserts look at the content of test.php which is distributed with this plugin

== Frequently Asked Questions ==

= How do I change the appearance of these tweets =

You can control their appearance in the normal way using css. Below is an example that will give you the
appearance used at http://maraboustork.co.uk.

#twitter_update_list {
   float: left;
   margin-right: 20px;
   width: 280px;
   font-size: 10px;
}

#twitter_update_list li {
   border-bottom: 1px dotted #E6E6E6;
   list-style-type: none;
   padding: 10px 0px;
}

If you have renamed your <ul> element then ensure that that name is also changed in the above css.

= How can i add more than 1 feed to my page =

Simply add an additional `<?php get_tweets_with_rt('[your_twitter_username]', [number_of_tweets], '[control_id]'); ?>` 
under the existing one replacing the parameters as necessary.

== Screenshots ==

screenShot1.png

== Changelog ==

