# IDOR

## What is an IDOR?

IDOR stands for Insecure Direct Object Reference and is a type of access control vulnerability.\


This type of vulnerability can occur when a web server receives user-supplied input to retrieve objects (files, data, documents), too much trust has been placed on the input data, and it is not validated on the server-side to confirm the requested object belongs to the user requesting it.

## **Encoded IDs**

When passing data from page to page either by post data, query strings, or cookies, web developers will often first take the raw data and encode it. Encoding ensures that the receiving web server will be able to understand the contents.

<figure><img src="../.gitbook/assets/image (105).png" alt=""><figcaption></figcaption></figure>

## **Hashed IDs**

Hashed IDs are a little bit more complicated to deal with than encoded ones, but they may follow a predictable pattern, such as being the hashed version of the integer value. For example, the Id number 123 would become 202cb962ac59075b964b07152d234b70 if md5 hashing were in use.



It's worthwhile putting any discovered hashes through a web service such as [https://crackstation.net/](https://crackstation.net/) (which has a database of billions of hash to value results) to see if we can find any matches.

## **Unpredictable IDs**

If the Id cannot be detected using the above methods, an excellent method of IDOR detection is to create two accounts and swap the Id numbers between them. If you can view the other users' content using their Id number while still being logged in with a different account (or not logged in at all), you've found a valid IDOR vulnerability.

## **Where are they located?**

The vulnerable endpoint you're targeting may not always be something you see in the address bar. It could be content your browser loads in via an AJAX request or something that you find referenced in a JavaScript file.&#x20;



Sometimes endpoints could have an unreferenced parameter that may have been of some use during development and got pushed to production. For example, you may notice a call to **/user/details** displaying your user information (authenticated through your session). But through an attack known as parameter mining, you discover a parameter called user\_id that you can use to display other users' information, for example, **/user/details?user\_id=123**.

