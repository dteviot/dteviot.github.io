# How to convert a new site using the Default Parser
Sometimes WebToEpub is unable to figure out which content on a web page should be packed into the EPUB.
When this happens, WebToEpub asks you to tell it which element on the web page has the content to pack.
You use the default parser page to tell WebToEpub how to find the content by telling WebToEpub the Cascading Style Sheet Selector (CSS Selector) for the element containing the wanted content.
(If you're not familiar with CSS Selectors, they're a shorthand notation for specifying elements on a HTML page.  Please see [https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors) for an excellent description of CSS selectors.)

As seen in this screenshot, 
<img src="https://dteviot.github.io/Projects/webToEpubImages/DefaultParserScreenshot.png">
the The Default Parser has 5 text inputs and 3 buttons.
In order, the inputs for the control are:
<ol>
<li><strong>Hostname</strong>  This is the hostname portion of the web site URL.  It's automatically filled in, so just ignore it.</li>
<li><strong>CSS Selector for the content</strong> This is where you tell WebToEpub the element that holds the content</li>
<li><strong>CSS Selector for Title of Chapter</strong> Sometimes the title of each chapter isn't in the same element that holds the rest of a chapter's text.  e.g. <a href="http://www.ironteethserial.com/dark-fantasy-story/story-interlude/prologue/">http://www.ironteethserial.com/dark-fantasy-story/story-interlude/prologue/</a>. When this happens, you can use this to tell WebToEpub which element holds the chapter's title and WebToEpub will include this element to the front of the text content that it fetches.  Obviously, this field can be left blank if this isn't an issue.</li>
<li><strong>CSS Selector for Elements to remove</strong> Sometimes the content contains things that are not wanted in the EPUB.  e.g. Advertisements, Share links, etc.  This input is used to say which elements are to be removed from the content before packing into the EPUB. Obviously, this field can be left blank if this isn't an issue.</li>
<li><strong>URL of test chapter</strong> If you'd like to test that the CSS Selectors you have provided work, then provide the URL of a chapter and click the "Test" button.  WebToEpub will fetch that chapter from the internet and run the CSS Selectors against the web page.  The resulting chapter that would appear in the EPUB will be shown below the test button.  Obviously, if you don't want to test your selector(s) this can be left blank.</li>
</ol>

The buttons are:
<ol>
<li><strong>Help</strong>  Brings up this web page.</li>
<li><strong>Test</strong> Will test the provided CSS Selectors against a chapter.</li>
<li><strong>Finished</strong> Tell WebToEpub you've finished configuring the CSS Selectors.</li>
</ol>


## Worked Example

Let's assume we want to convert <a href="http://www.ironteethserial.com/table-of-contents/">"The Iron Teeth"</a> into an EPUB.
Looking at the above page, we can see that the first chapter of this story is at <a href="http://www.ironteethserial.com/dark-fantasy-story/story-interlude/prologue/">http://www.ironteethserial.com/dark-fantasy-story/story-interlude/prologue/</a>

<ol>
<li>The first step is to discover the <a href="https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Getting_started">HTML element</a> that contains the content to put into each chapter of the EPUB.  To do this 
  <ol>
  <li>Open a chapter of the story (e.g. <a href="http://www.ironteethserial.com/dark-fantasy-story/story-interlude/prologue/">http://www.ironteethserial.com/dark-fantasy-story/story-interlude/prologue/</a>) in your web browser of choice.</li>
  <li>Open the browser's DOM Inspector.  E.g. On Firefox use <a href="https://developer.mozilla.org/en-US/docs/Tools/Add-ons/DOM_Inspector/Introduction_to_DOM_Inspector">CTRL+Shift+C</a>, on Chrome open <a href="https://developers.google.com/web/tools/chrome-devtools/inspect-styles/edit-dom">"Developer Tools" and select the "Elements" tab</a>. 
      On Chrome, this looks like <img src="https://dteviot.github.io/Projects/webToEpubImages/FindingContent.png"> </li>
  <li>Find the HTML element that encloses the entire text your want in the EPUB. in this case it's <strong>&lt;div class="post_content"&gt;</strong></li>
  <li>Figure out the CSS Selector for the element.  In this case it's a div element with a class, so the CSS Selector is <strong>div.post_content</strong></li>
  <li>Put this CSS Selector into the relevant input.</li>
  </ol>
</li>
<li>We can now test the CSS Selector to see if it works.  To do this:
  <ol>
  <li>Copy the URL of the chapter into the "Url of chapter to test" input</li>
  <li>Click the test button</li>
  <li>Examine the text that appears in the scroll box below the buttons.</li>
  </ol>
</li>
<li>You should now see that the chapter text has appeared, but it's missing the chapter title.  If you wish to add the title, 
  <ol>
  <li>Go back to the browser's DOM inspector and find the CSS Selector for the element holding the chapter  (in this case it's <strong>h2.post-title</strong></li>
  <li>Copy the CSS Selector into the "Title of Chapter" input</li>
  <li>Run the test again</li>
  </ol>
</li>
<li>If desired, a similar process can be used to remove any elements in the content that are unwanted.</li>
<li>When satisfied with the test results, click the "Finished" button.</li>
<li>You will now go to the usual "WebToEpub" page and you can continue as normal.</li>
</ol>

