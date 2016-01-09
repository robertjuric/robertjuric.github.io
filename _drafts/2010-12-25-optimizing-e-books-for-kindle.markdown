---
author: robertjuric
comments: true
date: 2010-12-25 03:03:32+00:00
layout: post
slug: optimizing-e-books-for-kindle
title: Optimizing e-Books for Kindle
wordpress_id: 355
categories:
- General Tech
tags:
- conversion
- ebook
- Kindle
- Mobipocket
- NCX
- PDF
- PRC
---

[![](http://robertj.files.wordpress.com/2010/12/before.jpg?w=1024)](http://robertj.files.wordpress.com/2010/12/before.jpg)

I've recently started uploading more technical documents to my Kindle for ease of access. But to compete with paper documents, I have to be able to access the information quickly. This means I rely heavily on the navigation built into the Kindle. I like to be able to jump straight to the Table of Contents, click on the chapter I want, or be able to quickly skip chapters. So what's wrong with this picture? This comes from a PDF e-book I purchased from CiscoPress and copied to my Kindle. There are 3 major things I have a problem with in this PDF. 1. There are no click-able links in the Table of Contents. 2. The Go To menu is missing an entry for the Table of Contents. 3. There are no tick marks to use the five-way controller for navigation. These are pretty major problems for me because it only allows me to read the document straight through, however I'd much rather be able to skip around through the book. Let's see if there is a better way to do this...

What we're going to do is convert the PDF to a PRC file. This will allow us to create the navigation that we want, but it will take a little extra leg work in order to properly apply the finishing touches.

1. First, Download and Install [Mobipocket eBook Creator](http://www.mobipocket.com/en/DownloadSoft/ProductDetailsCreator.asp)

2. Import Your PDF for Conversion


[![](http://robertj.files.wordpress.com/2010/12/screenshot006.jpg)](http://robertj.files.wordpress.com/2010/12/screenshot006.jpg)


[![](http://robertj.files.wordpress.com/2010/12/screenshot007.jpg)](http://robertj.files.wordpress.com/2010/12/screenshot007.jpg)Take note of the publication folder, our next step involves browsing the publication folder to find an image to use as the Cover image.

3. Locate and Set the Cover Image


[![](http://robertj.files.wordpress.com/2010/12/screenshot0081.jpg)](http://robertj.files.wordpress.com/2010/12/screenshot0081.jpg)




[![](http://robertj.files.wordpress.com/2010/12/screenshot009.jpg)](http://robertj.files.wordpress.com/2010/12/screenshot009.jpg)




4. Next we have to edit the .HTML file so that we can find the HTML Anchor names to create our TOC item on the Go To menu.




[![](http://robertj.files.wordpress.com/2010/12/screenshot010.jpg)](http://robertj.files.wordpress.com/2010/12/screenshot010.jpg)




Here we can see that there is an anchor named "link1" designating the Contents page in the book. We'll now add this reference as a TOC Guide Item.




[![](http://robertj.files.wordpress.com/2010/12/screenshot011.jpg)](http://robertj.files.wordpress.com/2010/12/screenshot011.jpg)To create the link, we select the .HTML file by browsing for it, and then appending the HTML anchor of the Contents page. Next we need to save the project to create the .OPF Mobipocket project file.




6. At this point we can either call it quits and build the .PRC file, or we can push forward and create a .NCX file so that we can use the five-way controller for navigation. I agree, let's move on and do this right.




Araby Greene wrote a great guest article [here](http://www.cjs-easy-as-pie.com/p/create-ncx-file-by-araby-greene.html) discussing the NCX file and it's use in-depth. I suggest you read the article and then come back for a high-level of what you need to add a NCX file to your project. Alright, now that you're back let's build our NCX file. You will have to go back to the original .HTML file we edited and search for more anchor names for each of the chapters, and also any other items such as preface, index, appendix, etc. that you may want to add. For my test example I used the Contents page, and Chapter 1 and 2:




[![](http://robertj.files.wordpress.com/2010/12/screenshot012.jpg)](http://robertj.files.wordpress.com/2010/12/screenshot012.jpg)The key items needed to create the navPoints are an id, the sequential playOrder (the order for the ticks), the text for the label, and the HTML link and anchor.




Here is a building block example:




    
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE ncx PUBLIC "-//NISO//DTD ncx 2005-1//EN" 
      "http://www.daisy.org/z3986/2005/ncx-2005-1.dtd">
    <ncx xmlns="http://www.daisy.org/z3986/2005/ncx/"  
      version="2005-1" xml:lang="en-US">
    <head>
    <meta name="dtb:uid" content="uid"/>
    <meta name="dtb:depth" content="1"/>
    <meta name="dtb:totalPageCount" content="0"/>
    <meta name="dtb:maxPageNumber" content="0"/>
    </head>
    <docTitle><text>Document Title</text></docTitle>
    <docAuthor><text>Document Author</text></docAuthor>
    <navMap>
    <navPoint id="uniqueID" playOrder="1">
    <navLabel><text>TextLabel</text></navLabel>
    <content src="link.html#anchor"/>
    </navPoint>
    </navMap>
    </ncx>
    




7. Next, save this file as TOC.ncx and place it in the Publication folder.




8. Now we have to edit the .OPF project file to add insert a reference to the .NCX file:




The code to paste is:




    
    <item id="ncx" media-type="application/x-dtbncx+xml" href="toc.ncx"></item></manifest><spine toc="ncx"><itemref idref="item1"/></spine>


You are essentially adding an item to the manifest and rewriting the spine.

[![](http://robertj.files.wordpress.com/2010/12/screenshot015.jpg)](http://robertj.files.wordpress.com/2010/12/screenshot015.jpg)9. Now we can save the .OPF and build the .PRC

10. We can download and install the [Kindle Previewer](http://www.amazon.com/gp/feature.html?ie=UTF8&docId=1000234621) to open up the .PRC and verify the tick marks show up. Next we can copy the .PRC to the Kindle and enjoy an easily navigable reference document.

And voila...


[![](http://robertj.files.wordpress.com/2010/12/after.jpg?w=1024)](http://robertj.files.wordpress.com/2010/12/after.jpg)We now have click-able links in our Contents page, a shortcut to the Contents page in the Go To menu, and tick marks we can use to skip chapters using the five-way controller. A few caveats though: when importing the PDF into Mobipocket it might not create a link for every item in the table of contents. This could easily be fixed by modifying the .HTML file and adding anchors and links. I feel as long as I get the chapters linked I'm 80% happy. I also noticed that the 13.6mb PDF file I first started with turned into a 6.4mb PRC file.


So if you are as detailed orientated as I am, you can now enjoy your tech docs in geeky bliss.

Merry Christmas!
