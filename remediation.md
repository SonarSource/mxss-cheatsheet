---
layout: page
title: Remediation
---

There are many ways developers can consider remediating this type of vulnerability. Understanding the fundamentals of this attack alongside the application needs, can help devs make a better choice when it comes to remediation.

We tried to separate different approaches we saw throughout our research of remediation taken by applications and sanitizers. 

#### Sanitize client side
- This is probably the **most important rule to follow**. Using sanitizers that run on the client side, such as [DOMPurify](https://github.com/cure53/DOMPurify), avoids parser differentials risk. Due to the complexity of parsing and most likely serving content to different parsers (Firefox vs Chrome vs Safari etc...), it is impossible to avoid differentials when HTML is parsed not in the same place where the content is eventually rendered. For that reason, server-side sanitizers are prone to fail.
- When using Server-Side Rendering (SSR) with a client-side JS framework, it can be easy to drop in libraries like [isomorphic-dompurify](https://www.npmjs.com/package/isomorphic-dompurify). They let client-side sanitizers like [DOMPurify](https://github.com/cure53/DOMPurify) "just work" in SSR mode. But to achieve this, they also introduce a server-side HTML parser like [jsdom](https://www.npmjs.com/package/jsdom), which introduces parser differential risks. The safest option for web apps using SSR is to disable SSR for user-controlled HTML and defer the sanitization and rendering to the client-side only.

#### Don't reparse
- In order to avoid "Round trip mXSS" the application can insert the sanitized DOM tree directly into the document unlike serializing and re-rendering the content. **Note** that this approach can be done only when the sanitizers are implemented on the client side and might cause unexpected behaviors (such as rendering content differently due to not adapting to the context of the page)

#### Always encode or delete raw content 
- Because the idea of mXSS is to figure out a way for a malicious string to be rendered as raw text in the sanitizer but parsed as HTML later, not allowing/encoding any raw text in the sanitizer stage would make it impossible to re-render it as HTML.\
    **Note** that this could break some things such as CSS code

#### Not supporting foreign content elements
- Not supporting foreign content elements (**deleting svg/math elements and their content not renaming**) in your sanitizers reduces complexity significantly.\
    **Note** this doesn't mitigate mXSS but offers a precaution step.
