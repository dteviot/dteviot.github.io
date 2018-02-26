# Customizing the Template for a new Web Site

This is a worked example of customizing TemplateParser.js to handle the novellfull.com web site.
I will assume you've already read [How to write a new Parser](webToEpub_FAQ.html#write-parser) in the FAQ and done steps 1 to 5, giving the new file the name NovellfullParser.

# Step 1, Name the new Parser Class
As the web site is novellfull.com, I will call the new parser NovellfullParser.
To name the parser find the line of code in the template that goes

```javascript
class TemplateParser extends Parser{
```

and change it to

```javascript
class NovellfullParser extends Parser{
```

# Step 2, Tell WebToEpub when to use the new parser
From examining a few web pages on the site, it should be obvious that the rule that WebToEpub needs to use to know to use the NovellfulParser is very simple.
"If the URL has 'novellfull.com' as the host name, then the Novellfull parser should be used."
The rule can be expressed with the following code.

```javascript
parserFactory.register("novelfull.com", function () { return new NovelfullParser() });
````

The key parts of this line are the site's hostname and the name of the new Parser.

# Step 3, Tell new parser how to get the URLs for all the pages.
If we look at a page with a list of chapters e.g. [http://novelfull.com/magi-craft-meister.html](http://novelfull.com/magi-craft-meister.html) we can see that in this case the URLs are hyperlinks that are in an unordered list with a class of "list-chapter".  e.g. The relevant HTML looks like this:

```html
<ul class="list-chapter">
   <li><a href="/magi-craft-meister/chapter-1-epilogue-of-two-worlds.html">Volume 0 - Chapter 1 – EPILOGUE OF TWO WORLDS</a>></li>
   <li><a href="/magi-craft-meister/chapter-2-successors-birth.html">"Volume 0 - Chapter 2 – SUCCESSOR’S BIRTH</a></li>
</ul>
```

We can select all these links with using the CCS selector "ul.list-chapter a".
And the javascript code to do this is:

```javascript
getChapterUrls(dom) {
    return [...dom.querySelectorAll("ul.list-chapter a")]
        .map(link => util.hyperLinkToChapter(link, null));
};
```

Note that querySelectorAll() uses the CCS selector previously mentioned.

# Step 4, Get the story's title.
Examination of several "list of chapters" pages shows that the title of the story is a &lt;h3&gt; element with a class of "title".  e.g. It looks like

```html
<h3 class="title">Magi Craft Meister</h3>
```

We can get the text in this element with the following code:

```javascript
    extractTitle(dom) {
        return dom.querySelector("h3.title").textContent.trim();
    };
```

# Step 5, Get a chapter's content from a web page.
Examination of a couple of chapters shows that the content for each page is enclosed in a &lt;div&gt; element with an id of "chapter-content".  e.g. It looks like

```html
<div id="chapter-content">"Nidoh Jin was an orphan"
```

We can get this element with the following code:

```javascript
findContent(dom) {
    return dom.querySelector("div#chapter-content");
};
```

# Step 6, (Optional) Get the chapter's title.
The chapter content element we found in the previous step does not include the title of each chapter.
Examination of a couple of chapter pages shows that the title of each chapter is the only &lt;h2&gt; element on the page.
So, the code to obtain a chapter's title is:

```javascript
findChapterTitle(dom) {
    return dom.querySelector("h2");
}
```

# Step 7, (Optional) Get the author.
Examination of several "list of chapters" pages shows that the author's name is the first hyperlink in the &lt;div&gt; element with a class of "info"

So, the code to obtain the author's name is:

```javascript
extractAuthor(dom) {
    let authorLink = dom.querySelector("div.info a");
    return (authorLink === null) ? super.extractAuthor(dom) : authorLink.textContent;
};
```

# Step 8, (Optional) Get the cover image.
Examination of several "list of chapters" pages shows that the cover image is the first hyperlink in the &lt;div&gt; element with a class of "book"

So, the code to obtain the cover image is:
```javascript
findCoverImageUrl(dom) {
    return util.getFirstImgSrc(dom, "div.book");
}
```
