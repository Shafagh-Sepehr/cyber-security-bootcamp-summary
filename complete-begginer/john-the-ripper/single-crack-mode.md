# Single Crack Mode

## **Single Crack Mode**

So far we've been using John's wordlist mode to deal with brute forcing simple., and not so simple hashes. But John also has another mode, called Single Crack mode. In this mode, John uses only the information provided in the username, to try and work out possible passwords heuristically, by slightly changing the letters and numbers contained within the username.

\


**Word Mangling**\



The best way to show what Single Crack mode is,  and what word mangling is, is to actually go through an example:

If we take the username: Markus

Some possible passwords could be:

* Markus1, Markus2, Markus3 (etc.)
* MArkus, MARkus, MARKus (etc.)
* Markus!, Markus$, Markus\* (etc.)

This technique is called word mangling. John is building it's own dictionary based on the information that it has been fed and uses a set of rules called "mangling rules" which define how it can mutate the word it started with to generate a wordlist based off of relevant factors for the target you're trying to crack. This is exploiting how poor passwords can be based off of information about the username, or the service they're logging into.\


## **Using Single Crack Mode**

To use single crack mode, we use roughly the same syntax that we've used to so far, for example if we wanted to crack the password of the user named "Mike", using single mode, we'd use:\


`john --single --format=[format] [path to file]`

`--single` - This flag lets john know you want to use the single hash cracking mode.

## Example Usage:

`john --single --format=raw-sha256 hashes.txt`

A Note on File Formats in Single Crack Mode:

If you're cracking hashes in single crack mode, you need to change the file format that you're feeding john for it to understand what data to create a wordlist from. You do this by prepending the hash with the username that the hash belongs to, so according to the above example- we would change the file hashes.txt

From:

`1efee03cdcb96d90ad48ccc7b8666033`

To

`mike:1efee03cdcb96d90ad48ccc7b8666033`
