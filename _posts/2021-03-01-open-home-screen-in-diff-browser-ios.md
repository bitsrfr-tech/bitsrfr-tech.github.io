---
title: Open a Home Screen Bookmark in a Different Browser (iOS)
date: 2021-03-01
---

You can open a website from a Home Screen bookmark in any iOS browser you want. You just have to follow iOS's protocols.

# The Problem: Your Home Screen Bookmarks Open in Safari
You want to be able to open a website from an icon on your iPhone or iPad Home screen. You open the website in Safari, open the *Share* menu, and tap "Add to Home Screen." Now you have an icon on your Home Screen that you can click to open the site in Safari. Instead, you want the website to open in Brave.

# The Solution: Use an X-Callback URL Shortcut
To make a bookmark open in a different browser, find the X-Callback URL for the web browser you want to use. Then, create a Shortcut that will open the X-Callback URL which will launch your web browser with the link you choose. Finally, add the iOS Shortcut to your iPhone's or iPad's Home Screen. Here are step by step instructions, with screenshots from iOS 14, to show you exactly how to do it.

### To open a Home Screen bookmark in a browser other than Safari, you will need two things:
- The browser's X-Callback URL (a.k.a. URL scheme) for opening web links (common ones are listed below)
- The iOS Shortcuts app, or another app that handles X-Callback URLs such as [Scriptable](https://docs.scriptable.app/callbackurl/) or [Pythonista](https://omz-software.com/pythonista/docs/ios/urlscheme.html)

## Create the X-Callback URL
1. Find your web browser's X-Callback URL scheme for opening web links (see list below)
2. To the end of the URL scheme, add the URL of the website you want to open

## Open the X-Callback URL with Shortcuts
**1. Open the Shortcuts app and create a new Shortcut**
![iOS Shortcuts](/images/Diff_Browser_1.png)

**2. Find the *Open X-Callback URL* action, and add it to your Shortcut**
![iOS Shortcuts Open X-Callback URL action](/images/Diff_Browser_2.png)

**3. To the action, add the X-Callback URL you created**
![iOS Shortcuts X-Callback URL](/images/Diff_Browser_3.png)

**4. Press the *play* button to run the Shortcut**
![Run iOS Shortcut](/images/Diff_Browser_4.png)

## Add the Shortcut to Your Home Screen
**1. On your Shortcut page, tap the circle with three dots**
![Save iOS Shortcut](/images/Diff_Browser_5.png)

**2. Enter a name for your Shortcut, and tap *Add to Home Screen***
![Add iOS Shortcut to Home Screen](/images/Diff_Browser_6.png)

**3. Select a Home Screen name and icon, and tap *Add***
![Add iOS Shortcut to Home Screen 2](/images/Diff_Browser_7.png)

**4. On the *Details* page, tap *Done***

*Note: On iOS 14.4 (and possibly other versions), some users experience a bug in which the Shortcut fails to launch when the user taps its Home Screen icon. Hopefully Apple and/or the app developers will fix the bug soon. Until then, you can work around the issue by opening the Shortcuts app and running the Shortcut from there.*

----

# Common Web Browser URL Schemes for Opening Web Links
Replace *WebsiteURL* with the URL you want to open. Some browsers accept the URL if you add "https" to it, and some require just the second level and top level of the domain (e.g. bitsrfr.com instead of /)

### Safari
<code>
https://WebsiteURL<br>
https://bitsrfr.com
</code>

### Brave
<code>
brave://open-url?url=WebsiteURL<br>
brave://open-url?url=https://bitsrfr.com
</code>

### Duck Duck Go
<code>
ddgQuickLink://WebsiteURL<br>
ddgQuickLink://bitsrfr.com
</code>

### Firefox
<code>
firefox://open-url?url=WebsiteURL<br>
firefox://open-url?url=bitsrfr.com
</code>

### Firefox Focus
<code>
firefox-focus://open-url?url=WebsiteURL<br>
firefox-focus://open-url?url=bitsrfr.com
</code>

### Google Chrome
<code>
googlechrome://WebsiteURL<br>
googlechrome://bitsrfr.com
</code>

----

In summary, if you want to open a Home Screen website shortcut in a browser other than Safari, find the correct X-Callback URL, create a Shortcut to will open the X-Callback URL, and add the Shortcut to your Home Screen.

Happy customizing!

## Resources
- Decent list of app URL schemes: [https://ios.gadgethacks.com/how-to/always-updated-list-ios-app-url-scheme-names-paths-for-shortcuts-0184033/](https://ios.gadgethacks.com/how-to/always-updated-list-ios-app-url-scheme-names-paths-for-shortcuts-0184033/)
- Decent list of app URL schemes: [https://github.com/bhagyas/app-urls](https://github.com/bhagyas/app-urls)
- Decent list of app URL schemes: [https://app-talk.com/#working-copy](https://app-talk.com/#working-copy)
- Scriptable URL scheme docs: [https://docs.scriptable.app/callbackurl/](https://docs.scriptable.app/callbackurl/)
- Pythonista URL scheme docs: [https://omz-software.com/pythonista/docs/ios/urlscheme.html](https://omz-software.com/pythonista/docs/ios/urlscheme.html)
- Apple docs on using X-Callback URLs with Shortcuts: [https://support.apple.com/guide/shortcuts/use-x-callback-url-apdcd7f20a6f/ios](https://support.apple.com/guide/shortcuts/use-x-callback-url-apdcd7f20a6f/ios)
- Apple intro to URL schemes: [https://support.apple.com/guide/shortcuts/intro-to-url-schemes-apd621a1ad7a/ios](https://support.apple.com/guide/shortcuts/intro-to-url-schemes-apd621a1ad7a/ios)
