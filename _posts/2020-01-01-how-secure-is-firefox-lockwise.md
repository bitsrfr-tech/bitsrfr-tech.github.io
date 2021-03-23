---
title: How Secure is Firefox Lockwise Password Manager?
date: 2020-01-01
---

Please note: this article was written in January of 2020. I believe it is still worth the read because it offers some good points to consider in choosing a password manager. However, the statements about how Firefox Lockwise works may be different now.

# How secure is Firefox Lockwise password manager?
After hearing that LastPass was acquired by a private equity firm[1], I decided it might be a good time to start looking at other password managers. I remembered hearing about Firefox Lockwise. I’m a fan of Mozilla’s focus on user privacy, so I started researching and testing Lockwise.

Note: I am not a security analyst. I am a tech savvy user who did some research and testing and figured I would share what I found. It doesn’t look like security researches have done much testing on Lockwise yet. However, Lockwise is open-source[2], so the code is available for analysis in the future.

In my testing, I found that using Lockwise with its default settings is significantly less secure than other popular password managers like LastPass and Bitwarden. However, with some setting adjustments and a workaround, it can be a decent alternative with many new features in the works.

## Security Considerations
* Lockwise uses the same encryption methods as other popular password managers, LastPass and Bitwarden[3][4][5].

* Passwords are encrypted before being sent to cloud storage, so Mozilla cannot see your passwords[3].

* Lockwise has the ability to configure a master password that is separate from your Firefox account password[6]. However, the master password is only for encrypting the passwords locally on your PC[7]. This can leave quite a security hole. I will discuss this later in this article.

* Firefox supports two-factor authentication[8], so logging into Lockwise can require two-factor authentication, but unlocking Lockwise on a mobile device is a different story as that is tied to the unlocking of your device rather than your Firefox account.

* Lockwise cannot be configured to prompt the user for his/her Firefox account password every time it is used. Other popular password managers have this important security feature. There is an open issue on Lockwise’s GitHub to get this fixed[12].

* Lockwise is open source[2]. [Here is why you might want an open source password manager[13]](https://www.reddit.com/r/security/comments/ag6r14/how_important_is_being_opensource_in_a_password/).

* Lockwise can be configured to work with Mozilla Monitor for additional security[3].

## Other considerations
* Lockwise is only available as Android and iOS apps and through the Firefox browser. There is no Google Chrome extension or support[9].

* In my own experience with Firefox browser, filling passwords with Lockwise is visibly faster than using LastPass.

## A Major Security Concern
Firefox’s master password feature is a necessary feature for securing Lockwise, but it doesn’t quite do the job.

Note: Testing was done on a Windows 10 PC running Firefox 71.0 and an Android 7.1.1 device running Firefox 68.3.0 and Firefox Lockwise 3.3.0.

### How the Master Password Works

First, the user must enable and set a master password in his/her Firefox account security settings.

When using the Firefox browser, the user is prompted to enter the master password every time he/she attempts to view or copy a password from Lockwise. The user is not prompted to enter the master password when Firefox auto-fills a password field on a website, but he/she is prompted to enter the master password each time the Firefox browser is launched.

### The Problem with the Master Password
The master password is only for encrypting the passwords locally on the user’s PC. If an attacker gains access to the user’s Firefox account, the attacker could log into Firefox on a different device (where a master password was not set) and view all of the user’s saved passwords.

**This puts the user’s passwords at significant risk of compromise in the case of phone hijacking attacks.**

In my understanding of phone hijacking, an attacker can get access to applications that the user had on his/her phone, and those applications may be logged into the user’s account when the attacker accesses them[10].

If an attacker gains access to the user’s phone, and the user has the Firefox mobile browser installed, and the browser is synced with his/her Firefox account, the attacker can view all of the user’s saved passwords[11].

With a standalone password manager like LastPass or Bitwarden, device compromises, such as phone hijacking, aren’t as much of an issue because the password manager application can be configured to require password entry every time the user wants to use it. Lockwise cannot do that yet.

When a user syncs Firefox to his/her Firefox account, it typically remains logged into the account indefinitely. If somebody gets access to the user’s device, they are also more likely to get access to the saved passwords.

### A Possible Workaround
It is possible to create two separate Firefox accounts — one that is used exclusively for Lockwise, and another for the web browser, but this would be far more inconvenient than using a standalone password manager. It would also come with the risk of accidentally saving passwords to the wrong account or logging into the wrong account. Additionally, because Lockwise doesn’t have a timeout feature, the user would need to manually sign out of Lockwise every time it is used to prevent Lockwise from staying logged in.

## Will I Use Lockwise?
If I was only going to use my passwords in the Firefox web browser on a PC, Lockwise would make sense. I would configure a master password on Firefox and feel reasonably secure. However, I typically want my passwords to be available on mobile devices and in Google Chrome as well.

Unfortunately, Lockwise is either unavailable or not quite secure enough when it comes to anything but Firefox on a PC. For those reasons, I will not be using Firefox Lockwise.

If Mozilla further segregates Lockwise from Firefox and adds a few more security features, I will reconsider because I am generally a big fan of Mozilla and its products.

## References
[1] [https://www.youtube.com/watch?v=WGXqnqEquts&t=1170s](https://www.youtube.com/watch?v=WGXqnqEquts&t=1170s)

[2] [https://github.com/mozilla-lockwise](https://github.com/mozilla-lockwise)

[3] [https://blog.mozilla.org/firefox/password-security-features/](https://blog.mozilla.org/firefox/password-security-features/)

[4] [https://www.lastpass.com/how-lastpass-works](https://www.lastpass.com/how-lastpass-works)

[5] [https://help.bitwarden.com/article/what-encryption-is-used/](https://help.bitwarden.com/article/what-encryption-is-used/)

[6] [https://support.mozilla.org/en-US/kb/use-master-password-protect-stored-logins](https://support.mozilla.org/en-US/kb/use-master-password-protect-stored-logins)

[7] [https://security.stackexchange.com/questions/220968/is-firefoxs-lockwise-secure?newreg=01e723b005024957a625bd4278469be1](https://security.stackexchange.com/questions/220968/is-firefoxs-lockwise-secure?newreg=01e723b005024957a625bd4278469be1)

[8] [https://support.mozilla.org/en-US/kb/secure-firefox-account-two-step-authentication](https://support.mozilla.org/en-US/kb/secure-firefox-account-two-step-authentication)

[9] [https://www.mozilla.org/en-US/firefox/lockwise/](https://www.mozilla.org/en-US/firefox/lockwise/)

[10] [https://us.norton.com/internetsecurity-emerging-threats-phone-hijacking-when-criminals-take-over-your-phone-and-everything-in-it.html](https://us.norton.com/internetsecurity-emerging-threats-phone-hijacking-when-criminals-take-over-your-phone-and-everything-in-it.html)

[11] [https://support.mozilla.org/en-US/kb/manage-saved-login-information](https://support.mozilla.org/en-US/kb/manage-saved-login-information)

[12] [https://github.com/mozilla-lockwise/lockwise-ios/issues/1105](https://github.com/mozilla-lockwise/lockwise-ios/issues/1105)

[13] [https://www.reddit.com/r/security/comments/ag6r14/how_important_is_being_opensource_in_a_password/](https://www.reddit.com/r/security/comments/ag6r14/how_important_is_being_opensource_in_a_password/)