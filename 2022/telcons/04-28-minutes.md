# 28 April 2022 Privacy CG Meeting

-   Chair: Theresa O'Connor
-   Scribe: Cameron Boozarjomehri (first half), Sam Weiler (second half)

## Agenda

-   [Cross-site cookies standardization](https://github.com/privacycg/meetings/issues/16); see [this comment](https://github.com/privacycg/meetings/issues/16#issue-1143436255) for a detailed agenda
-   Any other business

## Attendees

1.  Anne van Kesteren (Mozilla)
2.  Alexandre Nderagakura (IAB Europe)
3.  Allan Spiegel (Adobe)
4.  Angela Ballard (CJ Affiliate)
5.  Andrew ?
6.  Ben Kelly (Google Chrome)
7.  Ben VanderSloot (Mozilla)
8.  Brandon Maslen (Microsoft Edge)
9.  Brian Campbell (Ping)
10. Brian Lefler (Google Chrome)
11. Brian May (dstillery)
12. Cameron Boozarjomehri (Mozilla)
13. Cezary Cerekwicki (Opera)
14. Chris Needham (BBC)
15. Christine Runnegar (PING co-chair)
16. Claude Vervoort (Cengage Group, IMS LTI Working Group co-chair)
17. Dan McArdle
18. Dan Veditz
19. Daniel LaLiberte
20. Dylan Cutler
21. Don Marti (CafeMedia)
22. Eli Grey (Transcend)
23. Eric Lawrence (Microsoft)
24. Eric Mwobobia (ARTICLE19)
25. Erik Anderson (Microsoft Edge)
26. Erik Taubeneck (Meta)
27. Frantz
28. Gabby Schnaid
29. Gaurav Chhawchharia (Yahoo)
30. George Fletcher (CapitalOne)
31. Guillermo Movia (Article 19)
32. Heather Flangan (Spherical Cow Consulting)
33. Helen Cho
34. James Hartig (Admiral)
35. James Rosewell (51Degrees - 20 mins after start)
36. Jason Nutter (Microsoft)
37. Jeegar Shah (CJ Affiliate)
38. Jeffrey Yasskin (Google Chrome)
39. Johann Hofmann (Google Chrome)
40. Joshua
41. John Wilander (Apple WebKit)
42. Kaan Icer
43. Kaustubha Govind (Google Chrome)
44. Kelda Anderson
45. Kris Chapman (Salesforce)
46. Lee Graber (Tableau / Salesforce)
47. Lola Odelola (Samsung Internet)
48. Matthew Liu (The Washington Post)
49. Mike O'neill (Baycloud)
50. Nick Doty (Center for Democracy & Technology)
51. Paul Zühlcke (Mozilla)
52. Peter Snyder (Brave, PING)
53. Pragya Seth (Arkose Labs)
54. Providence Baraka (Article 19)
55. Sam Goto (Google)
56. Sam Macbeth (DuckDuckGo)
57. Sam Weiler (W3C/MIT)
58. Shubhie Panicker
59. Stefan Popoveniuc (Google Chrome)
60. Steve Englehardt (DuckDuckGo)
61. Steven Bingler (Google)
62. Tanvi Vyas (Mozilla, co-chair)
63. Tara Whalen (Cloudflare)
64. Theodore Olsauskas-Warren (Google)
65. Theresa O'Connor (Apple, co-chair)
66. Tim Cappalli (Microsoft Identity)
67. Tim Huang (Mozilla)
68. Vittorio Bertocci (Auth0|okta)
69. Wendell Baker (Yahoo)
70. Wendy Seltzer (W3C)
71. Yi Gu (Google Chrome)
72. Shubhie Panicker (Google Chrome)

## Notes

### [Cross-site cookies standardization](https://github.com/privacycg/meetings/issues/16)

-   Anne: Should Cross Site Cookies be blocked or Partitioned?

-   Firefox plans to ship partitioned cookies for 3rd parties, Chrome plans to block 3rd Party cookies

-   John Wilander (Apple Webkit): No Plans for blocking by default but open to "opt-in" condition for partitioned cookies.

-   Tess: Opposition to partitioning by default is compatibility based

-   John: Partitioned cookies should be session cookies/in memory only, that's what we were shipping

-   Brian May: Why limit cookies to just session cookies

-   John: Reasoning is mostly around Auth, very risky to do so in a 3rd party context. Better as 1st party where user can look in url bar and check the site they are logging in on

-   Kaustubha: Agrees with john, partitioning by default could have multiple issues, from the chrome perspective there is concern over developer confusion. Cook have compatibility issues for user and non-user facing elements. If the cookies are no longer supported then developers and site operators need to be looped in on the change. Chrome hopes to have a migration period for testing, why they back an opt-in option. Worried if partition cookie by default, could run in to storage issues.

-   Pete (Brave): partition 3rd party cookies by default,avoid compatibility issues by making partitions ephemeral (30sec - 1 min for some log in flows). Disabled storage access partition in chromium

-   Brandon Maslen (Edge): prefer 3rd party cookies are blocked and sites opt in and make a conscious decision to achieve that behavior.

-   Ben (mozilla): we partition cookies, unlike safari where they are more ephemeral firefox keeps cookies longer so seeing less breakage. Firefox does see a business logic breakage instead but doesn't see it as a privacy loss to block by default and do opt-in partitioning.

-   Kristen Chapman: from a Saas perspective, prefer not auto-block, but want partitioned and something site can opt in to. Biggest issue with partitioned cookie for session is how cookies are used for history. It's not about tracking the user so much as providing user experience. Store data client side instead of server side to improve user experience and give users some control with client side cookies. Absence of partitioned cookies means more need to store serverside

-   Iee graber: When partitioning or 3rd party cookies gets turned off or partitioning and chips gets released, how have we decided on a reasonable amount of time between ending support and moving to something new

-   John: webkits HTML storage is already ephemeral and ties into Kristens point of client side can be used for HTML storage instead of cookies in header. Didn't see compat issues from ephemeral cookie but rather servers getting different cookies for 1st vs 3rd party cookie

-   Eric Lawrence: Sites requesting cross browser consistency, and what is the runway for opt-in (get sites updated to accommodate new attribute). Blocking cookies is better because more predictable, and wind up with accidental compatibility wins where site didn't need to be updated or needed minimal update

-   Tanvi: Mozilla chose to partition cookies (similar to storage) to prevent cross-site tracking. Nothing wrong with partitioned cookie because not cross site. We understand this can cause web compat issues and want uniform behavior between browsers, just want to communicate firefox's reasoning for its stance.

-   Anne: Seems like most folks leaning toward blocking 3rd party cookies by default (with options for opt-in to partitioned cookies)

-   Anne: Opt-in to partitioned cookies

-   Option 1) Google proposes CHIPS: Cookies have additional attribute used to set additional restrictions/permissions.

-   Option 2) Using a dictionary approach to set cookie parameters

-   Lee Graber: Push back on Anne's claim that Partitioned cookie only work in a partitioned context. Supposedly the 1st and 3rd party cookies can persist and be shared even when partitioned. Second Question, if I set the partitioned attribute in today's browser, how many browsers will clear cookies versus just ignore this flag?

-   Anne: if you set a partitioned cookie in a 1st party context, it may be ignored and the cookie is still set. No browser other than chrome doing anything with partitioned cookies so other browsers would likely ignore flag

-   Kaustubha confirmed this

-   Dylan Cutler: in chrome can set partitioned cookies in 1st party context, the context will still just be the 1st party

-   Anne: and when client replays it, still have partitioned context?

-   Dylan: yes, same context

-   Tess: Is the partitioned bucket where partitioned key is just the top level site? Is that a different partition than 1st party cookie

-   Dylan: behavior wise it's the same, just stored in different data structure to accommodate sites that haven't transitioned

-   John Wilander (Apple): from Webkits perspective like storage access APIs route but open to CHIPS (http implementation is a different vertical and they emphasize how http is used for things beyond browser)

-   Brian May: what will the output of this discussion be? What will the future home of the partitioned cookies conversation be? Will there be a doc exploring what we discussed?

-   Anne: right now the goal is to see where everyone stands for these cookie discussions, and split for how we handle cookies across various specs. None deal with partitioned cookies or 3rd party cookie blocking and we need to get that context. No concrete doc summarizing the whole thing

-   Tess: artifact from these minutes to capture discussion

-   Ben: in favor of opt-in for partitioned cookies, but between 2 options leaning towards CHIPS, nice to have details in the HTTP header and not another API. 1st party sets tie in with chips is hard to support

-   Brandon: We like the core concept of CHIPS; we also like requestStorageAccess; allows UA to confirm user's agreement.  Leaning toward a world with both.  Before we move to block-by-default, leave site a way to opt into partitioned world.

-   Johann (Chrome): re: cookie fields: priority of constituencies suggests prioritizing dev experience over what browsers prefer to implement.  If there's consensus on CHIPS - everyone likes it with only minor issues - that's valuable.  We're very interested in a cross-browser solution.  I want to see CHIPS go forward.

-   Lola Odelola (Samsung): we block 3rd p cookies by default; users can change settings.  Haven't looked at CHIPS.  It doesn't seem bad.  @@ How does CHIPS work?

-   From zoom chat since audio cut out: We have similar concerns that were earlier expressed so would like to know more clarification about how tightly FPS is coupled to CHIPS

-   Tess: this sounds similar to Ben's concern re: coupling with FPS.

-   Heather (FedID CG Chair): we've been building a table of 3p mitigations.  Please help us fill that out; it might be some of the documentation you're looking for. <https://github.com/fedidcg/use-case-library/wiki/Third-party-cookie-mitigations>

-   Kaustubha: we can work w/ browsers that don't support FPS to disentangle them.  We seem to be aligned re: partitioning, only difference is set v. site. But we're willing to work with you.  FPS will be a setting; we want to make sure the turned-off mode is consistent and specified.

-   Lee: I'm confused between SAA and CHIPS.  SAA is method to allow tracking cookies?  But CHIPS supports non-tracking cookies w/ lower requirements? Seems different; SAA is an extra layer if tracking not needed?

-   John: SAA is not about tracking cookies; it's about 1st p cookies, which could be tracking, but we thingk of it as ID.   if caller of API says they're willing to take partitioned cookies, we might not ask user for permission.   our concern on HTTP is not about Apple's architecture. It is a developer concern for all the developers who use HTTP for things other than websites. Cookies are the HTTP state mechanism. We prefer keeping cookies as an HTTP thing and not a browser thing. Also wanted to mention that the SAA route allows us to let developers ask for persistent partitioned cookies and the browser can then decide to ask the user. SAA is more flexible in this regard.

-   Johann: I understand John's concerns re: architecture.  Priority of constituencies, for us, means devs.  But I agree with you in principle; good to keep in mind.  No one on my team wants to stop SAA innovation; we just want CHIPS to be supported.

-   Ben VanderSloot: we don't observe privacy benefits to SAA in Firefox, since if they have JS, they can use LocalStorage; not a win on our platform.  A pro for SAA is that it unifies the way to get out of default cookie behavior.  We see SAA still being used in future even if we also do CHIPS.

-   Anne: How do we organize standardization?

-   John: in IETF spec, we'd look at the non-web cases; make sure they aren't confused.

-   Anne: want to define behavior for 3p cookies; same-site attr is different from how we define it.   Only top-level site; Chrome experimented with keys, but going back to one key for performance reasons.   How do we tack CHIPS onto this?  Existing things not super well defined, esp. Integration with HTML and Fetch.  And who owns store/parsing definitions?  Current cookie spec has faulty layering.  Is there appetite for resolving that?

-   Johann: yes.  this seems all very browser specific, maybe we would do IETF a favor by moving 3p cookie specification to a different forum?

-   Anne: something like that is reasonable.  Not sure if parser should be self-contained.  Other specs deal w/ higher level logic.

-   Tess: I'm struck by how this is defined across multiple SDOs and there are layering problems.  Those seem connected.  Not surprised.  I'm struck by the similarities of the problem to others - e.g. storage partitioning effort.  That's not producing a spec, but it's about organizing, e.g. keeping todo lists, even as the work happens in other specs.  Anne, is that model working well?  Can we apply it here?

-   Anne: it might be a good start.  It might also help Brian May; have one to outline what needs to be done.

-   John: my reading of the situation is that people who live most of their standards life in IETF are annoyed that we just bring them things.  They might be more annoyed if we break this out their spec; they might be happier if we work with them.  They also have a lot of expertise we want to make use of.

-   Nick Doty: I'm supportive of addressing the coordination problem.  It could be that the concepts of partitioning might apply beyond the web context; some of the non-web HTTP things might benefit from partitioning capability.

-   James Rosewell: we should step back and think about use cases for cookies - for state sharing mechanism in our digital infrastructure.  To my mind, the privacy boundary of domain no longer relevant. [First and third party are not relevant under laws [data controller and processor is - see ICO & CMA - "There is no explicit reference to the distinction between first-party and third-party data in data protection law']](https://ico.org.uk/media/about-the-ico/documents/2619797/cma-ico-public-statement-20210518.pdf). We should look to align what we're doing to laws and use cases, whether that is fedID, advertising, commerce or others.  Rather than focus on micro level, maybe step up, have workshop to discuss what we want the state sharing mechanism to look like.

-   Sam: yay for re-engaging IETF folks, and thanks for taking on editing updated cookie I-D. encouraged.

-   Tess: this seems valuable.  Exciting to have consensus.

-   Anne: can we tackle last two items in next Europe-friendly meeting?  I like the idea of having a cookie repo where we try to link things together.  Maybe make it part of storage partitioning?  I'll open an issue at http extensions to discuss layering things differently.

## Topics we didn't get to

-   Interaction of cross-site cookies and SameSite=None.

-   Ephemeral partitioned third-party storage (including cookies) by Brave: [Top-Frame lifetime, partitioned storage for embedded frames proposals#18](https://github.com/privacycg/proposals/issues/18).
