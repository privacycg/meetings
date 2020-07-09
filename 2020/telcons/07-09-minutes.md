# Privacy CG telecon, 9 July 2020

[Agenda link](https://github.com/privacycg/meetings/blob/master/2020/telcons/07-09-agenda.md)

*   Chair: Tess
*   Scribe: Peter Saint-Andre

## Attendees:

1. Theresa O’Connor (Apple)
2. Peter Saint-Andre (Mozilla)
3. Anne van Kesteren (Mozilla)
4. Tanvi Vyas (Mozilla)
5. Kristen Chapman (Salesforce)
6. Przemylsaw Iwanczak
7. Sam Weiler (W3C/MIT)
8. Joel Odom (Salesforce)
9. John Sabella (PubMatic)
10. George London (Survata)
11. Ohtsu Shigeki (Yahoo JAPAN)
12. Pete Snyder (Brave)
13. Raj Belur
14. Paul Jenson
15. Brad Lassey (Google)
16. Ben Savage (Facebook)
17. Ian Truslove (White Ops)
18. Jack Frankland
19. Russell Stringham (Adobe)
20. Steven Valdez (Google)
21. Steven Englehardt (Mozilla)
22. Bill Densmore  | [ITEGA.ORG](https://www.itega.org/mission)
23. Brandon Maslen (Microsoft)
24. Christine Runnegar (PING co-chair)
25. Max Gendler (NY Times)
26. Martin Meyer (Akamai)
27. Jason Nutter (Microsoft)
28. Erik Anderson (Microsoft)
29. Paul Jensen (Google)
30. Sebastian Zimmeck (Wesleyan University)
31. Mike O’Neill (Baycloud)
32. Kate Cheney (Apple)
33. Lionel Basdevant (Criteo)
34. Kaustubha Govind (Google)
35. Marijn Kruisselbrink (Google)
36. Martin Robinson (Igalia)
37. John Wilander (Apple WebKit)
38. Arnaud Blanchard (Criteo)
39. Sean Harrison (Google)
40. Michael Kleber (Google Chrome)
41. Valentino Volonghi (NextRoll)
42. Devin Rousso (Apple WebKit)
43. Jeffrey Yasskin (Google Chrome)
44. Wendy Seltzer (W3C)
45. James Hartig (Admiral)
46. Paul Bannister (CafeMedia)
47. Kushal Dave (Scroll)
48. Christy Harris (Future of Privacy Forum)
49. Nicolas Arciniega (Microsoft)
50. Lesley Norton (Mozilla)
51. Dan Veditz (Mozilla)
52. Theodore Olsauskas-Warren (Google)
53. Mark Xue (Apple)
54. Chris Needham (BBC)
55. Chris Pedigo (DCN)
56. David Van Cleve (Google Chrome)mi
57. Wendell Baker (Verizon Media)
58. Shivani Sharma (Google)
59. Andy Sharkey (CafeMedia)
60. Arnaud Blanchard
61. Josh Karlin (Google)
62. Marshall Vale (Google)
63. Samuel D. Sousa (Triton Digital) 
64. Arthur Edelstein (Mozilla)
65. Bartlomiej Romanski
66. Mike Lerra (Google)
67. Pranjal Jumde (Brave)
68. Melanie Richards (Microsoft)
69. Cathie Chen (Igalia)
70. Chelsea Gong
71. Shaun Gilmore (Apple)
72. Chetna Bindra
73. Dave Harbage
74. gmcgrath
75. Jeff wieland
76. Joey Salazar (ARTICLE 19)
77. Shivan Sahib (Salesforce)
78. Michael Smith
79. Mingzhe Li
80. Niarci
81. Shaun Gilmore (Apple)
82. Soujanya
83. Vibha Sethi (Verizon Media)
84. Viraj
85. Eric Mwobobia (ARTICLE 19)
86. Jack J (Google)
87. Jeff Wieland

## Introductions

Jason Nutter says hi from Microsoft

## Work Items

### [Client-Side Storage Partitioning update](https://github.com/privacycg/storage-partitioning/) - Anne

*   **Anne van Kesteren**: First step towards formalizing partitioned state: [https://fetch.spec.whatwg.org/#http-cache-partitions](https://fetch.spec.whatwg.org/#http-cache-partitions)
*   Briefly, started formalizing partitioning states and first one landed in Fetch standard, HTTP cache is required to be partitioned by 1P sites
*   Chrome team interested in adding more keys
*   Mostly done by Shivani Sharma
*   Related infra work in HTML standard
*   In good position to generalize this (e.g., to networking states)
*   Doing a similar thing with various storage APIs
*   Grounding them into a common set of primitives (kind of a common store)
*   Key is still origin there
*   Idea is that we have a single place to update that key to origin + top-level site
*   PRs in place to update HTML spec, also IndexedDB
*   Prelim work on shared workers, broadcast channel
*   Next step is to tie this into Storage Access API
*   Also better explain how clearsite data
*   **John Wilander**: we recently published comprehensive docs on WebKit tracking protection [https://webkit.org/tracking-prevention/](https://webkit.org/tracking-prevention/) , including partitioning - for LocalStorage we make storage not only partitioned but ephemeral; we could include this as optional for browsers, or a special version of partitioning
*   **Anne**: do you apply that to IndexedDB and Service Workers etc.?
*   **John**: no, this came about organically (e.g., Service Workers are not ephemeral)
*   **Mike O'Neill**: Is there discussion about expiration times etc.? Having a general API would be good
*   **Anne**: clearing yes, automated expiry being discussed but nothing concrete yet; those use cases are known, just figuring out how to best approach them
*   **Michael Kleber**: +1 to what Mike O'Neill just said; there's a lot of benefit to having not just max possible lifetime but also lifetime controllable by the person setting the value (just as we have for cookies); in talking with users of cookies, they are forced to choose between cookies and local storage, but only with cookies are they given control over lifetime and this can be legally desirable in some situations
*   **Anne**: issue is at [https://github.com/whatwg/storage/issues/11](https://github.com/whatwg/storage/issues/11)
*   Anne: there's ongoing discussion on this topic, post there if you have feedback

### [Storage Access API integration issues](https://github.com/privacycg/storage-access/issues?q=is%3Aissue+is%3Aopen+label%3Aagenda%2B) - Tess

**Tess**: trying to gauge implementer interest on each of these possible integrations.

#### [API Integration with Permissions Policy (formerly Feature Policy) #12](https://github.com/privacycg/storage-access/issues/12) (See also [Provide mechanism for nested iframes to request storage access #10](https://github.com/privacycg/storage-access/issues/10))

*   **Tess**: Potential upside is having a coherent story about how to enable or disable in nested iframes, for instance
*   Just saw announcement of Microsoft's implementation of storage access API, which is great
*   **Anne**: Mozilla is supportive
*   **Brandon**: in Edge we do support permissions policy, so we already do that
*   **John Wilander**: I don't have an opinion yet, need to read more about permissions policy, we think it's definitely the third party asking to do something not the first party, which this is compatible with; sounds pretty good to me

#### [Integrate with the Permissions API or its model? #32](https://github.com/privacycg/storage-access/issues/32)

*   **Tess**: Mechanism to request permission and standardized hooks about where and when to prompt
*   Currently have abstractions in the spec, permissions API would rationalize all of that
*   **Mike O'Neill**: in relation to storage access API, if origin doesn't know if it has access to its 1P cookies, it would be nice to know if something like ITP is in effect, perhaps feature policy to request the highest level of privacy
*   **Tess**: if I hear you correctly, storage access API iframe can determine if it has storage access...
*   **Mike**: perhaps a header that enables origin to know or set that it doesn't have access; other aspect is that feature policy would be at the top level and apply to embedded third parties
*   **Tess**: this would be API call specifying that embedded resources need to use storage access API
*   **Christy**: with prompt to users, is there rate limiting or other mechanism that would prevent over-prompting?
*   **Tess**: is that question about permissions API or storage access API?
*   **Christy**: permissions
*   **Tess**: not sure about permissions API but hopefully the authors have done their homework; there are plenty of affordances for this in the storage access API
*   **Christy**: is this up to the publisher or at the discretion of the third party requesting?
*   **Tess**: the user agent does the throttling
*   **Dan Veditz:** permissions API doesn't have a model for 3P request, this is up to the page so it only comes up once; permissions can be delegated from the top-level document down, so requests come from the top-level page
*   **Anne**: first, permissions policy and API should be as much in line as possible (e.g., for geolocation), I'd expect the same symmetry here; second, what happens if top-level revokes or denies storage access? you can do this in permissions, and it's not clear to me what the model should be
*   **Tess**: you could get into inconsistent state on the page?
*   **Anne**: weird thing would be for first parties
*   **Tess**: so for instance iframes that are same origin?
*   **Anne**: even the top level - it has permission by default but it could revoke it for itself
*   **Tess**: we'd have to rethink it in this case
*   **Anne**: it might be OK to ignore it
*   **John Wilander**: my interpretation of Christy's question is slightly different - I think it's about the storage access API prompt, this might differ between user agents; we've been shipping this in webkit for 2 years, so we have experience; [john broke up and then sped up]… rules out 95% ... ; needs to be interaction with 3P content; we have seen misuse of API when user says don't allow, but resource keeps requesting; the second time the user says don't allow we don't show the prompt again for that page, but we don't extend that across sites
*   **Tess**: in the spec, there's a flag wasExpresslyDenied - never prompt again if that flag is set
*   **John**: we might need to revisit that, because we realized there might need to be an escape hatch because the user might not like the results of denying 
*   **Tess**: I’ll a file follow-up issue
*   **Jeffrey Yasskin**: model of storage policy where you ask about particular iframe within top level site doesn't fit the permissions model, we might need to modify the model to address this use case; might also look at [Document Policy](https://w3c.github.io/webappsec-feature-policy/document-policy.html), which has a different model than permissions policy
*   **Anne**: I'd recommend looking at the issue - we sketched this out, we're delegating ability to ask for permission, not delegating permission itself
*   **Jeffrey**: that probably fits it in permissions policy but not the permissions spec: permissions spec has "allow", "prompt", "deny" states, and you're not going to have a "prompt" state for permission-to-prompt; I suspect permission spec needs some changes
*   **Vibha Sethi**: will Chrome move to permissions policy?
*   **John**: for Safari, we've mentioned it in the spec and intention is to support per-iframe sandbox permission, not sure if moving it to permissions solves all the cases, but I think there needs a way to forbid embedded 3P content from prompting users
*   **Brandon**: in Chromium we support permissions policy and permissions integration, here again it's as Anne said; we support iframe token as well
*   **Brandon**: to add to Christy's question, in Chromium by integrating with permissions pipeline we get quieter notifications and coalescing of prompts; e.g., can't prompt again for 7 days, we get those for free because it's a permission under the hood
*   **Steve Englehardt**: following up on John's question, our prompting rules are similar to Safari with a few exceptions, more parties would be able to request storage access if there was 1P interaction; also don't have two-prompt allowance; we also allow access without storage access prompt if limited to less than 5 sites
*   **Josh Karlin**: we'd like to avoid a prompt, perhaps isolated frame where this would happen, thinking about a requestStorageAccess API there, but a regular iframe wouldn't be able to call it

#### [API Integration with Reporting API #33](https://github.com/privacycg/storage-access/issues/33)

*   **Tess**: do implementers support this, would you like to support storage access via reporting API, etc.?
*   **Pete Snyder**: right now we disable reporting API in Brave
*   **Anne**: it would be good to know if there is developer demand
*   **Brandon**: this was provided as feedback when implementing in Chromium to raise the issue and find out if developers are interested in this
*   **Kris Chapman**: yes, this would be useful, the more information we can get back the better
*   **Jeff Wieland**: we've done discovery work to persist user's opt-out preferences across domains, is that a valid use case for storage access API?

### Proposals

#### [Top-Frame lifetime, partitioned storage for embedded frames](https://github.com/privacycg/proposals/issues/18) - Pete

*   **Pete Snyder**: In Brave we block all 3P storage. Two issues:
    *   There's a web compat hit
    *   We have some exceptions where the hit is bad
*   Trying to address both of those problems
*   Proposal is ephemeral storage by default, if you navigate away etc. then it goes away
*   Partitioned storage
*   Different under different pages
*   Is there interest as an alternative to allowing or blocking storage?
*   **John Wilander**: this ties into storage partitioning and making it ephemeral; for us that's limited to the browser session, but in general we're interested in exploring how to make partitioned storage ephemeral
*   **Steve Englehardt**: is key the URL or the document? for escape valve, is that permanent or ephemeral?
*   **Pete**: we're in early days, feedback is useful; right now partitioned at the frame level; escape valve is the same as how storage access works in Firefox or Edge
*   **Michael Kleber**: we're also interested in ephemeral nature of access/data - we've thought of several options, one is session lifetime that is something like "as long as there's a tab open in the browser" then there's still a live visit to the site (perhaps also with an upper bound); this has some nice properties, e.g., prefer to avoid strong differentiation between classical website with navigation across pages vs. single-page app; other extreme is keep storage until browser is restarted, but that seems too long nowadays; we are interested in something in between
*   **Kris**: we'd be interested in ephemeral storage but make it more like cookies where lifetime could be set and not automatically session-based (e.g., consider a chat history where it's good to bring up old state to restart the interaction - we'd prefer not to keep that on the server because that could be a privacy concern)
* **Pete**: just going to say that we’re doing a study of the webcompat / privacy effect on the web over the next two months, and will share those results here when we have them

#### [Bounce Tracking Protection next steps](https://github.com/privacycg/proposals/issues/6#issuecomment-654501839) - John

*   **John**: Brief update, see summary ^
*   3 potential next steps listed there
*   Would people support taking one or more of those next steps?

### Any other business

*   [Upcoming WebAppSec meeting](https://github.com/w3c/webappsec/blob/master/meetings/2020/2020-07-21-agenda.md) is on 21 July Tuesday 11am PST.
    *   Referrer Policy topics would be of interest to this group - Dan Veditz
*   Feedback on using this Google Doc instead of Cryptpad for (a) notes (b) queue
    *   Scribe says this was easier than Cryptpad
    *   +1 from participants re queueing
