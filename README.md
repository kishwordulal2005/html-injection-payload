# html-injection-payload


HTML Injection / Fun HTML Tag Testing Notes

1. Initial question: reflected in three places

The input appeared to be reflected in multiple locations on the page, including the page title/header, a suggestion area, and a search link.

Important conclusion:

Reflection alone does not automatically prove exploitable HTML injection.

The key test is whether the browser interprets the HTML tag or displays the literal characters.

Escaped example

&lt;u&gt;unknone&lt;/u&gt;

If it appears like this, the input is escaped.

Interpreted example

<u>unknone</u>

If the browser actually renders the underline, HTML is being interpreted at that reflection point.

2. Early harmless verification tags

Simple tests suggested for checking whether formatting changes are visible:

<b>test</b>
<i>test</i>
<u>test</u>
<h1>test</h1>

A quick way to inspect the result is also through browser Developer Tools.

If the DOM contains:

&lt;h1&gt;TEST123&lt;/h1&gt;

the value is escaped.

If the DOM contains:

<h1>TEST123</h1>

and it renders as a heading, that confirms HTML rendering.

3. Tags with obvious visual changes

Text formatting

<b>Bold</b>
<i>Italic</i>
<u>Underline</u>
<s>Strike</s>
<del>Deleted</del>
<ins>Inserted</ins>
<mark>Highlighted</mark>
<small>Small</small>
<big>Big</big>
<sup>Raised</sup>
<sub>Lowered</sub>

Layout and structure

<br>
<hr>
<h1>Heading</h1>
<blockquote>Quoted text</blockquote>
<pre>Preserved text</pre>
<center>Centered text</center>

Code-like appearance

<code>code</code>
<kbd>CTRL+C</kbd>
<tt>terminal style</tt>

Other text tags

<em>Emphasis</em>
<strong>Strong</strong>
<q>Quote</q>
<cite>Citation</cite>
<small>Small</small>

4. Discussion about <s>

The <s> tag was identified as a particularly good visual test because the strike-through effect is immediately noticeable.

Examples:

<s>unknone</s>

<del>unknone</del>

Good quick visual tests include:

<mark>unknone</mark>
<s>unknone</s>
<h1>unknone</h1>
<kbd>unknone</kbd>
<blockquote>unknone</blockquote>

5. Making each letter move or jump

A single standard HTML formatting tag cannot normally animate every character independently.

Effects where individual letters move one by one generally require:

CSS animation

JavaScript

Separate elements around individual characters

A classic deprecated option can move the entire text:

<marquee>unknone</marquee>

That moves the whole word, not each letter independently.

6. <marquee> experiments

Basic example:

<marquee>unknone</marquee>

Bouncing behavior:

<marquee behavior="alternate">unknone</marquee>

This was considered one of the funniest immediately visible effects because it can make the text bounce.

Examples:

<marquee behavior="alternate">🚀 UNKNONE 🚀</marquee>

<marquee behavior="alternate">💀 HA HA HA 💀</marquee>

Note: <marquee> is obsolete HTML and browser support is legacy/non-standard.

7. Attempt to change text color

An attempted form was:

<marquee>font="red"=(unknone)</marquee>

That does not use valid HTML syntax for changing the color.

The old HTML syntax is:

<font color="red">unknone</font>

Combined with marquee:

<marquee><font color="red">unknone</font></marquee>

If the text still appears green, possible explanations include:

The page's CSS is overriding the color.

The site sanitizes or modifies the <font> element or its attributes.

The surrounding element has a stronger color rule.

CSS rules using !important can override weaker styling.

The fact that a visual tag works is useful evidence of HTML rendering, but each tag and attribute can be filtered differently.

8. Old-school / funny tags discussed

<marquee>Hello</marquee>
<blink>Hello</blink>
<center>Hello</center>
<strike>Hello</strike>
<tt>Hello</tt>
<nobr>Hello Hello Hello</nobr>

Some old or obsolete tags may behave differently depending on the browser.

Other legacy examples mentioned:

<big>Hello</big>
<xmp><h1>LOL</h1></xmp>
<plaintext>

These are historical/obsolete and are mainly interesting for learning about older HTML behavior.

9. Single-tag family requested

The goal became finding tags similar to:

<b>
<i>
<u>
<s>
<br>

Compact list

<b>Bold</b>
<i>Italic</i>
<u>Underline</u>
<s>Strike</s>
<em>Emphasis</em>
<strong>Strong</strong>
<small>Small</small>
<sub>Sub</sub>
<sup>Sup</sup>
<mark>Mark</mark>
<code>Code</code>
<kbd>Kbd</kbd>
<q>Quote</q>
<cite>Cite</cite>
<del>Delete</del>
<ins>Insert</ins>
<pre>Pre</pre>
<tt>TT</tt>
<big>Big</big>

Void/simple tags:

<br>
<hr>
<wbr>

10. Best quick visual checks

For immediately noticeable harmless formatting changes:

<s>unknone</s>

<mark>unknone</mark>

<big>unknone</big>

<small>unknone</small>

<sup>unknone</sup>

<sub>unknone</sub>

<code>unknone</code>

<kbd>unknone</kbd>

<blockquote>unknone</blockquote>

<h1>unknone</h1>

<center>unknone</center>

<marquee>unknone</marquee>

11. Key takeaway for HTML injection testing

A visible reflection is not enough to conclude that a vulnerability exists.

A safer way to verify HTML interpretation is to use harmless formatting and structural tags and then inspect the rendered DOM.

Possible outcomes:

Literal text appears
Example: <s>unknone</s> is visible exactly as typed.
→ The input is likely escaped.

Formatting is rendered
Example: unknone visibly has a strike-through.
→ HTML is being interpreted at that location.

Some tags work but others do not
→ A sanitizer, allowlist, or browser behavior may be filtering different elements/attributes.

For responsible testing, use systems you own or have explicit permission to test, and prefer harmless visual payloads while determining how the input is handled.
