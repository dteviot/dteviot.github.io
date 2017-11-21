# WebToEpub Frequently Asked Questions

- [Privacy Policy](#privacy-policy)
- [How to select a sub-range of URLs to include in an EPUB?](#url-subrange)
- [How to convert a new site using the Default Parser?](#default-parser)
- [How to see file downloading progress?](#download-progress)
- [Using Baka-Tsuki "Series Page" parser?](#baka-tsuki-series-page)
- [How to write a new Parser?](#write-parser)
- [Structure of WebToEpub code?](#code-structure)

<h2 id="privacy-policy">Privacy Policy</h2>
WebToEpub potentially exposes some of your web browsing history.
Specifically:
1. The EPUB files it creates record the URLs the content used to create the EPUB came from.  This is done so that if you wish to update the EPUB manually, you know where all the content came from.
2. When the Default Parser is used to create an EPUB, WebToEpub records the site's host name. This is so it can configure the Default Browser should you want to create another EPUB from the site.
3. As WebToEpub is hosted inside a Firefox or Chrome web browser, the browser itself (depending on your settings) may record your browsing history, or do anything else.

<h2 id="url-subrange">How to select a sub-range of URLs to include in an EPUB?</h2>
1. Click on the "Edit Chapter URLs" button.
2. Edit the hyperlinks. e.g. Delete the URLs that are not wanted. Note, you can copy and paste the hyperlinks to and from a text editor.
3. Click the "Apply Changes" button.

<h2 id="default-parser">How to convert a new site using the Default Parser?</h2>
ToDo

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

