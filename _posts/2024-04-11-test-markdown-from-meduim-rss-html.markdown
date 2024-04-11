---
layout: post
title:  Test markdown from medium rss
date:   2024-04-11 15:53:11 +0100
categories: jekyll update
excerpt_separator: <!--more-->
greeting: hello
image:
  url: assets/images/blog/2.jpeg
  alt: Alter image
---

![](https://cdn-images-1.medium.com/max/1024/1*Dl0F36mQqB_LNtA_dKd3ug.jpeg)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

_This is part three of a series explaining how to publish your book using Markdown. Here are_ [_part one_](https://medium.com/@blittler/writing-your-book-in-markdown-e6091a5c7a2e) _and_ [_part two_](https://medium.com/@blittler/creating-ebook-epub-and-mobi-files-from-markdown-fe1f2d6379a3)_._

When you’re getting your ebook ready for publishing — on Kindle or elsewhere — it may feel as if you’ve entered the Matrix.

Fortunately, I’m here to help you decipher it.

In order to properly format your ebook, there are a few areas you’ll need to consider:

1.  Metadata — primarily bibliographic info used to label your book
2.  Front matter — including your title page and TOC
3.  Formatting within your book — including indents, how headers appear, etc.
4.  Pandoc ‘switches’ — which is how we tie it all together

Let’s get to it.

### Metadata

Metadata is what Kindle and other publishing platforms use to populate bibliographic information about your book such as:

*   Title
*   Author
*   Publisher
*   Copyright
*   Published date

While there are a number of methods for including metadata, including adding it via a [YAML](https://pandoc.org/MANUAL.html#epub-metadata) data block (either inline or as a separate file), I prefer instead to use a metadata.xml file. The reason for this is twofold:

1.  Pandoc, which is our preferred processing tool, will automatically create a title page if you include any YAML metadata. I think this looks ugly and unprofessional. Instead, you can create your own title page and format it however you like.¹
2.  Kindle (and possibly other platforms) requires a certain amount of metadata, so we must include it in some way.

Including a metadata file is as simple as creating the file and referencing it when you build your epub in Pandoc. Here’s an example of the metadata.xml file that I use:

<dc:title id="title">Book Title: Book Subtitle</dc:title>   
<meta refines="#title" property="title-type">main</meta>  
<dc:language>en-us</dc:language>   
<dc:creator id="metadata\_id\_creator">Author Name</dc:creator>   
<dc:publisher>Publisher Name</dc:publisher>  
<dc:date>2020-08-05</dc:date>  
<dc:rights>Copyright © 2020 by Author Name</dc:rights>

Note that dc is short for “[Dublin Core](https://www.dublincore.org/specifications/dublin-core/dc-xml-guidelines/)”, which is a metadata format, similar in format to HTML.

The date should also be in the format YYYY-MM-DD.

### Front Matter

While the front matter will vary depending on the type of book you’re publishing, typically you’ll have a few sections:

1.  Title page
2.  Copyright page
3.  Dedication
4.  Foreward, prologue, or introduction
5.  Table of contents.

Personally, I’m a fan of centering the text on the copyright and title pages. You can do this easily in your markdown file by including standard HTML tags inline, like this:

<div style="text-align: center;">  
Copyright &copy; 2020 by Author Name. All rights reserved.  
<br /><br />  
Publisher Name :: Austin, TX  
<br />  
\[[www.weblink.co\](http://www.weblink.co)](http://www.robotarmy.co](http://www.robotarmy.co))  
<br /><br />  
ISBN 111-0-111-11111-0 (epub)  
</div>

For the dedication, forward, introduction, and other passages, no special formatting is needed — just make sure to separate them with header tags (# ) so that they are contained in their own sections.

If your book is fiction, then you probably don’t need a TOC. For non-fiction though, it’s helpful. Fortunately for us, Pandoc takes care of this automatically by generating a TOC based on our header tags. This is addressed in the ‘Pandoc switches’ section — just make sure to use header and subheader tags as appropriate for each section.

### Book Formatting

There are two ways to format your book: using CSS (also known as a ‘stylesheet’), and using inline formatting.

For the most part, we want to use CSS as this will make it easier to automatically format the entire manuscript. In some cases, though, we might want to apply special formatting (like in our copyright or title pages, as I’ve noted above), in which case, inline formatting is helpful.

Fair warning: CSS can be quite a black hole, and is very much subject to personal preference. This guide cannot cover all the possible permutations of CSS, but for the most part, there are just a few things that are critically important for epub files. They are:

1.  Indentation — do you want to indent each paragraph (as is typical in traditional publishing), or have line breaks between each paragraph?
2.  Table of contents — do you prefer your TOC to be numbered, or unnumbered?
3.  Special formatting — do you have specific formatting that you’d like to see for special elements, like blockquote (inline comments, often used when you’re quoting someone), or HR tags (separators)?

Ultimately, the sky is the limit in terms of CSS, and this is definitely an area worth playing with. In general, my approach is ‘the less CSS the better’, but there are plenty of resources around the web if you have the patience to experiment. Here are a few that I’d recommend:

*   [BB eBooks - CSS Boilerplate](http://bbebooksthailand.com/bb-CSS-boilerplate.html)
*   [mattharrison/epub-css-starter-kit](https://github.com/mattharrison/epub-css-starter-kit)

And here is my baseline CSS for Kindle, without indentation:

/\* kindle friendly css \*/

/\*promote h4 and h5 tags to 100$ font size\*/  
h4, h5 {font-size: 100%;}

/\* format blockquotes nicely \*/  
blockquote {  
 font-size: 90%;   
 color: #666666;  
 margin: 0;  
 padding-left: 3em;  
 border-left: 0.5em #EEE solid;  
}

/\* format hr lines nicely \*/  
hr  
{  
 height: 0;  
 margin-top: 1em;  
 margin-bottom: 1em;  
 border: 0;  
 border-top: 1px solid #eee;  
}

/\*suppress numbering in TOC to match the default pandoc styling\*/  
ol.toc li { list-style-type: none; margin: 0; padding: 0; }

With indentation, but forcing the first line of every chapter to not indent:

/\* kindle friendly css with indents\*/

p { margin: 0rem 0% 0rem 0rem; text-indent: 1rem; }

h1 + p, h2 + p { text-indent: 0em; }

h4, h5 {font-size: 100%;}

blockquote {  
 font-size: 90%;   
 color: #666666;  
 margin: 0;  
 padding: 1em 0 1em 3em;  
 border-left: 0.25em #EEE solid;  
}

blockquote > p { text-indent: 0em; }

hr  
{  
 height: 0;  
 margin-top: 1em;  
 margin-bottom: 1em;  
 border: 0;  
 border-top: 1px solid #eee;  
}

/\*suppress numbering in TOC to match the default pandoc styling\*/  
ol.toc li { list-style-type: none; margin: 0; padding: 0; }

Finally, do make sure to use [header tags properly](https://medium.com/@blittler/writing-your-book-in-markdown-e6091a5c7a2e), as this will have a big impact on how your book is organized and where page breaks are set.

### Pandoc Switches

In order to pull all of our formatting together, we need to tell Pandoc what files and ‘switches’ (i.e. options) to use. There are [many options](https://pandoc.org/MANUAL.html#options) available within Pandoc, but to be honest, it’s an overwhelming list. To make things a little easier, I’ve made a list of the switches that I typically use:

pandoc manuscrupt.md -o ebook.epub -t epub3 --toc --toc-depth=3   
\--epub-chapter-level=2 --epub-cover-image=cover.jpg   
\--css=epub.css --epub-metadata=metadata.xml

Let’s break it down.

pandoc manuscrupt.md -o ebook.epub

This fires up the Pandoc app and tells it to use your markdown (manscript.md) file as the input, and output it to an epub file (ebook.epub).

\-t epub3

This tells Pandoc to output the file format as epub (in our case, the latest version, which is epub3).

\--toc --toc-depth=3

This tells Pandoc to generate a table of contents at a depth of 3 headers (e.g. H1 to H3). Feel free to adjust this as you see fit.

\--epub-chapter-level=2 --epub-cover-image=cover.jpg

This option will break chapters at an H2 heading. While not necessary, it’s helpful if you’re using H1 tags as top-level sections (e.g. ‘Part One’ and ‘Part Two’), as this will ensure that each of your sections has its own dedicated pages (rather than running right into the first chapter of the section).

\--epub-cover-image=cover.jpg

This option embeds your cover image. Make sure the file path is correct (for simplicity, I just put the cover image in the same directory as my manuscript).

\--css=epub.css

This option tells Pandoc where to find your CSS file. Again, make sure the file path is correct.

\--epub-metadata=metadata.xml

And finally, this points to Pandoc to your metadata file.

### Testing

You’ll want to test your formatting with each platform that you intend to publish on, in particular Kindle. [I outline how to do that here](https://medium.com/@blittler/creating-ebook-epub-and-mobi-files-from-markdown-fe1f2d6379a3). This is especially important while you’re tweaking your CSS, as how one ebook platform displays it may vary from another.

\[1\] Leaving out the expected inline or YAML metadata will cause Pandoc to generate the following error: This document format requires a nonempty <title> element. Just ignore it — nothing will break.

![](https://medium.com/_/stat?event=post.clientViewed&referrerSource=full_rss&postId=ee15a5992bc)
