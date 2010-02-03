Multi-column IBM Docs
=====================

This Greasemonkey script attempts to make adjustments to IBM's default Support Document view, the primary adjustment being multiple columns. In addition, the script cleans up ugly 1990's-style tables, and converts `<tt>` tags to `<pre>` tags. Lastly, this script attempts to fix several defects that I have found in individual pages. These are enumerated below.

Multi-column
------------

For now, this script uses Firefox's `-moz-column-count`, `-moz-column-rule`, and `-moz-column-gap` CSS attributes. This is good enough for me, because I use Firefox, and most Greasemonkey users are Firefox users. It may need to change in the future though, as people get creative and allow Greasemonkey scripts to be incorporated into IE, Safari, and Chrome.

IBM's current scheme sets the content of a support document to be 443 px. Considering the font-size they use in these docs, this is a pretty good width; very easy on the eyes, lines aren't too long, etc. However, it leaves miles of whitespace on my 23" monitor. I toyed around with using multiple-columns in Firebug, and liked the results. Based on the 443 px number, this script will give you 2 columns if you have 1246 pixels across visible in the browser, and 3 columns if you have 1709 pixels.

Now, I don't even have 1709 pixels across dedicated to the browser, so there is a "Hide Navigation" link in the upper right to help you out. Clicking this link will hide the relatively not-useful navigation bars along the left and right side of the page. The script then recalculates how many columns to yield. With the navigation bars gone, you only need 936 pixels across visible in the browser for 2 columns, and 1399 pixels for 3 columns. Much more accessible!

Sometimes the contents of a document will include an element that pushes the 443 px width limit (`<table>`'s, `<tt>`'s, etc.), so the adjustment won't work as planned. To disable, just click the Greasemonkey icon at the bottom of the browser to disable, and reload.

Adjustments
-----------

+   Lots of code examples are listed in `<tt>` tags, and stretch way too wide. I convert these into `<pre>` tags, and set the CSS to `overflow: auto;` so that you can just scroll to see all the code. [Example](http://www-01.ibm.com/support/docview.wss?rs=2044&uid=swg24024682).
+   These documents use 1990's-style tables like `<table border='2' cellpadding='2' cellspacing='2'>`. This really feels like an ancient relic. So I cleaned these up. I search for these with the XPath `"//table[@border>0]"`, so let me know if there are tables that this query misses. [Example](http://www-01.ibm.com/support/docview.wss?rs=2044&uid=swg24024682).
+   In order to squeeze tables, I rewrite some of the content:
    -   `<th>` with "ALL CAPS" are converted to "Sentence case".
    -   "Size (bytes)" columns are converted to KB if all values are greater than 1024, or MB if all values are greater than 1024*1024.

    I intend to change at least 3 more things in here... listed below. [Example](http://www-01.ibm.com/support/docview.wss?rs=2044&uid=swg24024682).

Fixes
-----

In trying to convert all `<table><tr><td>CONTENT</td></tr></table>` blocks to `<div>CONTENT</div>`, I ran across some weird code. Much of it likely due to automated HTML-izing the contents of some WYSIWYG editor. Not sure. Anyways, here are listed fixes. Please send me requests and examples of poorly-rendering documents.

+   Blocks of `<tt>...</tt><tt>...</tt><tt>...</tt>...` without line breaks. These really should just be one `<tt>`. So in converting to `<pre>`'s, I did so. Let me know if its ugly / broken anywhere.
+   `<th>` with "ALL CAPS" are converted to "Sentence case". These appeared randomly.
+   Some `<pre>` tags end up being the sole element in a `<ul>`, not even inside a `<li>` or anything. These are converted into `<div>`'s with `padding-left: 20px;`. [Example](http://www-01.ibm.com/support/docview.wss?rs=3352&context=SSDV2W&dc=D400&uid=swg24023138&loc=en_US&cs=UTF-8&lang=en&rss=ct3352rational).

Plans
-----

There are more fixes and squeezings that need to be done:

+   Convert any `<th>` tags that aren't in the 1st row to `<td>` tags.
+   Remove Download Director links.
+   Convert dates from `mm/dd/yyyy` to `mm/dd/yy`.
+   In converting `<tt>` to `<pre>` tags, I set the `<pre>` tags' width to always be 443. However, some of these tags are in (nested) lists. I need to find the calculated margin from the left of `#multi-column-div` and compensate. [Example](http://www-01.ibm.com/support/docview.wss?rs=3352&context=SSDV2W&dc=D400&uid=swg24023138&loc=en_US&cs=UTF-8&lang=en&rss=ct3352rational).