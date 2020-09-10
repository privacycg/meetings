# Privacy CG telecon, 10 September 2020

* Agenda: [09-10-agenda.md](https://github.com/privacycg/meetings/blob/master/2020/telcons/09-10-agenda.md)
* Chair: Tess
* Scribe: Pete

### Attendees

1. Abhinav Sinha(PubMatic)
2. Andrew Knox (Facebook)
3. Anne van Kesteren (he/him; Mozilla)
4. Aram Zucker-Scharff (Washington Post)
5. Arthur Edelstein (Mozilla)
6. Ben Savage (Facebook)
7. Bartłomiej Romański (RTB House)
8. Bill Densmore (ITEGA)
9. Brad Rodriguez (Magnite)
10. Brian Campbell (Ping)
11. Cezary Cerekwicki (Opera)
12. Chris Fredrickson (Google)
13. Chris Pedigo (DCN)
14. Christopher Cornett
15. Eric Mwobobia (ARTICLE19)
16. Erik Anderson (Microsoft)
17. Garrett McGrath (Magnite)
18. George Fletcher (Verizon Media)
19. George London (Survata)
20. Jade Kessler
21. James Hartig (Admiral)
22. Jason Nutter (Microsoft)
23. Jeddy
24. Jeff Weiland
25. Joao Natali
26. Joel Meyer (OpenX)
27. John Sabella (PubMatic)
28. John Wilander (Apple WebKit)
29. Josh Karlin (Google)
30. Kate Cheney (Apple)
31. Kaustubha Govind (Google)
32. Konrad Dzwinel (DuckDuckGo)
33. Kris Chapman (Salesforce)1
34. Kushal Dave (Scroll)
35. Lionel Basdevant (Criteo)
36. Marijn Kruisselbrink
37. Marshall Vale (Google)
38. Martin Meyer (Akamai)
39. maud
40. Melanie Richards (Microsoft)
41. Michael Kleber (Google Chrome)
42. Mike Lerra (Google)
43. Mike O’Neill (Baycloud)
44. Mingzhe Li (Akamai)
45. Niarci
46. Paul Bannister (CafeMedia)
47. Paul Jensen (Google)
48. Peter Snyder (Brave)
49. Raj Belur (Amazon)
50. Rowan Merewood (Google)
51. rstratto
52. Russ Hamilton (Google)
53. Russell Stringham (Adobe)
54. Ryan Avecilla
55. Sam Weiler (he/him, W3C/MIT)
56. Sean Harrison (Google)
57. Shaun Gilmore (Apple)
58. Shigeki Ohtsu (Yahoo Japan)
59. Shivani Sharma (Google)
60. Steven Englehardt (Mozilla)
61. Steven Valdez (Google)
62. Tanvi Vyas (Mozilla)
63. Theresa O’Connor (Apple)
64. Wendell Baker (Verizon Media)
65. Wendy Seltzer (W3C)
66. Devin Rousso (Apple)
67. Przemyslaw Iwanczak (RTB House)
68. Nicolas Arciniega (Microsoft)
69. Jeffrey Yasskin (Google Chrome)
70. James Rosewell (51Degrees)

## Agenda

* [The IsLoggedIn API](https://github.com/privacycg/is-logged-in)
    * [Support "Remember me" functionality #9](https://github.com/privacycg/is-logged-in/issues/9)
    * [Cater for existing logins, a.k.a. upgrade to IsLoggedIn #18](https://github.com/privacycg/is-logged-in/issues/18)
* [Client-Side Storage Partitioning](https://github.com/privacycg/storage-partitioning)
    * [Definition of third party #16](https://github.com/privacycg/storage-partitioning/issues/16)
* Any Other Business
    * F2F and upcoming meetings
    * PING next meeting on 24 Sept (during the normal Privacy CG time)

## [The IsLoggedIn API](https://github.com/privacycg/is-logged-in)

### [Support "Remember me" functionality #9](https://github.com/privacycg/is-logged-in/issues/9)

**Melanie:** motivation is that sites may log users out (time out etc.) so it might be valuable to have a way of persisting a “is-logged-in” bit across page authentications, so that subsequent authentications can require less steps (MFA, CAPTCHA, etc.).  Two points:

1. Are others interested in adding this metadata bit
2. More broadly, should there be a way of persisting data across IsLoggedIn logins

Other conversation included how to handle federated login / trust tokens, and how browser-mediated login flow could help.

**George:** My concerns went beyond federated login; many sites share login across multiple sites that one entity owns.  Or, you might want to have different auth tokens per property (to enforce permission scoping, etc). For example, Google requires heightened auth steps to login when there isn’t a way to tie the login attempt to a previous successful attempt. We should make sure login-history doesn’t get obliterated.

**Melanie:** If i understand correctly, we need something beyond “X logged in with Y” before?

**George:** As long as IsLoggedIn can connect logins for previous users, that might suffice, will have to think more.  But trust across authentications is an important signal, and having a “I successfully logged in before” hint early is important.

**Ben:** RememberMe use case is important bc login is complex, and its a lot of infrastructure browsers are taking on to stick themselves between browsers and websites. Good to demonstrate / motivate with specific problems IsLoggedIn is required for.

**John:** IsLoggedIn is motivated by disentangling login state and other browser features / state. Similar to how “user interaction / gesture” has been used in the past as a signal for different / heightened featuring gating, IsLoggedIn can serve a similar purpose for a potentially different feature set.  Additionally, browsers can use the explicit IsLoggedIn bit to, say, help users audit their logged-in-ness on sites.

**Sam:** Thanks to George for highlighting use cases related to multiple users, one device. Aram, please explain the paywall use case (either on the call or in GH).  Also, there might be confusion between IsLoggedIn and some “RememberMe / half-logged-in” state.  Maybe different language might solve this confusion. Re: how much data is purged

**Melanie:** Thinking more about lang sounds good 

**Steve:** RememberMe bit could be used to re-link prior and new state, defeating one of the main benefits of IsLoggedIn.

**Melanie:** RememberMe is still in early days, we should think more about this threat and how a RememberMe bit could be miss-used.

**John**: RememberMe bit is only revealed after IsLoggedIn is completed; you don't get the token first. But re “multiple accounts, single device” concerns, we’d need to take care that only the appropriate bit/token for the logged in account is shared / sent

**Michael:** Re Sam, seems like there are two ideas:

1. Is someone logged in 
2. Can i remember information about previous site interaction

**John:** Re Aram and audio issues so they can’t express themselves; a metered paywall use case might need to be handled by a different spec.  That seems different / out of spec for IsLoggedIn

**Ben:** I dont think John answered my question.   I think John said “is logged in is a useful signal, and there might be benefit in tying other functionality to it”, but that doesn’t answer why “mediating logins is the right way to do so”. Re the mentioned problems:

1. I'm not convinced that clearing state more often / in new ways is a problem
2. I’m not convinced that the cargo is worth the freight here, for this approach
3. youll need more features too

**John:** There does seem to need to be a way to clear more state, more often, such as regards bounce tracking and redirect tracking. If we could limit who gets to set state, and for how long, we could have a good solution to bounce tracking.

**Michael:** Logging into a website today often seems to involve some kind of cross-site tracking enabling token (email, federated login), but in general, being logged in seems to entail tracking-capability. So might be better to distinguish between “these are cases where we want state to be maintained” and “logging in”.

#### Zoom chat transcript

**Aram:** Hi, I'm experiencing audio issues with my mic, so I can't speak, but the goal is to support some degree of meter - which is important for the biz process for paywalls for publishers I was thinking more along the lines of a lot of publishers who have 1-hit meters on their paywalls, allowing them to see if a user is known to them, regardless of login state.   --- He will file an issue

**Bill Densmore:** Ben, is there some reason why it would not be a good thing for users to be clear where they are logged in and when? 

**George Fletcher:** I think there can be a simple solution for separating bounce-tracking and login flows. Basically, the RP can publish in a .well-known location the set of IDP redirects it will use for login. The browser, can pull that file from the .well-known location and easily distinguish login flows from bounce-tracking. 

**Sam:** Michael, there is some discussion the WebID docs of having IDPs providing unique identifiers (e.g. email addrs) to each RP Described in those docs as the RP tracking problem 

**Ben Savage:** isLoggedIn does not appear to be a solution that would fix "bounce tracking". It would seem to continue to allow bounce tracking for sites with logged in state 

**Melanie Richards:** thanks for the feedback everyone! 

**Michael Kleber:** Sam: Yes indeed, I hope that takes off!  But even then, AFAIK, your WebID IDP learns all the sites you visit 

**Sam:** Michael, one of the charms of WebAuthn is the chance to dodge all of that. 

**Anne van Kesteren:** A case I can see for browsers managing logins is to reduce the amount of popup usage (which can currently be used to escape various things) and eventually be able to put restrictions on popups, but isLoggedIn doesn’t really help with that. 

**Anne van Kesteren:** (With apologies to Tess, I’d move this to Slack, but it doesn’t seem others are posting there and not everyone is on Slack.) 

**Michael Kleber:** Sam: I'll be very happy to migrate to a web where logged-in-ness is less likely to imply some sort of cross-site joining... but I think IsLoggedIn (as a gatekeeper for longer-term single-site state) pushes *against* that goal

**JohnWilander:** Thanks for all the feedback on IsLoggedIn! 

### [Cater for existing logins, a.k.a. upgrade to IsLoggedIn #18](https://github.com/privacycg/is-logged-in/issues/18)

**John:** Previously was called “grandfathering”, now called “upgrade to IsLoggedIn”. It would be good to allow sites to upgrade existing sessions w/o being exploited.  We have ideas, but are eager for others' thoughts.  Please, share!

Since queue is empty, here are the ideas we’ve had:

1. grace period: for the first year IsLoggedIn exists, there could be a laxer set of rules
2. “the pill”: some potential state in the URL bar that indicates if the user is logged in or out, to signal state to user
3. prompt: browser can’t tell whether to respect the sites “this person is logged” in request, so ask
4. always respect for short period of time: short logged in periods automatically granted, but requires more interactivity 
5. heuristic: try to be clever browser side about whether a person is logged in (but, how to know the user name?)
6. nope: just force users to reauthenticate if they want the IsLoggedIn bit

**Wendy:** How to handle upgrades might depend on what would happen after IsLoggedIn (i.e. why the site wants IsLoggedIn), and user expectations might differ, requiring a new login seems safest.

**George:** Most identity providers will extend an auth-session as long as there is on going user activity; it would be good to be able to preserve that here

**John:** Good point, we have always considered “rolling” state, or allowing less/no-friction extending of active sessions.

The issue is on GitHub, ill add the above menu there, lets discuss there.

## [Client-Side Storage Partitioning](https://github.com/privacycg/storage-partitioning)


### [Definition of third party #16](https://github.com/privacycg/storage-partitioning/issues/16)

**Anne**: Is there a shared understanding of what “third-party” means? Maybe there is one in the Storage Access API?  Would be good to distinguish between 3p origin (for SOP), and 3p site (for state partitioning). Cookies, for example, have a diff definition of 3p than other specs / parts of the platform (cookies are evaluated against intermediate frames, other areas determine 3p-ness by comparing only against top level frame).

**Kaustubha:** We should consider how first-party sets interact with 1p vs 3p definitions.

**Anne:** Maybe you mean a different issue?

**Kaustubha:** Lets define 3p as “anything not in the 1p set”

**Anne:** That assume 1p sets

**Kaustubha:** Yes, currently we limit to origin + port + scheme, but why not extend to 1p sets (assuming folks agree on 1p sets)

**Tess:** Broadly agree with Anne, that we should specify clearer definitions, storage access api docs have a definition of what 1p is, and 3p is “everything else”

Re cookies: cookies look at intermediate frames. If the goal is to define privacy boundaries, then intermediate frames seem worth considering.  Cookies have odd legacy behavior, lets get the platform away from that.

**Anne:** this will be more meaningful once the platform removes direct script access.

**Tess:** So 3 definitions, 1p origin, 1p site, 1p site + intermediaries 

**Josh:** Re cookies: the can be useful to prevent certain attacks where the 1p is re-enbedded as a child of a 3p frame.  We should consider intermediary frames

**Anne:** But 3p frames can access top level frame storage directly anyway

**Josh:** true, but what if the frame doesn’t know its being embedded, and being hijacked for some attack / action. Also, +1 for a scheme-full-site definition; we do it for disk caching in chrome

**Anne:** Thats already in the standard

**Josh:** Where?

**Anne:** Site is defined in HTML / fetch

**Tess:** all web specs referencing ‘site’ should reference the above spec

**James:** we’re talking about definitions for first and third parties; we’re talking about intermediaries too. We would benefit from a clear definition of terms and policy concerning what each entity is and is not able to do and under what circumstances. I prefer the term “supply chain” chosen by a domain owner rather than intermediaries.  We would benefit from having a definition associated with each entity or party and of what different parties can access, their trust and privacy status. For example; if the definition of a third party is any entity not a first party and the first party is the entity associated with a domain name displayed in the address bar then that would make the web browser a third party. (note perhaps such work and definitions could build on [https://www.w3.org/TR/tracking-dnt/#terminology.participants](https://www.w3.org/TR/tracking-dnt/#terminology.participants))

**Jeffrey:** Wanted to refine the question: 

1. We have both a unary is-first-party(frame) definition and a binary are-first-party(frame1, frame2) predicate. The binary predicate is where we need to answer the "use DNS or also first-party-sets" question. The unary predicate is what cares about the nesting path.
2. it would be useful to separate the security definition from the privacy definition. Cookies appear to use a security-based definition, where environments aren't accidentally joined across security boundaries. This cares about the nesting path. Other things will use a privacy-based definition, where environments can't even intentionally join themselves. This probably won't care about the nesting path.

**Anne:** Main question here is whether to include intermediaries when determining 3p-ness. Its not clear what the benefit would be of including intermediaries given the capabilities that exist in the platform today.

**Josh:**  It would be useful for sites to know if there embedded or not.

**Anne**: if sites are worried about being embedded, there are X-Frame-Options too

#### Zoom chat transcript

**Aram:** Useful definitions at [https://www.w3.org/TR/tracking-dnt/#terminology.participants ](https://www.w3.org/TR/tracking-dnt/#terminology.participants)not sure if they are anywhere else

**Kristen Chapman:** +1 to using first-party sets as part of the definition when available

**Anne van Kesteren:** X-Frame-Options: sameorigin does protect against that

## F2F announcements

* Wednesday and Thursday Sep 16-17 8am - noon PT.
* Deadline for agenda+f2f issues and proposals is today.
* Final agenda will be sent out tomorrow morning pst to give everyone a chance to prepare. It will be posted at [https://github.com/privacycg/meetings/tree/master/2020/09-virtual](https://github.com/privacycg/meetings/tree/master/2020/09-virtual).

## Any other business

* No meeting September 24th - PING instead.  After Sept 16-17 Face to Face, our next meeting is October 8th.

**Tess:** for the meeting after the 8th (Oct 22?), it might make sense to cancel that call, since its in the middle of TPAC
