# simple XSS attack

In a real-world web app pentest, we would test this for a variety of things, one of which would be Cross-Site Scripting (or XSS). If you have not yet encountered XSS, it can be thought of as injecting a client-side script (usually in Javascript) into a webpage in such a way that it executes. There are various kinds of XSS – the type that we are using here is referred to as "Reflected" XSS, as it only affects the person making the web request.

client-side filters are absurdly easy to bypass. There are a variety of ways we could disable the script or just prevent it from loading in the first place.

we just intercept the request and then put the data we want in the request data.

e.g. we put `<script>alert("Succ3ssful XSS")<script>` instead of email and then url encode it with `Ctrl + u`&#x20;

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

There are three major types of XSS attacks:

<table data-header-hidden><thead><tr><th width="222"></th><th></th></tr></thead><tbody><tr><td>DOM (Special)<br></td><td>DOM XSS <em>(Document Object Model-based Cross-site Scripting)</em> uses the HTML environment to execute malicious javascript. This type of attack commonly uses the <em>&#x3C;script>&#x3C;/script></em> HTML tag.<br></td></tr><tr><td>Persistent (Server-side)<br></td><td>Persistent XSS is javascript that is run when the server loads the page containing it. These can occur when the server does not sanitise the user data when it is uploaded to a page. These are commonly found on blog posts. <br></td></tr><tr><td>Reflected (Client-side)<br></td><td>Reflected XSS is javascript that is run on the client-side end of the web application. These are most commonly found when the server doesn't sanitise search data. <br></td></tr></tbody></table>

\
More information: [Cross-Site Scripting XSS](https://owasp.org/www-project-top-ten/OWASP\_Top\_Ten\_2017/Top\_10-2017\_A7-Cross-Site\_Scripting\_\(XSS\))

from owasp juice:

## DOM XSS example:

<figure><img src="../../.gitbook/assets/image (87).png" alt=""><figcaption></figcaption></figure>

We will be using the iframe element with a javascript alert tag:&#x20;

_\<iframe src="javascript:alert(\`xss\`)">_&#x20;

Inputting this into the search bar will trigger the alert.

<figure><img src="../../.gitbook/assets/image (88).png" alt=""><figcaption></figcaption></figure>

Note that we are using iframe which is a common HTML element found in many web applications, there are others which also produce the same result.&#x20;

This type of XSS is also called XFS (Cross-Frame Scripting), is one of the most common forms of detecting XSS within web applications.

Websites that allow the user to modify the iframe or other DOM elements will most likely be vulnerable to XSS.  &#x20;

Why does this work?

It is common practice that the search bar will send a request to the server in which it will then send back the related information, but this is where the flaw lies. Without correct input sanitation, we are able to perform an XSS attack against the search bar.&#x20;

## persistent XSS example:

First, login to the admin account.

We are going to navigate to the "Last Login IP" page for this attack.\


<figure><img src="../../.gitbook/assets/image (89).png" alt=""><figcaption></figcaption></figure>

It should say the last IP Address is 0.0.0.0 or 10.x.x.x&#x20;

As it logs the 'last' login IP we will now logout so that it logs the 'new' IP.

<figure><img src="../../.gitbook/assets/image (90).png" alt=""><figcaption></figcaption></figure>

Make sure that Burp intercept is on, so it will catch the logout request.

We will then head over to the Headers tab where we will add a new header:

<table data-header-hidden><thead><tr><th width="353">header</th><th>value</th></tr></thead><tbody><tr><td><em>True-Client-IP</em><br></td><td><em>&#x3C;iframe src="javascript:alert(`xss`)"></em><br></td></tr></tbody></table>

<figure><img src="../../.gitbook/assets/image (91).png" alt=""><figcaption></figcaption></figure>

Then forward the request to the server!\
When signing back into the admin account and navigating to the Last Login IP page again, we will see the XSS alert!

<figure><img src="../../.gitbook/assets/image (92).png" alt=""><figcaption></figcaption></figure>

Why do we have to send this Header?

The _True-Client-IP_  header is similar to the _X-Forwarded-For_ header, both tell the server or proxy what the IP of the client is. Due to there being no sanitation in the header we are able to perform an XSS attack.&#x20;

## reflected XSS example:

First, we are going to need to be on the right page to perform the reflected XSS!

Login into the admin account and navigate to the 'Order History' page.&#x20;

<figure><img src="../../.gitbook/assets/image (93).png" alt=""><figcaption></figcaption></figure>

From there you will see a "Truck" icon, clicking on that will bring you to the track result page. You will also see that there is an id paired with the order.  &#x20;

<figure><img src="../../.gitbook/assets/image (95).png" alt=""><figcaption></figcaption></figure>

We will use the iframe XSS, _\<iframe src="javascript:alert(\`xss\`)">,_ in the place of the _5267-f73dcd000abcc353_

After submitting the URL, refresh the page and you will then get an alert saying XSS!

<figure><img src="../../.gitbook/assets/image (97).png" alt=""><figcaption></figcaption></figure>

Why does this work?

The server will have a lookup table or database (depending on the type of server) for each tracking ID. As the 'id' parameter is not sanitised before it is sent to the server, we are able to perform an XSS attack. &#x20;
