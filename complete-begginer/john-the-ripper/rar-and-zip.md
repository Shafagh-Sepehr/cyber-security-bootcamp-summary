# rar and zip

## zip2john

`zip2john [options] [zip file] > [output file]`\


`[options]` - Allows you to pass specific checksum options to zip2john, this shouldn't often be necessary



Example Usage

`zip2john zipfile.zip > zip_hash.txt`

***

## **rar2john**

Almost identical to the zip2john tool that we just used, we're going to use the rar2john tool to convert the rar file into a hash format that John is able to understand. The basic syntax is as follows:

`rar2john [rar file] > [output file]`



Example Usage

`rar2john rarfile.rar > rar_hash.txt`
