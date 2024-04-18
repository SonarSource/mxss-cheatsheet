---
layout: page
title: mXSS Explained
---

Mutation Cross-Site Scripting (mXSS) is a sophisticated variation of the well-known Cross-Site Scripting (XSS) vulnerability. When an application needs to safely render the user’s input as HTML, to support some HTML features, sanitization would be the solution. Allowing specific tags and attributes while stripping or encoding others. Unfortunately, this is not a straightforward task since HTML is a syntax-tolerant language that might change or “mutate” when parsing. mXSS takes advantage of that by providing a payload that seems innocent initially when parsing (during the sanitization process) but mutates it to a malicious one when re-parsing it (in the final stage of displaying the content).
The abstract idea is to figure out a way for a malicious string containing an XSS vector to be rendered as raw text in the sanitizer but parsed as HTML when passed to the browser. There are several ways that could cause this behavior:

## Round trip mXSS

mXSS round trips occur due to the fact that HTML content might change if reparsed. As written in the [HTML spec](https://html.spec.whatwg.org/multipage/parsing.html#serialising-html-fragments): “It is possible that the output of this algorithm if parsed with an HTML parser, will not return the original tree structure. Tree structures that do not roundtrip a serialize and reparse step can also be produced by the HTML parser itself, although such cases are typically non-conforming.”

Using a `form` element, we can demonstrate this by the following example:

A `form` element cannot have another `form` nested inside of it ***“Content model: Flow content, but with no form element descendants.“***
As demonstrated in the [HTML spec](https://html.spec.whatwg.org/multipage/parsing.html#serialising-html-fragments) the following string will initially create a DOM tree with nested forms. But when serialized and reparsed the nested form will get omitted:
```html
<form id="outer"><div></form><form id="inner"><input>

html
├── head
└── body
    └── form id="outer"
        └── div
            ├── form id="inner"
            └── input
```

The `input` element will be associated with the inner `form` element. Now, if this tree structure is serialized and reparsed, the `<form id="inner">` start tag will be ignored, and so the `input` element will be associated with the outer `form` element instead.
```html
<html><head></head><body><form id="outer"><div><form id="inner"><input></form></div></form></body></html>

html
├── head
└── body
    └── form id="outer"
        └── div
            └── input
```

## Parsing differential mXSS
This type of mXSS takes advantage of a mismatch between the sanitizer’s parsing algorithm vs the renderer's (e.g. browser) one. 

Let’s take for example the [noscript](https://html.spec.whatwg.org/multipage/scripting.html#the-noscript-element) element, the parsing rule for it is: “If the [scripting flag](https://html.spec.whatwg.org/multipage/parsing.html#scripting-flag) is enabled, switch the tokenizer to the [RAWTEXT state](https://html.spec.whatwg.org/multipage/parsing.html#rawtext-state). Otherwise, leave the tokenizer in the [data state](https://html.spec.whatwg.org/multipage/parsing.html#data-state).” ([link](https://html.spec.whatwg.org/multipage/parsing.html#parsing-html-fragments))
Meaning, that depending on whether JavaScript is disabled or enabled the body of the `noscript` element is rendered differently. It is logical that JavaScript would not be enabled in the sanitizer stage but will be in the renderer. This behavior is not wrong by definition, but could cause bypasses as such:
`<noscript><style></noscript><img src=x onerror=”alert(1)”>`

There is potential for many other parser differentials that might occur, such as different HTML versions, content type mismatch, and more…

## Desanitization
Desanitization is a crucial mistake made by applications when interfering with the sanitizer’s output before sending it to the client, essentially undoing the work of the sanitizer. Any small change in the markup might cause different behavior of the parser resulting in a bypass of the sanitization. We've discussed this issue before in several blog posts, where we identified vulnerabilities in various applications, including:

* [Pitfalls of Desanitization: Leaking Customer Data from osTicket](https://www.sonarsource.com/blog/pitfalls-of-desanitization-leaking-customer-data-from-osticket/)
* [Reply to calc: The Attack Chain to Compromise Mailspring](https://www.sonarsource.com/blog/reply-to-calc-the-attack-chain-to-compromise-mailspring)
* [Code Vulnerabilities Put Proton Mails at Risk](https://www.sonarsource.com/blog/code-vulnerabilities-leak-emails-in-proton-mail/)
* [Remote Code Execution in Tutanota Desktop due to Code Flaw](https://www.sonarsource.com/blog/remote-code-execution-in-tutanota-desktop-due-to-code-flaw/)
* [Code Vulnerabilities Put Skiff Emails at Risk](https://www.sonarsource.com/blog/code-vulnerabilities-put-skiff-emails-at-risk/)

## Context-dependent
HTML parsing is complex and can be different depending on the context, for example parsing a whole document vs parsing a fragment is different in Firefox (see *Browser Specific* section in the main page). Dealing with the change from sanitizing to rendering in the browser, developers might mistakenly change the context in which the data is rendered causing parsing differential and eventually bypassing the sanitizer. Modern sanitizers are not aware of the context in which the result will be put, this aimed to be solved when browsers implement built-in sanitizer.
