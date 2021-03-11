# Privacy CG Teleconference, 11 March



*   Chair: Erik
*   Scribe: Brendan Riordan-Butterworth (1st half), Ben Savage (2nd half)

## [Private Click Measurement](https://github.com/privacycg/private-click-measurement)


### [Enable the advertiser/merchant to choose not to send attribution reports to click sources #65](https://github.com/privacycg/private-click-measurement/issues/65)

John Wilander (JW): There are some comments.  Directly to me.  Don’t want to be forced to share campaign performance with other parties, like competition.  This is “our data about our business”, we want the option to keep the data to myself.  

JW: Technically, some type of option in a pixel request (query string param) to say “don’t schedule report to click source”, or similar functionality in JS API.

Paul Bannister (PB): Say out loud, summary: I understand the advertiser perspective, this is broadly how things work today.  Standard pubs don’t get the attribution and click data.  This perpetuates the problem, using John’s analogies.

PB: Advertisers called “EMAIL” and publisher called “POPSEARCH”, there might be concerns about the POPSEARCH wanting to use the EMAIL stuff.

PB: But the larger platforms can use their large scale to force different terms. While smaller pubs can’t get this.  Though “small” is still quite large, just not as big as large platforms.

PB: This feature might limit equity across

Kristen Chapman (KC): Assumption - It is a publisher, who has been sold an ad space.  But that might not be right?  If there’s a GMail click in email, who gets the click, Google or the party who sent the email?  Salesforce offers opportunity to host sites, but the report should go to the client, not the host, right?

KC: It is about publishers, but it’s about getting the data to the sites rather than the platform.

JW: This has not been discussed in the issue.  I think this is why we’re trying to use slightly abstract terms.  There’s a separate discussion, should 3rd parties be allowed to be the reporting endpoint?  The Etsy issue from Facebook.  Report goes to the Etsy platform, rather than the vendor.

JW: This is about 2 things, where the click happened and where the attribution event gets sent to.  Advertisers don’t want to send the attribution stuff to click sources.  

KC: I get that, but it’s not all the same thing in the first place, anyway.  

JW: Yes, at least 2 types in that issue, huge search engine, mid-sized publisher.

Ben Savage (BS): Really interesting issue, no philosophical issue.  Advertisers can optimize for clicks OR conversions on Facebook.  If they don’t want to share, they don’t enable conversions.  

BS: The feature might allow the advertiser to get some insight into conversions even if they don’t want to share.  So that some aggregate insights might be achievable, though turning it off (of their own prerogative) might get some useful insight.  

JW: This is more of a business decision, agreed.  There might be scenarios where there is value from sharing, there might be some scenarios where there isn’t.  There’s always the ability to share offline, removing some data that they don’t want to share.  

JW: Going to PB points, it’s a tradeoff, but the advertiser really is the one that makes the decision.

BS: Some advertisers are not allowed to send data to FB

JW: Some advertisers cannot use PCM without this functionality.

Brian May (BM): Another example of why turning it off is good would be affiliate links, so that all comers (the affiliates) don’t know what happens afterwards.  


### [Offer same-site pixel "API" as alternative to a JavaScript API #71](https://github.com/privacycg/private-click-measurement/issues/71)

JW: Issue #71 - Pixel vs JS API.  Kristen may have brought up that some individuals, companies are reluctant for pixels and really anti JS API.  We’ve said that we are uncertain about when we want to remove the pixel.  There are functions that are supported in the JS API, like wildcard, that might not be available in pixel, where 100s of pixels might be needed.

JW: JS API offers more granularity.

JW: But a wildcard, same-site, pixel based API might offer alternatives.  Tho it’s tricky, because it’s a pixel, so it’s a bit harder.  

Anne van Kesteren (AK): Why does it have to be a request? 

JW: Would a meta tag or other way than JS be possible?  Open to the possibilities.  

AK: Adding comments to the issue.

Dan Appelquist (DA): It feels to me that the whole point is to put tech in place that supports advertising, as opposed to the old way which subverts the intended use.  Does this go back in time?

JW: I think this thing, even if I don’t like the implementation so far, removes the need of 3rd party, actually good, same site.  We’re trying to handle the reluctance to adopt third party code.  

DA: There is the challenge of defining what a 3rd party is with upcoming API.  

Kristen Chapman: Some ads simply won’t accept JS.  Either due to restrictions (email clients), or policy (on websites).  For the idea to support JS, but there are situations where you can’t rely on JS, and right now that’s pixels.  Same site pixel does feel like it’s something in between.  

Lola Odelola (LO): From Samsung.  Got me thinking about what’s happening in another space, event level conversion API.  Is there a way to bridge the two?  Seems to have the same goals, discussion to meet in the middle, work together?  

JW: Thanks for the comment.  The Conversion Measurement API from Google Chrome?  PCM and that are **Event Level.**  There are some fundamental differences in the privacy goals.  64 vs 8 bits, for example.  PCM is intended to be privacy by design, not optional privacy by spec.  

JW: There are features that will be in Conversion Measurement API beyond the goals of PCM.  TAG suggested that we align naming convention across the 2, and that’s being done.  


### [Support measurement of non-paid/organic clicks #72](https://github.com/privacycg/private-click-measurement/issues/72)

JW: Support measurement of non-paid/organic clicks #72.  Another company reached out directly for this.  Talking about the case for measuring Organic vs Paid traffic.  

JW: Organic is people talking, articles, that type of thing.  It’s a binary thing.  Send out empty source as part of the reporting?  Is there interest in this type of functionality?

BM: Good idea, but use something other than “empty”.  

JW: There are 256 meaningful entries, but we can make sure it’s obvious in the report that it doesn’t correspond to a source ID.

BS - We hear alot about this use case, it’s a top use case.  Is it worth money to spend on ads?  What’s the incremental value in spending money?  Maybe you can see a chart where you see what’s going on (paid vs unpaid). But really you want to do lift measurement.  Take a look at the other issue I file, where view-through is evaluated.  

BS - Help advertisers understand what’s going on with their spend.  


## [First Party Sets](https://github.com/privacycg/first-party-sets)


### [Software as a service use case for FPS #33](https://github.com/privacycg/first-party-sets/issues/33)

Joel Odom: Summarizes issue. Use-case: Software as a service. Marketing use case. Salesforce provides marketing software as a service. Marketing is different from advertising. There is usually some self-identification of interest in working together in B2B use cases. Example: conference. Customers use software to accept registrations for a conference. New factor is that their SaS is running on a “3rd party” website. Software allows them to accept registrations and more (I didn’t understand what else). A customer yesterday in the UK - talking about privacy changes. Country specific TLDs. They’d like to work across different domains. Desire to raise this case about “marketing” versus “advertising”. Is this a valid use-case? Seems like it’s not in the threat model. Is this a good use for FPS? If this is not a use-case for FPS - looking for Privacy preserving alternatives. 

Kaustubha Govind: Seeing more examples like this from service providers, which operate multiple domains. Agree that the two different brands owned by the same organization fits in. Where the 3rd party comes in is where it’s NOT a FPS use-case. Storage partitioning is the right solution here. I’ve proposed an issue (and Firefox announced cookie partitioning). This is what Chrome is thinking of as well. You mentioned your customer wants to use the same marketing backend across these TLDs. Cookie partitioning + FPS should allow you to share storage across multiple domains owned by the same organization. 

Joel: We will experiment with this and try. In general I’m worried that marketing use-cases might be left out. 

Kaustubha: I know Safari’s prescribed solution is “requestStorageAccess” API.

Joel: I’m familiar with this. There is no homogenous way across the browsers and the UX is not… seamless. Hopefully we can find a middle-ground that preserves privacy. 

Kris: I’d like to see the SaaS use-case supported under FPS. I’d like to see them used as an education mechanism. Where you are grouping domains, you’re explaining to the end user how they are formed. I’d like to see SaaS companies be able to explain: “These are my domains, these are the vendors I’m using and why.” Wrt the “Storage Access API” we have no ability to explain. If we can make FPS more informational it’s more beneficial to end users.

Lola: What is being done to prevent ad networks from abusing FPS? Have we started thinking explicitly about how to prevent abuse in FPS.

Kaustubha: 1. Require all the domains in the set are owned by the same organization; 2. Each domain can only belong to one FPS.

Lola: I’ll follow up on GitHub.

Kaustubha: In response to Kris. I think what you’re suggesting is your organization could list these vendors and express their affiliation. Chrome isn’t implementing “Storage Access”. We think cookie partitioning is a better solution. You don’t need the user to opt-in. You get a “sharded identity”. “Request Storage Access” actually gives you access to 3rd party cookies. We are trying to find other solutions. Question: What do other browser vendors think about this?

John: “Storage Access API”. It’s not about partitioned storage. It’s a way to request access to your “First Party Storage” as a 3rd party. We supported partitioned cookies from 20XX - 2019. Servers were not prepared for seeing different cookies coming in in different scenarios. How can we ensure existing servers do not get confused by partitioned cookies? One example: Server thought it was in a bad state and wrote blank cookies. User was getting logged out. Caused many problems for a major site. I think there needs to be an opt-in. We aren’t against partitioned storage, but we try to make it ephemeral. 

Anne: I’m curious about the problems Safari ran into with partitioned cookies. If they are partitioned correctly, it seems this shouldn't happen. 

John: They were doing redirects between 1st parties. They needed to be “visited” so they did a redirect. 

Anne: They’d also have to do link-tracking for that 3rd party thing to know right?

John: No, they weren’t trying to forward the user-identity into a 3rd party context. It got bad. 

...discussion of this specific example… scribe can’t keep up…

Anne: If you can dig up the details of this - it would be helpful.

John: It was filed as “this is broken” and I did the investigation. It could be a specific thing that only affected Safari. Maybe that logic about needing to be “visited” was responsible.


### [Cache partitioning impacts perf optimizations that FPS might help recover #35](https://github.com/privacycg/first-party-sets/issues/35)

Erik Anderson: Both Chrome and Edge have started to turn on double-keyed cache partitioning. Double keyed means: “top origin” + “frame origin”. It revealed that some people have different definitions of “double keyed”. A team at Microsoft spotted an issue via telemetry. They’re loading a document from another origin. But they know, before kicking that off it’ll fetch a specific script. That broke. Now with double-keying, when that script is downloaded in the top-level context, it’s different from the one downloaded in the iFrame. Filing this issue to see if there is a way to recover this optimization. Maybe FPS? Safari is not double-keying. It’s all top-level origin. Is cache partitioning perceived to be a security boundary? What is FPS protecting? Is this a relaxation that makes sense?

Kaustubha: &lt;details about Chrome>, &lt;details about other browsers>. Some of your have a much more expansive view of web platform issues. With respect to cache partitioning, that’s to prevent cross-site-leaks. So I’ve been treating it as a privacy / security thing. I’m understanding we should NOT use FPS for this. At least for cache, seems like we are converging on NOT using FPS and instead keeping site as the key. Is that performance hit so significant that we should trade it off against security?

Erik: When we spoke about it as a security boundary was it about data-leakages, or something more significant? 

John: We implemented storage and cache partitioning in 2013. It was purely about privacy at the time. In our parlance it would be “triple keying”. I don’t think we have seen a bunch of “attacks” leveraging that we are only keying off of the “top site”. We have seen trackers abusing the partitioned cache. If they are blocked by using storage - they’ll resort to cache. We’ve been looking at “how long should we hold on to cached entities”.

Anne: I think for the attacks you want to look at access leaks. Things that are possible are like, figuring out what kind of pages they like on Facebook. I think you can do some amount of searching into Gmail. A bunch of these things have been tackled. The cache can be used for a bunch of side-channel reads across sites. If one site is compromised it can be used to attack another. As for storage - there are complexities with Service Workers (similar to HTTP Cache). We don’t have a good answer there right now. If sites adopt service workers they are liable to attacks.

Erik: Do you view the lack of double / triple keying as an unresolved security issue?

Anne: There are a bunch of nuanced cases. Sometimes you can even attack things that are triple keyed (as the parent). There are other attacks against frames as well. History API. Window enumeration. Other side-channel attacks. If we want to tackle security of iFrames it needs more than cache. But the cache does help. Not a current priority. 

Daniel: Still open TAG review on FPS. I’d encourage engagement. “Same Party Cookie” attribute. It kind of threw us… it’s stalled, because we feel like the review for FPS hasn’t bottomed out. I encourage re-engagement on the FPS review in TAG. 

Kaustubha: I was anticipating comments. How should we do this? 

Daniel: I don’t know. It might be better to have a side discussion on it where we do it in the context of the TAG. I’m not sure exactly. Let me take that feedback back to the TAG group. Summarize our concerns. 


## Any other business
*   Virtual F2F in May; please respond to [Doodle Poll](https://doodle.com/poll/ad75unudbfizeamb?utm_source=poll&utm_medium=link)
*   Next meeting in 2 weeks.  Reminder that if you want to dig further into a specific topic, we can put together an ad hoc meeting.  You can [file an issue here](https://github.com/privacycg/meetings/issues/new) to kick that off.
*   US Daylight Saving Time in effect March 14th; double check your calendar for March 25th call


## Attendees
1. Achim Schlosser
2. Adrian J Isles
3. Allan Spiegel
4. Anne van Kesteren (he/him; Mozilla)
5. Andrew Knox (Facebook) 
6. Angela Ballard
7. Anthony Molinaro
8. Arthur Edelstein (Mozilla)
9. Barb Smith
10. Ben Savage (Facebook)
11. Bill Densmore (ITEGA) 
12. Bill Maslyn
13. Brendan Riordan-Butterworth (eyeo GmbH / IAB Tech Lab)
14. Brian May (dstillery)
15. Chris Needham (BBC)
16. Chris Wilson (he/him; Google)
17. Chris Yanda (BBC)
18. Christine Runnegar (PING co-chair, part)
19. Christopher Conrett
20. Craig Spiezle
21. Daniel Applequist
22. Daniel Rojas
23. David Dabbs
24. Don Marti (CafeMedia)
25. Eric Mwobobia (ARTICLE19) 
26. Ericka Wright
27. Erik Anderson (Microsoft)
28. George Fletcher (Verizon Media)
29. Heather Tjeerdsma (Dotdash)
30. Heinz Baumann (Quantcast)
31. Hitesh Lad (Epsilon/CJ Affiliate)
32. Jack Frankland
33. Jade Kessler
34. Jason Nutter (Microsoft)
35. Jeegar Shah
36. Joel Odom (Salesforce)
37. Jon Callas (he|they), EFF
38. Kate Cheney
39. Kaustubha Govind (Google)
40. Kelda Anderson (Microsoft)
41. Kris Chapman (Salesforce)
42. Leonard Law
43. Les Cochrane
44. Liz Hurley (Epsilon/CJ Affiliate)
45. Lola Odelola (Samsung Internet)
46. Manuel Budemaier
47. Marshall Vale
48. Matt
49. Maud Nalpas (Google/Chrome)
50. Max Gendler (NY Times)
51. Melanie Richards (Microsoft)
52. Mike O’Neill (Baycloud)
53. Paul Bannister (CafeMedia)
54. Paul Marcilhacy (Criteo)
55. Peter Saint-Andre (Mozilla)
56. Peter Snyder (Brave)
57. Przemyslaw Iwanczak
58. Raj
59. Russ Hamilton (Google)
60. Russell Stringham (Adobe)
61. Salvo Nicotra (Neodata Group)
62. Sam Macbeth (DuckDuckGo)
63. Sean Harrison (Google)
64. Shachaf
65. Shaun Gilmore
66. Stefano Cossu
67. Steven Englehardt (Mozilla)
68. Steven Valdez (Google)
69. Tanvi Vyas (Mozilla)
70. Theodore Olsauskas-Warren(Google)
71. Theresa O’Connor (Apple)
72. Thomas Bergemann
73. Tim Cappalli (Microsoft Identity)
74. Tim Reid (SpotX)
75. Tony Resendiz
76. Viraj Awati
77. Sam Weiler
78. sc
79. Wendy Seltzer (W3C)
80. Kate Cheney (Apple)
81. John Wilander (Apple WebKit)
82. Tess O’Connor (Apple WebKit)
83. Valentino Volonghi (NextRoll)
84. Wendell Baker (Verizon Media)
