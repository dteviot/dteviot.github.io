# WebToEpub Frequently Asked Questions

- [Privacy Policy](webToEpub_PrivacyPolicy.html)
- [How to select a sub-range of URLs to include in an EPUB?](#url-subrange)
- [How to convert a new site using the Default Parser?](#default-parser)
- [How to see file downloading progress?](#download-progress)
- [Using Baka-Tsuki "Series Page" parser?](#baka-tsuki-series-page)
- [How to write a new Parser?](#write-parser)
- [Structure of WebToEpub code?](#code-structure)

<h2 id="url-subrange">How to select a sub-range of URLs to include in an EPUB?</h2>
1. Make sure all the URLs you want in the EPUB are selected.  (Easiest way to do this is click on the "Select All" button, which will select ALL URLs.)
2. Click on the "Edit Chapter URLs" button.
3. A text editor with a list of hyperlinks, one hyperlink for each selected URL will open.  Each hyperlink looks like this:
```HTML
<a href="URL_TO_DOWNLOAD">Title</a>
```
Where href is the URL to obtain a chapter to put in the EPUB, and "Title" is the title that will be given to the chapter in the table of contents.
The chapters will be placed in the EPUB in the same order they appear on this page.
4. Edit the hyperlinks. e.g. Delete the URLs that are not wanted. You can also change the order of the hyperlinks or their titles. Note, you may find it easier to copy/paste the hyperlinks into a text editor to modify them, then paste back the edited list.
5. Click the "Apply Changes" button.

<h2 id="default-parser">How to convert a new site using the Default Parser?</h2>
<ol>
<li>The first step is to discover the <a href="https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Getting_started">HTML element</a> that contains the content to put into each chapter of the EPUB.  To do this 
  <ol>
  <li>Open a chapter URL in your web browser of choice.</li>
  <li>Open the browser's DOM Inspector.  E.g. On Firefox use <a href="https://developer.mozilla.org/en-US/docs/Tools/Add-ons/DOM_Inspector/Introduction_to_DOM_Inspector">CTRL+Shift+C</a>, on Chrome open <a href="https://developers.google.com/web/tools/chrome-devtools/inspect-styles/edit-dom">"Developer Tools" and select the "Elements" tab</a></li>
  <li>Find the HTML element that encloses the entire text your want in the EPUB and record the element's:
    <ul>
    <li>Type (e.g. &lt;Body&gt;, &lt;Div&gt;, &lt;Article&gt;),</li>
    <li>id (if there is one.)  If there's no ID, check the </li>
    <li>class (if there is one.)  Note, multiple elements may have the same class, so, you need to check that the class is unique.  If class is not unique, then search the element's parents for one with an id or unique class.</li>
    </ul>
  </li>
  </ol>
</li>
<li>Open the web page with the list of URLs that are the chapters to pack into the EPUB.</li>
<li>Click on WebToEpub's icon to open WebToEpub.</li>
<li>You will get a warning message that Default parser will be used. Click OK and WebToEpub will open.</li>
<li>Above the "Pack EPUB" button, there's a line titled "Element with Chapter Content" with two drop down controls and an edit box.  Set these controls to the content element you found in the first step.  e.g. Assume in step 1 you found that the content was in a &lt;Div&gt; element with a class name of "post-body".  In that case you would
  <ol>
  <li>Set the first drop down to &lt;Div&gt;.</li>
  <li>Set the second drop down to "Class is".</li>
  <li>Set the edit box to "post-body" (Don't include the quotes.)</li>
  </ol>
</li>
<li>In the list of chapters to fetch, uncheck the chapters you don't want. (The default parser will show every hyperlink it finds on the page.)</li>
<li>(Optional) Set "Cover Image URL:" to a suitable image.</li>
<li>Click "Pack epub"</li>
</ol>

<h2 id="download-progress">How to see file downloading progress?</h2>
WebToEpub's display of progress in downloading the requested URLs is limited.  
To obtain a much more detailed and more frequently updated download progress update, you can use the Network Monitoring facilities built into Chrome and Firefox.
The basic procedure is:
1. Open the respective developer tools.
2. On the Developer tools pane, select the networking tab.
3. Click WebToEpub's "Pack EPUB" button.

For more detailed instructions, try the following links
- [Chrome](https://developers.google.com/web/tools/chrome-devtools/network-performance/resource-loading) or 
- [Firefox](https://developer.mozilla.org/en-US/docs/Tools/Network_Monitor)

<h2 id="baka-tsuki-series-page">Using Baka-Tsuki "Series Page" parser?</h2>
When originally created, WebToEpub only worked for "Full Text" web pages.  That is, web pages that had the Full Text of a volume. e.g. [https://www.baka-tsuki.org/project/index.php?title=Shinmai_Maou_no_Keiyakusha:Volume_7](https://www.baka-tsuki.org/project/index.php?title=Shinmai_Maou_no_Keiyakusha:Volume_7).
In version 0.0.0.43, I've added a new parser that will work with "Series Pages" that show all the volumes in a series.  e.g. [https://www.baka-tsuki.org/project/index.php?title=Shinmai_Maou_no_Keiyakusha](https://www.baka-tsuki.org/project/index.php?title=Shinmai_Maou_no_Keiyakusha).
However, there is a problem. I've not been able to discover a reliable way for WebToEpub to distinguish between the two page types.

So, by default WebToEpub will use the old "Full Text" parser when it encounters a Baka-Tsuki web page.  However, if you browse to a Baka-Tsuki series page, you can use the "Manually Select Parser" drop down control under "Advanced Options" and select the "Baka-Tsuki Series Page" parser.

Alternately, you can check the "Automatic parser select includes Baka-Tsuki Series Page Parser".option. When checked, WebToEpub will make a "best guess" at which of the two Baka-Tsuki parsers should be used for the currently selected Baka-Tsuki page.  However, it won't always pick the correct parser.  When this happens, you will need to manually select the correct parser.

<h2 id="write-parser">How to write a new Parser?</h2>
If you have basic knowledge of JavaScript and HTML then creating a new parser for a site that WebToEpub can't currently handle may be as short as 10 minutes work.
Basic steps are:
1. Install from Source, using the [instructions here](https://github.com/dteviot/WebToEpub#how-to-install-from-source)
2. Copy the file "Template.js" in the folder plugin/js/parsers.
3. Rename the copied file, based on the site you want to parse.
4. Add link to the new parser to popup.html.
5. Text replace "Template" in the file with the new Parser name.
6. Uncomment the functions of the template you need, modifying the sample implementations as required.

ToDo: provide worked example of creating a new parser.

<h2 id="code-structure">Structure of WebToEpub code?</h2>
ToDo

