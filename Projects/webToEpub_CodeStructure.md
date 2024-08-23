# Structure of WebToEpub code

## Overview of Files
* popup.html and js/main.js provide WebToEpub's core UI.
  * js/ChapterUrls.js, js/CoverImageUI.js, js/DefaultParserUI.js, js/ProgressBar.js and js/UserPreferences.js provide the rest of the UI functionality.
* js/ParserFactory.js selects the Parser (derived from js/Parser.js) to use to process each web page. 
  * js/ImageColletor.js (and js/Imgur.js) are used by the Parsers to handle processing images from the web page.
  * js/HttpClient.js is used to fetch web pages (and images, JSON or anything else) from the internet.
* js/EpubPacker.js assembles the EPUB file
  *  js/EpubItem.js and js/EpubItemSupplier.js are a "bridge" to convert the HTML collected by Parsers into items to put into an EPUB.
* js/Download.js handles saving the EPUB file to the hard drive. this file is called "Download" because it uses the Download API to do the save. (Yup, it's a hack.)


## Files
<table>
<thead>
<tr><td>File</td><td>Description</td></tr>
</thead>
<tr><td>popup.html</td><td>HTML that provides the UI for WebToEpub</td></tr>
<tr><td>js/main.js</td><td>Main logic behind popup.html's UI</td></tr>
<tr><td>js/ChapterUrlsUi.js</td><td>Logic for the "List of Chapters" on the UI</td></tr>
<tr><td>js/CoverImageUI.js</td><td>Logic for the "Select Cover Image" list for Baka-Tsuki on the UI</td></tr>
<tr><td>js/DefaultParserUI.js</td><td>Logic for the "Default Parser" on the UI</td></tr>
<tr><td>js/Download.js</td><td>Wraps Chrome's Download API.  (Handles saving EPUB to hard drive.)</td></tr>
<tr><td>js/EpubItem.js</td><td>An item to pack into an EPUB file. (e.g. an XHTML or image file)</td></tr>
<tr><td>js/EpubItemSupplier.js</td><td>Converts "files" from internet into EpubItems to pack into an EPUB</td></tr>
<tr><td>js/EpubMetaInfo.js</td><td>Container holding metadata for an EPUB.</td></tr>
<tr><td>js/EpubPacker.js</td><td>Assembles EPUB using metadata and items from supplier</td></tr>
<tr><td>js/ErrorLog.js</td><td>Records errors/warnings and displays to user and/or saves to file</td></tr>
<tr><td>js/Firefox.js</td><td>Code that is only needed by Firefox version of WebToEpub</td></tr>
<tr><td>js/HttpClient.js</td><td>Wraps making HTTP calls to internet.  Retry, decode response, etc.</td></tr>
<tr><td>js/ImageColletor.js</td><td>Fetch images from internet, remove duplicates, rewrite image tags for EPUB, etc.</td></tr>
<tr><td>js/Imgur.js</td><td>Logic for fetching/processing images and galleries from Imgur</td></tr>
<tr><td>js/Parser.js</td><td>Base class for reading a site's HTML and converting into EpubItems</td></tr>
<tr><td>js/ParserFactory.js</td><td>Logic to figure out which parser to use for a web page</td></tr>
<tr><td>js/ProgressBar.js</td><td>Code to manipulate the Progress Bar on the UI</td></tr>
<tr><td>js/Sanitize.js</td><td>Code to cleanup converting HTML to XHMTL</td></tr>
<tr><td>js/UserPreferences.js</td><td>UI logic for user to set Options</td></tr>
<tr><td>js/Util.js</td><td>Library of miscellaneous functions</td></tr>
</table>


## Algorithms

### Basic steps to create an EPUB
<ol>
<li>Figure out parser to use for web page(s)</li>
<li>Get URLs of web pages that need to be fetched from internet</li>
<li>For each web page
   <ol>
   <li>Fetch from internet</li>
   <li>Find content to put in EPUB</li>
   <li>Find and fetch any images needed on page</li>
   </ol></li>
<li>Convert web pages into items for EPUB
   <ol>
   <li>Find content to put in EPUB</li>
   <li>Remove junk (e.g. Scripts) from content</li>
   <li>Fixup hyperlinks (e.g. footnotes), remove next/previous chapter links (where possible)</li>
   <li>Rewrite image tags for EPUB</li>
   <li>Convert from HTML to XHTML</li>
   </ol></li>
<li>Assemble the EPUB
   <ol>
   <li>Generate Manifest</li>
   <li>Generate Table of Contents</li>
   <li>Pack items</li>
   </ol></li>
<li>Save EPUB to hard drive</li>
</ol>

Note. due to need to fix up hyperlinks that may cross chapters, can't convert web pages until all pages have been collected.

### Choosing parser for web page
ToDo - include special case of EPUB that has multiple sites with different formats requiring different parsers.


## Solutions to site issues that require special coding
<table>
<thead>
<tr><td>Problem</td><td>See Parser</td></tr>
</thead>
<tr><td>Site does not use UTF8 encoding or inform of coding used</td><td>69shu</td></tr>
<tr><td>Each chapter spans multiple HTML pages</td><td><a href="https://github.com/dteviot/WebToEpub/blob/ExperimentalTabMode/plugin/js/parsers/YushuboParser.js">YushuboParser.js</a></td></tr>
<tr><td>Chapters "links" in Table of Contents (ToC) are not hyperlinks</td><td>ArchiveOfOurOwn</td></tr>
<tr><td>Walk multiple ToC pages to get all Chapters</td><td>BabelChain, Scribblehub</td></tr>
<tr><td>Make multiple REST calls to get all ToC "pages" for all Chapters</td><td>Novelsect</td></tr>
<tr><td>ToC requires REST call(s) to list all chapters</td><td>GravityTales, Lnmtl</td></tr>
<tr><td>ToC across multiple HTML pages</td><td>AsianHobbyist, Novelfull, Shinsori, ZenithNovels</td></tr>
<tr><td>Assemble chapter content from JSON</td><td>Novelsect</td></tr>
</table>

