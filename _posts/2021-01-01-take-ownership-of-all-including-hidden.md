---
title: Taking Ownership of Everything, Including Hidden Files, in a Directory
date: 2021-01-01
---

I wanted to remove the "Powered by Ghost" text in the footer of the Bitsrfr blog's theme. I also wanted to change the RSS feed hyperlink to standard RSS instead of Feedly's version. Bitsrfr runs on the Ghost.io CMS (self-hosted) and the Alto theme.

I connected to my server via SFTP with FileZilla. Easy enough. Then, I dug around until I found the Alto theme file that contained the text and link I wanted to change. As luck would have it, both were in the same file: *footer.hbs*.

I edited the file, saved it, and told FileZilla to sync it back to the server. I had done the same thing many times with many other blogs and themes. This time, it failed. Thanks to FileZilla's wonderful logging, it was easy to figure out this was a permission issue. FileZilla's log said, <code>Error:	/var/www/bitsrfr/content/themes/Alto-master/partials/footer.hbs: open for write: permission denied
Error:	File transfer failed".</code>

I wasn't sure what was wrong with the permissions though. The account I connected with was in the sudo group, and I had no trouble a few minutes before when I made a similar change to a blog on another server.

It turns out, any themes that are uploaded to Ghost.io manually (as opposed to using one of the default themes that comes pre-installed with the CMS) are owned by the *ghost* user and group.

After I figured that out, it was pretty easy to fix things. I compared the Alto theme's directory permissions to the Casper (pre-installed) theme's directory permissions. My user account and user group were the owners of all the files within the Casper theme's directory. So I needed to mimick that with the Alto theme's directory.

### Time for a quick lesson in changing directory ownership in UNIX/Linux systems.

I got hung up for a minute because I ran this command:

<code>chown -R me:me /var/www/bitsrfr/content/themes/Alto-master/*</code>
    
The UNIX command for taking ownership of a file or directory is <code>chown</code>. In order to take ownership of everything within a directory, I used the recursive flag, <code>-R</code>. Then, I set my account to be the owner, <code>me:me</code>. And finally, I entered the path to the directory in which I wanted the ownership change applied.

That almost worked, but it failed to grant ownership of hidden files to my account. After some searching, I found that the <code>-R</code> flag misses hidden files if it is used with the parent directory name followed by the * (all files) symbol.

So in my case, it missed the hidden files because my command used, <code>.../Alto-master/*</code> in specifying where the ownership change should be applied.

I didn't want to change the ownership of the *Alto-master* directory, just everything within it.

To correct the issue - to take ownership of all files and directories in the directory, including hidden files - I, first, changed into the *Alto-master* directory:

<code>cd /var/www/bitsrfr/content/themes/Alto-master</code>

Then, from inside that directory, I ran:

<code>chown -R me:me *</code>

The command worked that time. It gave me ownership of everything, including hidden files.

Since I already had ownership of the non-hidden files and directories, I could have also just changed into the *Alto-master* directory and ran:

<code>chown -R me:me .</code>
    
That would have given me ownership of all hidden files and directories.
    
After that, I had ownership of everthing in the *Alto-master* directory, and I was able to make my changes!

After I made the changes, I ran one more command to make them take effect.

<code>ghost restart</code>
    
And that is the story of how I learned that <code>chown -R</code> doesn't like \*, and how to change ownership of all files and directories recursively, including hidden files and directories.

Resources:
- https://serverfault.com/questions/156437/how-to-chown-a-directory-recursively-including-hidden-files-or-directories
- https://www.geeksforgeeks.org/permissions-in-linux/