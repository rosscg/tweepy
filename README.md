Tweepy with multiple access tokens
==================================

This fork merges the current version of Tweepy (3.5.0) with the multiple
authentication functionality from the fork by Alexandru Stancui, available here:
https://github.com/svven/tweepy

At the time of writing, there exists an issue with the Twitter API whereby
the reset time for a resource, returned either in the response header as
x-rate-limit-reset, or by the function api.rate_limit_status(), is incorrect.
This is currently handled in the master and this fork by continuing a loop
(binder.py, line 204).

Changes
-------

* **Multiple access tokens with `RateLimitHandler`** (https://github.com/svven/tweepy/blob/master/tweepy/limit.py)

> RateLimitHandler class inherits from OAuthHandler, and introduces add_access_token that can be used as follows:

> ```python
> from tweepy import RateLimitHandler
> from config import CONSUMER_KEY, CONSUMER_SECRET, ACCESS_TOKENS
>
> def get_api():
> 	auth = RateLimitHandler(CONSUMER_KEY, CONSUMER_SECRET)
> 	for key, secret in ACCESS_TOKENS:
> 		try:
> 			auth.add_access_token(key, secret)
> 		except Exception, e:
> 			print key, e
> 	print 'Token pool size: %d' % len(auth.tokens)
> 	api = API(auth,
> 		wait_on_rate_limit=True, wait_on_rate_limit_notify=True)
> 	return api
>
> api = get_api()
> ```

> Provided access tokens are used selectively based on requested resource (https://dev.twitter.com/docs/rate-limiting/1.1/limits) and current rate limits. The access token with most remaining requests per window for the specified resource is being selected and used when applying the authentication, before the actual request is performed. This pattern ensures the usage of available access tokens in a round robin fashion, exploiting to maximum the rate limits.

Installation
------------

    pip install -e git+https://github.com/rosscg/tweepy.git#egg=tweepy
