twitter-api-php-cached
======================
Simple PHP Wrapper for Twitter API v1.1 calls w/ caching

This is a fork of the [twitter-api-php](https://github.com/J7mbo/twitter-api-php) from [J7mbo](https://github.com/J7mbo/)

This fork includes the addition of a flat file cache for requests made to Twitter. The goal of this api is to moderate the Twitter calls to avoid exeeding the request rate-limit.

An added benefit to this library is that it acts as a proxy to the Twitter API so that you can access the API using Javascript or mobile applications while allowing PHP to negotiate OAuth.

How To Use
------
There are additional instructions below for configuring the original version of this library that still apply.
#### Set the cache file path ####

    $tweet_file = 'TweetCache.json';
    
#### Set the cache duration in minutes ####

    $cache_time = 2;

#### Set access tokens ####

    $settings = array(
        'oauth_access_token' => "YOUR_OAUTH_ACCESS_TOKEN",
        'oauth_access_token_secret' => "YOUR_OAUTH_ACCESS_TOKEN_SECRET",
        'consumer_key' => "YOUR_CONSUMER_KEY",
        'consumer_secret' => "YOUR_CONSUMER_SECRET"
    );
    
From here, calling the index.php file will return the latest 30 tweets from your own timeline as specified in the UpdateTimeline() function.

Expanding and Modifying
------
If you want to call a different Twitter API you can simply modify the UpdateTimeline() function to suite.

**If you want to call more than 1 Twitter API function, you'll need to do a little more work.**

- Create a cache file variable for each call you'd like to cache
- Modify the ReadFromCache() and UpdateCache() functions to accept a cache file variable and use that value when reading/writing to cache
- Create additional API call functions along the lines of UpdateTimeline()
- Modify index.php to check for a query parameter indicating what function is being requested


twitter-api-php (original)
======================
Simple PHP Wrapper for Twitter API v1.1 calls

**[Changelog](https://github.com/J7mbo/twitter-api-php/wiki/Changelog)** ||
**[Examples](https://github.com/J7mbo/twitter-api-php/wiki/Twitter-API-PHP-Wiki)** ||
**[Wiki](https://github.com/J7mbo/twitter-api-php/wiki)** ||
**[Donate](https://github.com/J7mbo/twitter-api-php/wiki/Donate)**

[Instructions in StackOverflow post here](http://stackoverflow.com/questions/12916539/simplest-php-example-retrieving-user-timeline-with-twitter-api-version-1-1/15314662#15314662) with examples. This post shows you how to get your tokens and more. 
If you found it useful, please upvote / leave a comment! :)

The aim of this class is simple. You need to:

- Include the class in your PHP code
- [Create a twitter app on the twitter developer site](https://dev.twitter.com/apps/)
- Enable read/write access for your twitter app
- Grab your access tokens from the twitter developer site
- [Choose a twitter API URL to make the request to](https://dev.twitter.com/docs/api/1.1/)
- Choose either GET / POST (depending on the request) 
- Choose the fields you want to send with the request (example: `array('screen_name' => 'usernameToBlock')`)

You really can't get much simpler than that. Here is an example of how to use the class for a POST request to block a user, and at the bottom is an example of a GET request.

Installation
------------

**Normally:** If you *don't* use composer, don't worry - just include TwitterAPIExchange.php in your application. 

**Via Composer:** If you *do* use composer, here's what you add to your composer.json file to have TwitterAPIExchange.php automatically imported into your vendor's folder:

    {
        "require": {
            "j7mbo/twitter-api-php": "dev-master"
        }
    }

Of course, you'll then need to run `php composer.phar update`.

How To Use
------
#### Include the class file ####

    require_once('TwitterAPIExchange.php');

#### Set access tokens ####

    $settings = array(
        'oauth_access_token' => "YOUR_OAUTH_ACCESS_TOKEN",
        'oauth_access_token_secret' => "YOUR_OAUTH_ACCESS_TOKEN_SECRET",
        'consumer_key' => "YOUR_CONSUMER_KEY",
        'consumer_secret' => "YOUR_CONSUMER_SECRET"
    );

#### Choose URL and Request Method ####

    $url = 'https://api.twitter.com/1.1/blocks/create.json';
    $requestMethod = 'POST';

#### Choose POST fields ####

    $postfields = array(
        'screen_name' => 'usernameToBlock', 
        'skip_status' => '1'
    );

#### Perform the request! ####

    $twitter = new TwitterAPIExchange($settings);
    echo $twitter->buildOauth($url, $requestMethod)
                 ->setPostfields($postfields)
                 ->performRequest();

GET Request Example
----------------

Set the GET field BEFORE calling buildOauth(); and everything else is the same:

    $url = 'https://api.twitter.com/1.1/followers/ids.json';
    $getfield = '?screen_name=J7mbo';
    $requestMethod = 'GET';

    $twitter = new TwitterAPIExchange($settings);
    echo $twitter->setGetfield($getfield)
                 ->buildOauth($url, $requestMethod)
                 ->performRequest();

That is it! Really simple, works great with the 1.1 API. Thanks to @lackovic10 and @rivers on SO!
