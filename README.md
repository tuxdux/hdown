# hdown
[![Build Status](https://travis-ci.org/tuxdux/hdown.svg?branch=master)](https://travis-ci.org/tuxdux/hdown)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/tuxdux/hdown/blob/master/LICENSE)

<p align="center">
<img src="example.gif">
</p>

## TABLE OF CONTENTS

* [NAME](#NAME)  
* [SYNOPSIS](#SYNOPSIS)  
* [DESCRIPTION](#DESCRIPTION)  
* [OPTIONS](#OPTIONS)  
* [USAGE](#USAGE)  
* [INSTALLING](#INSTALLING)  
* [BUGS](#BUGS)  
* [SITES](#SITES)
* [MORE EXPLANATION](#MORE_EXPLANATION)  
* [REQUIREMENTS](#REQUIREMENTS)  

## NAME<a name="NAME"></a>

hdown — A simple tool to download images from your favorite doujinshi sites.

**WARNING** : This program can only download JPG or PNG images.

## SYNOPSIS<a name="SYNOPSIS"></a>

**hdown** [<ins>OPTIONS</ins>]

## DESCRIPTION<a name="DESCRIPTION"></a>

````
This is a small bash script. Nothing extraordinary.
````

hdown is a downloading tool that uses **wget** to download images from a doujinshi site. This tool can also download images from any website that uses a certain pattern to store their images. It does not take any arguments since it reads all the required information via the standard input with prompts. (Providing a URL is easier that way and it does not get stored in the history.)

If a site stores their images as : `https://www.some-site.com/images/img001.jpg`, and there are 25 images at the site, all you need to provide the program is the <ins>PATTERN</ins>, <ins>PADDING</ins> and <ins>PAGES</ins>. Some examples of sites that store images in such format would be `hentai2read.com`, `nhentai.net` and `pururin.io`.

<ins>PATTERN</ins> — The url till just where the number starts. In the above example, it would be `https://www.some-site.com/images/img`. If there is a slash before the number, you **must** include it in the pattern.

<ins>PADDING</ins> — The number of digits in the url of the images. In the above example it would be 3, since the index of the images is stored as 001, 002, 011 and similar. If the number of digits is not fixed, like if the numbers are 1, 2, 11 and similar, you need to provide 0.

<ins>PAGES</ins> — The number of pages to download **starting from** the beginning page. Without the **-p**, **-l** or **-e** options, the images are downloaded from page 1. So if you provide 10 for <ins>PAGES</ins>, the images will be downloaded till page 10.

You will have to download (give links to the images for) each chapter manually.

**WARNING** : This program will not work if the website has the cloudflare DDoS protection turned on.

## OPTIONS<a name="OPTIONS"></a>

````
This program does not support long options.
````

**-h**

Show summary of manual.

**-p**

Start downloading from a custom page number. The <ins>PAGE-NUMBER</ins> is also taken via the standard input. If **-l** or **-e** is provided, this will have no effect other than the name of downloaded files changing. So use this option with **-l** or **-e** if you want to start naming the files from a certain number. Note that if you give 10 for <ins>PAGE-NUMBER</ins> and then 10 for <ins>PAGES</ins>, the images will be downloaded till page 19\. The last page is calculated as : <ins>PAGE-NUMBER</ins> + <ins>PAGES</ins> - 1

**-n**

Save the files with a custom name. The <ins>NAME</ins> is also taken via the standard input. The files will be saved as "_NAME PAGE_.jpg" (or png, depending on the extension). So, if you provide `Test` for the <ins>NAME</ins>, the first image will be saved as `Test 1.jpg`, the second image will be saved as `Test 2.jpg` and so on.

**-l**

Download images from `luscious.net`. If this option is provided, the program, instead of the usual <ins>PATTERN</ins> and <ins>PADDING</ins>, asks for the <ins>URL</ins> to the first page (the page from where you want the download to start from). So, if you want to download pages 8-10, you need to provide the <ins>URL</ins> to the 8th page and 3 for <ins>PAGES</ins> (8,9,10 — 3 pages). If you want the files named from 8 onwards as well, you **need** to provide the **-p** option.  
**Note that the** <ins>URL</ins> **is the actual URL to the page and not the URL to the image.**

**-e**

Download images from `e-hentai.org`. If this option is provided, the program, instead of the usual <ins>PATTERN</ins> and <ins>PADDING</ins>, asks for the <ins>URL</ins> to the first page (the page from where you want the download to start from). So, if you want to download pages 8-10, you need to provide the <ins>URL</ins> to the 8th page and 3 for <ins>PAGES</ins> (8,9,10 — 3 pages). If you want the files named from 8 onwards as well, you **need** to provide the **-p** option.  
**Note that the** <ins>URL</ins> **is the actual URL to the page and not the URL to the image.**

**-f**

Read the required data from a file. If this option is provided, the program simply asks for the path to the file which has data stored in the following form (<ins>FILE</ins> is also taken via standard input with prompt) :
````
; Comment
OPTIONS ; Another Comment
DATA
BLANK LINE
OPTIONS
DATA
````

The only problem would be to maintain the order of data. First, custom file name (if using **-n**), then custom page number (if using **-p**) and then everything else, i.e, the usual order in which the program requests the data. So a perfect example of <ins>FILE</ins> would be :
````
; Comment
-np
Test
10
https://www.some-site.com/images/img
4
11

https://www.some-other-site.com/img
3
20

-e
https://e-hentai.org/proceeding-long-url
20
````

* This would run the program first for :    
<ins>NAME</ins> = `Test`    
<ins>PAGE-NUMBER</ins> = `10`    
<ins>PATTERN</ins> = `https://www.some-site.com/images/img`    
<ins>PADDING</ins> = `4`    
<ins>PAGES</ins> = `11`

* Then after completion of first download, the program would run with :    
<ins>PATTERN</ins> = `https://www.some-other-site.com/img`    
<ins>PADDING</ins> = `3`    
<ins>PAGES</ins> = `20`

* Then after completion of second download, the program would run with option **-e**, with :    
<ins>URL</ins> = `https://e-hentai.org/proceeding-long-url`    
<ins>PAGES</ins> = `20`.

So, you can simply give data without any options like usual, even from a file. The data for **-l** and **-e** can also be given in the usual way. This makes downloading multiple chapters easier.  
The images from first data-set will be saved in a directory named as `RUN-1`, the images from second data-set in a directory named as `RUN-2` and so on.  
But make sure you add the - before the options even in the file.

**Anything after a semi-colon will be ignored.**

**WARNING** : **-l** and **-e** cannot be used together. The first one provided will be considered.

## USAGE<a name="USAGE"></a>

The various options can be combined, except for **-l** and **-e** together.

**EXAMPLE 1**

Suppose you want to download `25` images starting from `https://www.some-site.com/123456/img0003.jpg` (this would mean downloading pages 3-27) and you want the files named as `Test`.

You invoke hdown as :
````
$ hdown -np
````
Now you will be asked to (due to the **-n** option provided) :
````
Enter custom file name :
````
Here, enter the <ins>NAME</ins> with which you want to save files (explained in the **-n** option section). `Test` for this example.

Now you will be asked to (due to the **-p** option provided) :
````
Enter page no. to start from :
````
Here, enter the <ins>PAGE-NUMBER</ins> from which you want the download to start from (explained in the **-p** option section). `3` for this example.

Now you will be asked to :
````
Enter url pattern :
````
Here, enter the <ins>PATTERN</ins> as explained in the **DESCRIPTION** section. `https://www.some-site.com/123456/img` for this example.

Now you will be asked to :
````
Enter padding of index :
````
Here, enter the <ins>PADDING</ins> as explained in the **DESCRIPTION** section. `4` for this example.

Now you will be asked to :
````
Enter number of pages :
````
Here, enter the <ins>PAGES</ins> as explained in the **DESCRIPTION** section. `25` for this example.

That is it. Now the images will be downloaded to the current directory.

**EXAMPLE 2**

Suppose you want to download first 10 images of a particular doujinshi from luscious.net.

You invoke hdown as :
````
$ hdown -l
````

Now you will be asked to :
````
Enter url to the beginning page :
````
Here, enter the <ins>URL</ins> with which you want to save files (explained in the **-l** option section).

Now you will be asked to :
````
Enter number of pages :
````
Here, enter the <ins>PAGES</ins> as explained in the **DESCRIPTION** section. `10` for this example.

That is it. Now the images will be downloaded to the current directory.

## INSTALLING<a name="INSTALLING"></a>
To install, clone or download this repository and run the following command from the directory :
````
$ sudo ./install
````

If you want to uninstall, use :
````
$ sudo ./uninstall
````

## BUGS<a name="BUGS"></a>

* The program still creates empty files even if the download failed (this is a problem in **wget** actually).
* The program gets stuck if the server does not respond. This is often encountered when downloading from `e-hentai.org`.

## SITES<a name="SITES"></a>
This is a small list of sites that **hdown** can download images from :

SITE | SUPPORT
---- | -------
[nhentai.net](https://nhentai.net) | pattern-wise download
[hentai2read.com](https://hentai2read.com) | pattern-wise download
[pururin.io](https://pururin.io) | pattern-wise download
[hentaihere.com](https://hentaihere.com) | pattern-wise download
[luscious.net](https://luscious.net) | option support
[e-hentai.org](https://e-hentai.org) | option support

## MORE EXPLANATION<a name="MORE_EXPLANATION"></a>
Suppose you want to download all of a doujinshi from `hentai2read.com`

* Right-click on the image of the first page and click `View Image`
* Copy the url to just before where the index starts.
* Invoke hdown as : `hdown`
* Give the copied url when asked for `Enter url pattern :`
* Give the padding of index when asked for `Enter padding of index :`
* Give the number of pages when asked for `Enter number of pages :`
* Wait for the download to complete and enjoy.

## REQUIREMENTS<a name="REQUIREMENTS"></a>
* wget
* coreutils

