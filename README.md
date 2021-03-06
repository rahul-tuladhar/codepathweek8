# Project 8 - Pentesting Live Targets

Time spent: **5** hours spent in total

> Objective: Identify vulnerabilities in three different versions of the Globitek website: blue, green, and red.

The six possible exploits are:
* Username Enumeration
* Insecure Direct Object Reference (IDOR)
* SQL Injection (SQLi)
* Cross-Site Scripting (XSS)
* Cross-Site Request Forgery (CSRF)
* Session Hijacking/Fixation

Each version of the site has been given two of the six vulnerabilities. (In other words, all six of the exploits should be assignable to one of the sites.)

## Blue

Vulnerability #1: Session Hijacking

- [ ] Summary: There is a session hijacking vulnerability where you can log into the site given another user's authenticated sessionid. 
- [ ] GIF Walkthrough: 
![Alt Text](https://github.com/rahul-tuladhar/codepathweek8/blob/master/gifs/lab8_session_hijack_blue.gif)
- [ ] Steps to recreate: 
    - Create a target and attacker, using two difference browsers and two different versions, in this case blue and green
    - Have the target log in to the site
    - Assume the attacker can get the target's `session id`
    - Use the `change_session_id.php` webpage to change the attacker's `session id` to the target
    - Attacker now enjoys access to the target's staff page

Vulnerability #2: SQL injection

- [ ] Summary: The sql injection involves changing the url so that one of the params has some sort of delay on it to signify the attacker has accessed the database. This is a blind sql injection.
- [ ] GIF Walkthrough: 
![Alt Text](https://github.com/rahul-tuladhar/codepathweek8/blob/master/gifs/lab8_sql_blue.gif)
- [ ] Steps to recreate:
    - From the home public page, go to find a salesperson tab, then click on one of the sales persons listed
    - From the given url, input the sql injection `' OR SLEEP(5)=0;--'`
    - Watch as it takes longer for the request to complete for the page
    - In the gif I showcase what the database query would be like if it was invalid. It shows a page that says: `Database query failed`

## Green

Vulnerability #1: XSS
- [ ] Summary: Here a simple script with an alert can be embedded into the feedback as a stored XSS. The script is first embedded using one of the form fields available on the website. In this case the form field is in the "Contact" web page. The stored script will be activated when the admin logs into and visits the feedback page from the staff site.
- [ ] GIF Walkthrough: 
![Alt Text](https://github.com/rahul-tuladhar/codepathweek8/blob/master/gifs/lab8_xss_green.gif)
- [ ] Steps to recreate:
    - From the homepage, go to the "Contact" page
    - Enter in some information into the "Your Name" and "Your Email" fieled
    - Enter the XSS text into the Feedback form; in this case the input was `<script>alert('rt4hc has found the XSS!');</script>`
    - As an admin log into the staff site
    - From the staff go to where the user feedback is
    - As the feedback page loads the script will the execute and an alert will pop up

Vulnerability #2: User Enumeration
- [ ] Summary: When you attempt to login with a real username, the text “Log in was unsuccessful” is bolded. When the username is not within the database of the site, the text “Log in was unsuccessful” is not bolded. The developer had two different html styles for the different responses which someone could use to enumerate users. This fix for this would be to have the same html font and bolded for both input cases.

- [ ] GIF Walkthrough: 
![Alt Text](https://github.com/rahul-tuladhar/codepathweek8/blob/master/gifs/lab8_user_enumeration_green.gif)
- [ ] Steps to recreate:
    - Go to the login page
    - Enter a username that you know would be in the database; in this case "jmonroe99" was given to us
    - Enter a user name you know would definitely not be in the database; in this case "jmonroe9"
    - Notice the difference between the two html fonts of the response "Log in attempt was unsuccessful"

## Red

Vulnerability #1: IDOR
- [ ] Summary: You can indirectly reference site pages that should not be accessible by changing the id in the URL. I showcase what the difference was between accessing salespersons with available ids and one which salesperson which you should not have been able to access.
- [ ] GIF Walkthrough: 
![Alt Text](https://github.com/rahul-tuladhar/codepathweek8/blob/master/gifs/lab8_idor_red.gif)
- [ ] Steps to recreate:
    - Go to the Find a Salesperson tab
    - Click on any one of the salespersons
    - In the URL of the salesperson's page, there is an `?id=` field where you can change the number corresponding to the salesperson's id
    - Change the number in the `?id=` to some number that brings up a page that you know you shouldn't be able to see (such as `?id=10`)

Vulnerability #2: CSRF
- [ ] Summary: An attacker can forge the csrftoken of a request along with constructing a form to then edit some parts of the web page that they wouldn't normally have access to.
- [ ] GIF Walkthrough: 
![Alt Text](https://github.com/rahul-tuladhar/codepathweek8/blob/master/gifs/lab8_csrf_red.gif)
- [ ] Steps to recreate:
    - Go to a program which allows you to construct requests with form attributes such as Burp
    - Construct the post request with the appropriate params, include csrftoken=1 or any other number since the site doesn't have csrf protection
    - Change the other params like username or first name to something that you would notice
    - Send the POST request
    - Refresh the page where you would see the editted content

## Notes

Describe any challenges encountered while doing the work

Some of the challenges were figuring out how best to capture the gif for showing the exact exploit. There wsa also some confusion on what I was supposed to see in the user enumeration and the sql injection.
