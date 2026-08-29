HTML Tag Testing Notes

GitHub-safe version: All HTML examples inside code blocks are escaped so GitHub should display the tags as text instead of interpreting them.

HTML Injection / Fun HTML Tag Testing Notes

1. Initial question: reflected in three places

The input appeared to be reflected in multiple locations on the page, including the page title/header, a suggestion area, and a search link.

Important conclusion:

Reflection alone does not automatically prove exploitable HTML injection.

The key test is whether the browser interprets the HTML tag or displays the literal characters.

Escaped example

&amp;lt;u&amp;gt;unknone&amp;lt;/u&amp;gt;```

If it appears like this, the input is escaped.

### Interpreted example

```html
&lt;u&gt;unknone&lt;/u&gt;```

If the browser actually renders the underline, HTML is being interpreted at that reflection point.

---

## 2. Early harmless verification tags

Simple tests suggested for checking whether formatting changes are visible:

```html
&lt;b&gt;test&lt;/b&gt;
&lt;i&gt;test&lt;/i&gt;
&lt;u&gt;test&lt;/u&gt;
&lt;h1&gt;test&lt;/h1&gt;```

A quick way to inspect the result is also through browser Developer Tools.

If the DOM contains:

```html
&amp;lt;h1&amp;gt;TEST123&amp;lt;/h1&amp;gt;```

the value is escaped.

If the DOM contains:

```html
&lt;h1&gt;TEST123&lt;/h1&gt;```

and it renders as a heading, that confirms HTML rendering.

---

## 3. Tags with obvious visual changes

### Text formatting

```html
&lt;b&gt;Bold&lt;/b&gt;
&lt;i&gt;Italic&lt;/i&gt;
&lt;u&gt;Underline&lt;/u&gt;
&lt;s&gt;Strike&lt;/s&gt;
&lt;del&gt;Deleted&lt;/del&gt;
&lt;ins&gt;Inserted&lt;/ins&gt;
&lt;mark&gt;Highlighted&lt;/mark&gt;
&lt;small&gt;Small&lt;/small&gt;
&lt;big&gt;Big&lt;/big&gt;
&lt;sup&gt;Raised&lt;/sup&gt;
&lt;sub&gt;Lowered&lt;/sub&gt;```

### Layout and structure

```html
&lt;br&gt;
&lt;hr&gt;
&lt;h1&gt;Heading&lt;/h1&gt;
&lt;blockquote&gt;Quoted text&lt;/blockquote&gt;
&lt;pre&gt;Preserved text&lt;/pre&gt;
&lt;center&gt;Centered text&lt;/center&gt;```

### Code-like appearance

```html
&lt;code&gt;code&lt;/code&gt;
&lt;kbd&gt;CTRL+C&lt;/kbd&gt;
&lt;tt&gt;terminal style&lt;/tt&gt;```

### Other text tags

```html
&lt;em&gt;Emphasis&lt;/em&gt;
&lt;strong&gt;Strong&lt;/strong&gt;
&lt;q&gt;Quote&lt;/q&gt;
&lt;cite&gt;Citation&lt;/cite&gt;
&lt;small&gt;Small&lt;/small&gt;```

---

## 4. Discussion about `&lt;s&gt;`

The `&lt;s&gt;` tag was identified as a particularly good visual test because the strike-through effect is immediately noticeable.

Examples:

```html
&lt;s&gt;unknone&lt;/s&gt;```

```html
&lt;del&gt;unknone&lt;/del&gt;```

Good quick visual tests include:

```html
&lt;mark&gt;unknone&lt;/mark&gt;
&lt;s&gt;unknone&lt;/s&gt;
&lt;h1&gt;unknone&lt;/h1&gt;
&lt;kbd&gt;unknone&lt;/kbd&gt;
&lt;blockquote&gt;unknone&lt;/blockquote&gt;```

---

## 5. Making each letter move or jump

A single standard HTML formatting tag cannot normally animate every character independently.

Effects where individual letters move one by one generally require:

- CSS animation
- JavaScript
- Separate elements around individual characters

A classic deprecated option can move the entire text:

```html
&lt;marquee&gt;unknone&lt;/marquee&gt;```

That moves the whole word, not each letter independently.

---

## 6. `<marquee>` experiments

Basic example:

```html
&lt;marquee&gt;unknone&lt;/marquee&gt;```

Bouncing behavior:

```html
&lt;marquee behavior="alternate"&gt;unknone&lt;/marquee&gt;```

This was considered one of the funniest immediately visible effects because it can make the text bounce.

Examples:

```html
&lt;marquee behavior="alternate"&gt;🚀 UNKNONE 🚀&lt;/marquee&gt;```

```html
&lt;marquee behavior="alternate"&gt;💀 HA HA HA 💀&lt;/marquee&gt;```

Note: `<marquee>` is obsolete HTML and browser support is legacy/non-standard.

---

## 7. Attempt to change text color

An attempted form was:

```html
&lt;marquee&gt;font="red"=(unknone)&lt;/marquee&gt;```

That does not use valid HTML syntax for changing the color.

The old HTML syntax is:

```html
&lt;font color="red"&gt;unknone&lt;/font&gt;```

Combined with marquee:

```html
&lt;marquee&gt;&lt;font color="red"&gt;unknone&lt;/font&gt;&lt;/marquee&gt;```

If the text still appears green, possible explanations include:

- The page's CSS is overriding the color.
- The site sanitizes or modifies the `&lt;font&gt;` element or its attributes.
- The surrounding element has a stronger color rule.
- CSS rules using `!important` can override weaker styling.

The fact that a visual tag works is useful evidence of HTML rendering, but each tag and attribute can be filtered differently.

---

## 8. Old-school / funny tags discussed

```html
&lt;marquee&gt;Hello&lt;/marquee&gt;
&lt;blink&gt;Hello&lt;/blink&gt;
&lt;center&gt;Hello&lt;/center&gt;
&lt;strike&gt;Hello&lt;/strike&gt;
&lt;tt&gt;Hello&lt;/tt&gt;
&lt;nobr&gt;Hello Hello Hello&lt;/nobr&gt;```

Some old or obsolete tags may behave differently depending on the browser.

Other legacy examples mentioned:

```html
&lt;big&gt;Hello&lt;/big&gt;
&lt;xmp&gt;&lt;h1&gt;LOL&lt;/h1&gt;&lt;/xmp&gt;
&lt;plaintext&gt;```

These are historical/obsolete and are mainly interesting for learning about older HTML behavior.

---

## 9. Single-tag family requested

The goal became finding tags similar to:

```html
&lt;b&gt;
&lt;i&gt;
&lt;u&gt;
&lt;s&gt;
&lt;br&gt;```

### Compact list

```html
&lt;b&gt;Bold&lt;/b&gt;
&lt;i&gt;Italic&lt;/i&gt;
&lt;u&gt;Underline&lt;/u&gt;
&lt;s&gt;Strike&lt;/s&gt;
&lt;em&gt;Emphasis&lt;/em&gt;
&lt;strong&gt;Strong&lt;/strong&gt;
&lt;small&gt;Small&lt;/small&gt;
&lt;sub&gt;Sub&lt;/sub&gt;
&lt;sup&gt;Sup&lt;/sup&gt;
&lt;mark&gt;Mark&lt;/mark&gt;
&lt;code&gt;Code&lt;/code&gt;
&lt;kbd&gt;Kbd&lt;/kbd&gt;
&lt;q&gt;Quote&lt;/q&gt;
&lt;cite&gt;Cite&lt;/cite&gt;
&lt;del&gt;Delete&lt;/del&gt;
&lt;ins&gt;Insert&lt;/ins&gt;
&lt;pre&gt;Pre&lt;/pre&gt;
&lt;tt&gt;TT&lt;/tt&gt;
&lt;big&gt;Big&lt;/big&gt;```

Void/simple tags:

```html
&lt;br&gt;
&lt;hr&gt;
&lt;wbr&gt;```

---

## 10. Best quick visual checks

For immediately noticeable harmless formatting changes:

```html
&lt;s&gt;unknone&lt;/s&gt;```

```html
&lt;mark&gt;unknone&lt;/mark&gt;```

```html
&lt;big&gt;unknone&lt;/big&gt;```

```html
&lt;small&gt;unknone&lt;/small&gt;```

```html
&lt;sup&gt;unknone&lt;/sup&gt;```

```html
&lt;sub&gt;unknone&lt;/sub&gt;```

```html
&lt;code&gt;unknone&lt;/code&gt;```

```html
&lt;kbd&gt;unknone&lt;/kbd&gt;```

```html
&lt;blockquote&gt;unknone&lt;/blockquote&gt;```

```html
&lt;h1&gt;unknone&lt;/h1&gt;```

```html
&lt;center&gt;unknone&lt;/center&gt;```

```html
&lt;marquee&gt;unknone&lt;/marquee&gt;```

---

## 11. Key takeaway for HTML injection testing

A visible reflection is not enough to conclude that a vulnerability exists.

A safer way to verify HTML interpretation is to use harmless formatting and structural tags and then inspect the rendered DOM.

Possible outcomes:

1. **Literal text appears**  
   Example: `&lt;s&gt;unknone&lt;/s&gt;` is visible exactly as typed.  
   → The input is likely escaped.

2. **Formatting is rendered**  
   Example: `unknone` visibly has a strike-through.  
   → HTML is being interpreted at that location.

3. **Some tags work but others do not**  
   → A sanitizer, allowlist, or browser behavior may be filtering different elements/attributes.

For responsible testing, use systems you own or have explicit permission to test, and prefer harmless visual payloads while determining how the input is handled.
