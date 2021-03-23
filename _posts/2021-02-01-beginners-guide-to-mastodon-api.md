---
title: A Beginner's Guide to the Mastodon API - Post a Status Update
date: 2021-02-01
---

If you want to program a Mastodon bot or automate your Mastodon posting, or just want to post from the command line, you've found the right corner of the internet.

I am going to show you how to connect to the Mastodon API with cURL and Python.

If you are reading this article, I assume a couple things about you:
- You know what Mastodon is
- You have some basic programming knowledge

### Find your access token
Once you have a Mastodon account, you need your account's *access token*. To get that:
1. Sign into your Mastodon account
2. In the bottom left corner of your home page, click the *Developers* link
3. On the *Your applications* page, click the blue *NEW APPLICATION* button
4. Give your application a name, and decide what kind of access you want to have when you connect to your account via the Mastodon API (read, write, and follow is the default). You can always change this later
5. At the bottom of the page, click the blue *SUBMIT* button
6. You will be directed back to the *Your applications* page, but now you should see your application name. Click that
7. In your application's page, there are three tokens. For this tutorial, you need the *Your access token* one
    - Note: if your access token is ever compromised, you can click regenerate, and your old access token will stop working, and you'll be shown a new one

## Post a status update via cURL
cURL is a beautiful command line tool for send HTTP requests, and getting responses. We will use HTTP requests to communicate with Mastodon's API.

You probably already have cURL installed on your computer. You can confirm by opening a terminal, and entering <code>curl -V</code>.

If you don't get an error, you're good. I am using cURL version 7.68.0, but whatever version you have should work just fine.

You are going to need a couple pieces of information to communicate with the Mastodon API:
- Your Mastodon server's address
- Your Mastodon account's access token

Your **Mastodon server's address** is the address you go to in a web browser to sign into Mastodon. Mine is *mstdn.social*. Some other common ones are *mastodon.social*, *switter.at*, *mastodon.xyz*, and *alive.bar*.

Your **Mastodon account's access token** is the access token I showed you in step 7 above.

With those, you are ready to make your first Mastodon API request. Let's post a status update! Here is what mine looks like (except, that's no longer my access token because you're supposed to keep that secure).

<code>curl https://mstdn.social/api/v1/statuses -H 'Authorization: Bearer 4-Y3nDFgrz8hV7WmbRqDAV52TiAnsQ8jeSvfbYN0g30' -F 'status=Look Mom, I can update my status via Mastodon's API!'</code>

#### Let's break that request down
The first part is <code>curl</code>. That just tells the operating system to run the cURL program.

Next is <code>https://mstdn.social/api/v1/statuses</code>. That has a couple parts:
- *https://mstdn.social/* is my Mastodon server's address. Replace that with your own Mastodon server's address
- *api/v1/status* is the piece of the request that tells Mastodon you want to make a status update via version 1 of the API

After the URL, we have <code>-H</code>. That is how we tell cURL that the next chunk of text belongs in the HTTP request's header.

The chunk of text that goes in the header, <code>'Authorization: Bearer 4-Y3nDFgrz8hV7WmbRqDAV52TiAnsQ8jeSvfbYN0g30'</code> is a specification of the access token type ('Authorization: Bearer') followed by a single space, and then the Mastodon account's access token. Replace that with your own Mastodon account's access token.

Then, we have <code>-F</code> which tells cURL that the next chunk of text belongs in the HTTP request's form parameters.

The chunk of text that goes in the form parameters, <code>'status=testing the postautomaton bot!'</code> is the form parameter field ('status') followed by an equal sign, and then text of the status update. Replace <code>Look Mom, I can update my status via Mastodon's API!</code> with whatever you want your status update to say.

Hit *Enter*, and go check your Mastodon profile. The status update should be there!

## Post a status update via Python
Python is a classic scripting language. If you don't have it installed on your computer, you can grab it at [python.org/downloads/](https://www.python.org/downloads/).

You can check whether Python is installed, and which version, by opening a terminal, and entering <code>python -V</code>.

I am using Python version 3.8.5 for this example. Any version of Python 3 should work.

Same as the cURL request, for the Python request, you are going to need:
- Your Mastodon server's address
- Your Mastodon account's access token

Your **Mastodon server's address** is the address you go to in a web browser to sign into Mastodon. Mine is *mstdn.social*. Some other common ones are *mastodon.social*, *switter.at*, *mastodon.xyz*, and *alive.bar*.

Your **Mastodon account's access token** is the access token I showed you in step 7 above.

Here is the Python code you will run to post a status update to Mastodon. Although, yours will be slightly different because your Mastodon server and access token are probably not the same as mine.

<code>import requests

url = 'https://mstdn.social/api/v1/statuses'
auth = {'Authorization': 'Bearer 4-Y3nDFgrz8hV7WmbRqDAV52TiAnsQ8jeSvfbYN0g30'}
params = {'status': 'Mastodon API request from Pythong!'}

r = requests.post(url, data=params, headers=auth)

print(r)</code>

#### I will explain that code
The first line, <code>import requests</code> tells Python to use the *Requests* library. The *Requests* library is kind of like cURL. It is used for making HTTP and other internet requests.

On the next line, <code>url = 'https://mstdn.social/api/v1/statuses'</code>, we are putting our API request URL into a variable called *url*. That URL gets us to the Mastodon server, and tells it that we want to use version 1 of the Mastodon API to post a status update. Make sure to replace <code>https://mstdn.social</code> with your own Mastodon server's address.

A line down from that, we add our authorization token to a dictionary type variable called *auth*. For your request, <code>auth = {'Authorization': 'Bearer </code> will stay the same, but replace <code>4-Y3nDFgrz8hV7WmbRqDAV52TiAnsQ8jeSvfbYN0g30</code> with your own Mastodon account's access token.

Next, we put our request parameter(s) into a dictionary type variable called *params*. For your request, <code>params = {'status': '</code> will stay the same, but replace <code>Mastodon API request from Pythong!</code> with whatever you want your status update to say.

Run your Python script, and in no time at all, your status update will be posted to your Mastodon account.

## One more piece of useful information
That should give you a good starting point. One more thing that will come in handy is Mastodon's API documentation: [docs.joinmastodon.org/client/intro/](https://docs.joinmastodon.org/client/intro/).

If you want to jump right to the juicy part, [go here](https://docs.joinmastodon.org/methods/statuses/).

Mastodon's documentation is in a nice standard format. It tells you what type of request is required, the URL, the headers, and the form parameters for whichever task you want to complete.

Now you are well on your way to programming a Mastodon bot, automating your Mastodon activity, and social networking via the command line. Happy hacking!