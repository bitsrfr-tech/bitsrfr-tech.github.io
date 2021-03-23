---
title: Apple TV App URL Scheme
date: 2021-03-20
---

If you are on an iOS device and you have the Apple TV app installed, [tapping this link will open Napoleon Dynamite in your Apple TV app](com.apple.tv://tv.apple.com/us/movie/napoleon-dynamite/umc.cmc.5grtztomcjab13b37ts52yme4).

That can be useful if you are trying to create a list of movies that you want to watch, and you want to be able to easily open the movies in the Apple TV app.

I very much enjoy creating movie playlists ("Movies to Watch in Spring," "Movies About Writers," "Funniest Movies," etc), so I edited some JavaScript in the Scriptable app so that I can launch a movie in the Apple TV app by tapping on it from one of my lists. [You can get the Scriptable JavaScript code on my GitHub if you are interested](https://github.com/josephkreydt/Movie_List).

The key to opening the Apple TV app to a specific movie is the URL scheme. URL Schemes, also known as X-Callback URLs, are the layer that iOS provides for allowing apps to communicate with one another. Many apps can be launched with their URL scheme. Some apps can run different tasks based on different URL scheme parameters. It is up to the app developer to build URL schemes into the app.

The Apple TV app can be launched with the URL ["com.apple.tv://"](com.apple.tv://).

Additionally, specific movies can be launched in the Apple TV app by getting the movie's URL from the Apple TV website (just search *movie name + Apple TV* in a search engine), removing the *https://* from the front of the URL, and adding *com.apple.tv://* to the front of the URL in its place.

If you enter the new URL in Safari or in an iOS Shortcut, the Apple TV app will launch to the movie page within the app.

You can also use this URL scheme to launch Apple TV movies and shows from your own iOS app. Happy hacking!