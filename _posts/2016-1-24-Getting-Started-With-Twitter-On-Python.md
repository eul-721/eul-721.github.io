---
layout: post
title: Getting Started With Twitter REST API on Python
---

Over the past few days I have been experimenting the Twitter REST API with Python. There are many wrapper libraries for Python, so I will limit this post to this [library made by sixohsix](https://github.com/sixohsix/twitter). This tutorial is targeted towards Python beginners who wants to play with Twitter API (like myself).

For starters, you will need to acquire some tokens from Twitter. This set of credentials will be used to access Twitter API. Navigate to [Twitter's App Management Page](https://apps.twitter.com/) and create a new app.

![new_app.png]({{ site.baseurl }}/images/Twitter-API/new_app.png)

Fill in the details in the form. For experiment purposes you can just use placeholders. Otherwise, try to give them meaningful names. In this example, I will be building the app `twit-bot-rec`.

Once you are done with that, click it to see its details. The only tab that matters to us at this point is **Keys and Access Tokens**. Click on it and you should see something similar to below.

![app_details]({{ site.baseurl }}/images/Twitter-API/app_details.png)

Sensitive info on this page has been censored out for confidentiality. Having just created the app, you should only see the top section under **Application Settings**. Take note of `Consumer Key (API Key)` and `Consumer Secret (API Secret)`; we will need these later.

At the bottom, you should see a seperate *Your Access Token* category. You will need to generate a this before you can start using the API. Once you are done with that, keep note of the secrets provided: particularly `Access Token` and `Access Token Secret`.

Let's start some coding. First, create a directory where you will store your source files. I have stored mine under `~/dev/py-twit`. Navigate to that directory in the terminal, then execute the following.

```bash
pip install twitter
```

This will install the twitter API tools. Note that, this pip install is global, so all your other python scripts can have access to this `twitter` library. If this is not what you want, you may want to look into `Virtualenv` and create an isolated Python environment.

Start with a `main.py` file. In this file, go ahead and import the library we just fetched.

```python
import twitter
import json
```

I have also imported the json library to perform data manipulation later.

Remember the secret tokens from earlier? We will need to use them now. Create your `Twitter Api` object with the following code.

```python
#Replace everything encapsulated with <> brackets with appropriate keys

application_token = <Your Application Token>
app_secret = <Your Application Token Secret>
consumer_key = <Your Consumer Key>
consumer_key_secret = <Your Consumer Key Secret>

tApi = Twitter(auth=OAuth(
    application_token,
    app_secret,
    consumer_key,
    consumer_key_secret
));
```

If you attend to include more settings for your app, I advise you to extract these keys out of `main.py` into a `ini` file, which you will then read with `configparser` library. For security purposes, you may even want to read these keys from the environment variables instead.

Now that your API is registered, we can start making calls. Starting with the most basic...

```python
print(tApi.statuses.home_timeline())
```

Compile your script and you should see alot of text that kinda resemble JSON. In fact, all API calls ('least the one I know) return JSON responses. You will n'eed to wrap this result in a `json_dump`:

```python
print(json.dumps( tApi.statuses.home_timeline(),indent=4, ensure_ascii=FALSE)
```

Compile again. You should see a prettier JSON formatted string, but there's still too many to comprehend. You can add a `count` parameter to limit this search result.

```python
tApi.statuses.home_timeline(count=5)
```
You should now see only 5 JSON objects. These are the 5 newest statuses fetched from your twitter home timeline.

A similar call can be made to a *specific user's* timeline, by swapping `home_timeline` to `user_timeline`
```python
tApi.statuses.user_timeline(screen_name="twitter")
```

This will fetch `@Twitter`'s timeline.

Let's try making a post to your home_timeline. Make the call
```python
tApi.statuses.update(
    status="Getting started with Twitter from Python!")
```

Go check your Twitter page and you should see that you just made a new post!

By this point, you should have a basic understanding of how this wrapper library works. If you wish to explore other available methods, visit the [library home page](http://mike.verdone.ca/twitter/) as well as [Twitter' REST API documentation](https://dev.twitter.com/rest/public)

Keep in mind that **any non-streaming API** calls will have a rate-limit. Basically, Twitter puts a limit on how many times a set of token can use a certain API call, and returns a `HTTP 429` error after the limit has been reached. The library does provide a way to [retry a call](https://github.com/sixohsix/twitter#retrying-after-reaching-the-api-rate-limit), however.

I plan to explore with Twitter's streaming API in the next few days. Expect a relevent post to be uploaded.
