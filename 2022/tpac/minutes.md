# 12 September Privacy CG Meeting (at TPAC)
* Scribe: Ben Savage (first 45 minutes), Wendy Seltzer

# Agenda
* Setup, Welcome, Code of Conduct (chairs; 10m)
* FedID CG Coordination (Heather Flanagan: 30m)
    * Updates on FedCM work
    * Discussion of privacy-related topics
    * Decision tree flows
* Overview of group, brief status of open work items and proposals (chairs; 20m)
* Storage Access (Johann; 40m)
    * Status
    * Readiness for graduation
    * Remaining open issues
* CHIPS (Kaustubha; 30m)
    * [Slide deck](https://drive.google.com/file/d/1wSUfOb7BIjtmsO6TdxyBMmw3RUQqCtGa/view?usp=sharing)
    * Status
    * Readiness for graduation
    * Remaining open issues
* Spare (10m)
    * To allow for a break, transit time, or any other business
    * As time permits, [navigation tracking mitigation proposal](https://github.com/wanderview/bounce-tracking-mitigations/blob/main/explainer.md) (Ben Kelly)

## Notes

### Updates on Fed-CM

**Kris Chapman:** (Co-chair of FED-ID community group)

Here to talk about overlap between PrivacyCG and Fed-ID.

Idea: Go to website and sign in using credentials from another site. E.g. Sign-in to NPR using Google, Facebook or Apple

Behind the scenes looks like “tracking” in advertising. Communication. “Relying Party” and “Identity Provider” communicate with one another. Mechanisms they are using are often re-directs, URL-params, 3pcs. Changes will impact Fed-ID. There’s not just one protocol. 

The organizations doing this are not just commercial organizations. Also exists in governments, education, etc. These organizations don’t necessarily have developers they can rely on to implement things. We might think there’s only a handful of big companies that serve as identity providers, but it’s much larger. Thousands. 

When we make changes in this area, we need to continue to support identity federation, but in a way that requires the least amount of change, and is simple. 

In the “Federated Identity CG”, there is a “decision tree”. Identify what different proposals are options at different stages. Started to highlight which options are available in each browser. 



* FedCM proposal. This is not “the one proposal” that will handle all issues.
* StorageAccess API. 
* CHIPS
* First Party Sets
* Login Status API
* CNames
* Enterprise Policies & 3rd party cookies

The point of the decision tree is to determine what is possible. There are some cases that end up in “no man’s land” - where there is no path. We are here to work on that to try to find answers. 

Goals:



* Don’t break identity federation
* Proposals for red question mark boxes where there are no solutions.
* Want to make certain authors of proposals we intend to rely upon are aware of this dependency
    * Make sure proposal authors and browser vendors are signing up to support these flows
* Would be preferable if the decision tree is not wildly different per browser

There are many aspects (e.g. session management, logout, etc.)

**Tess:** Points out only 15 minutes remain

**Kris:** Any feedback? Questions?

Tess: What would be the most useful in the next 12 minutes? What is most urgent?

Kris: Figuring out how we get the federated use-cases understood. How do we work together to come to a joint conclusion. Personal opinion: In Fed-ID land, our orientation is towards keeping federation around. If you’re looking at it from a privacy perspective, your orientation is to eliminate bounce-tracking (etc.). Who wins? How do we negotiate that? Large problem. New type of challenge to the W3C. Few prior examples to follow. 

Heather: We’ve been talking about Privacy-CG stuff. Was everyone aware of that? That’s very important. Want to make sure authors of these proposals that their proposals are being considered for this use-case. 

Anne: Did you file issues on the repos to indicate this?

Kris: We have, but these are “global” use-cases. Not implementation level of detail. 

Johann: Can we get more specific? Is there a specific extension on Storage Access API? I think Fed-CM is the ideal solution. We have this ideal federated identity flow we want to move towards, and we have intermediate stuff there to ensure stuff doesn’t break. Storage Access API is used for that. Which of these are we talking about?

Kris: Fed-CM is *not* the ideal state for how identity federation will work. It’s a specific scenario. Concrete example: Storage access API is insufficient for Salesforce due to the requirement for user interaction. The more feedback on these decision trees the better. 

Johann: Is this still federated identity then? I had assumed Fed-Cm (plus some extensions) would cover it. 

Kris: It’s understandable that people think that is the case. 

Tim Cappalli: There’s a fundamental difference between agreeing there should be an identity/sign-in API for the web and thinking Fed-CM is that.

Martin: You said you don’t see Fed-CM as being the end state for all use-cases. Do you have a sense that there is an emerging end-goal in mind? Or that the group is working towards that?

Kris: No. We are advising the proposals trying to get towards an answer. We are not developing it ourselves.

John Wilander: I think (at least for me), I was under the impression that Fed-CM was trying to solve all of the use cases. I would start with a “gap analysis”. 

Vittorio: I just wanted to emphasize the magnitude of the gap between Fed-CM and the universe of things we do. We have a table. Fed-CM addresses one particular scenario (account selection). It occurs before we start. Some of the other flows in that table might work better if you used Fed-CM first. Asking people to use Fed-CM, is akin to asking people to change their car because their radio doesn’t work. 

Tess: You’ve had the last word. We booked a half-hour session. Next on the agenda is an overview of the group and status of work items. 


### Overview of group, brief status of open work items and proposals

This is the Privacy Community Group. We are here to incubate ideas around improving privacy on the web. Specifically privacy in browsers. We started in early 2020. We have 100s of participants from various industries. When we started we had a lightweight way of submitting proposals. We now have 6 work items in the group:



* Storage partitioning
* CHIPS
* Login Status API
* Private Click Measurement (might move to PAT-CG)
* Storage Access API
* Navigational Tracking Mitigations

Updates? 

John Wilander: Login Status API. It’s been stale since Melanie left Microsoft. Aim to merge with Fed-CM. 3 Gaps. Bespoke login mechanisms that won’t be mediated by the browser. Want to understand if the user is still logged in. Formalizing logging out. Want that to also be understood by browsers. 3rd is disentangling login from tracking (e.g. bounce tracking mitigations).

Johann: Do you (looking at how it has developed) still think all 3 goals are compatible? E.g. have a site declare a login status per user. Maybe that conflicts with the bounce tracking stuff. Has anything changed?

John: That’s part of the conversation here. Perhaps we start with mediated login first (bespoke stuff is so hard). 

PCM: Private Click Measurement. Intend to move to the PAT-CG. Will meet tomorrow. Will talk then. There is also Google’s ARA proposal and IPA. 

Johann: Have a full slot later.

Anne: Storage Partitioning. It’s where we left off last time. Recommend people study the repo. 

Jeffry: Navigational Tracking. There is a bounce tracking session coming up.

Erik T: Question about navigational tracking vs bounce tracking. Is one a subset?

Room: Yes

### Storage Access

[slides](https://docs.google.com/presentation/d/12BUOzSzXumx5zYWVbUDMQWx0O4pQR4w3j1QPNLlzM24/edit#slide=id.g1533373a0cf_0_178)

Johann: one of the three editors of Storage Access API. Overview, why we’re not quite ready to graduate yet. 

[slide: progress overview] 

We’re getting to where engines can implement from the spec without too much divergence. All 3 engines implementing and Edge. Editors from Chrome, Webkit, and Mozilla. Tess now an editor emerita.

Github blocking issues. Those with the blocking label. 

#44. How storage access grant is refreshed when you interact with embedded iframe.

#39 scope of grant in terms of domains. Some disagreement. Moz suggests top-level site by origin; .. 

When an embedded site requests storage access, there’s a permission grant. Scope could be to site or origin of the iframe. Site would mean similar resources on the same top-level could get access to SA. Chrome recently published some security considerations. Suggesting top-level site rather than origin. Need to resolve in conversation. 

#31 We don’t want to specify storage in the spec. Nor cookies. Defer to fetch. 

Domenic: so what does this API do, if that section is not filled out?

John Wilander: cookies

Domenic: but that section doesn't say how cookies are modified...

Johann: yes, we should do that.

Johann: Any other issues people want to raise?

More tasks not yet tagged. #37, exceptions vs undefined. We originally said let’s defer, but Anne suggested @@, I’ll make a PR. Mindful of user choice. 

Privacy and security considerations, we don't yet have them, so I filed an issue. We should. 

#5 aging out - how does SA expire. Need to write down the work. 

Wilander: two things that expire: user has seen the prompt and said allow, that shouldn’t expire; cookie access is tied to the document, if you reload the page…

Johann: if you reload, you get a new document. 

Kris Chapman: our previous issue with SA API has been that it needs user interaction. Where is that falling? Need user interaction, or browser-specific?

Johann: request-storage-access-for still requires user interaction

Kris: that’s a problem for Salesforce, and could also be problematic for FedID. How the SA is triggered could affect. 

kJohann: how to assure no user abuse, prompt abuse. Hard to guarantee that interaction.

Wilander: in our feedback on FPS, if there is a trustworthy and equitable FPS mechanism, could see some relaxation of interaction requirement within a set

Kris: one answer might be problematic. Flexibility helps. In different use cases, different signals might make more sense than user interaction.

Johann: think we should talk about use cases in more detail. 

Kris: From Salesforce perspective, SAAS use case. Could be helpful if there were an indication “I’m the first-party site”

Johann: finally, inline issues. In general, we have good alignment but haven’t gotten them all into the formal spec. 

… New things. SA API has been around for a while, unclear whether we should resolve new paths before it graduates. Or could be worked on after. 

Forward-Declare, for MS specifically for some authentication or identity use cases. The third-party is being navigated to for the first party… chain of navigation, then storage access. 

rSAFor, even more flexible. Call the API to request storage access for one embedded resource. 

Nick Doty: In privacy considerations, whether this is a clear permission that the user can understand. I think this should be graduated, and yet I’m concerned how we’ll explain it. 

Johann: There are a few approaches…. SA API has to ask the user, but leeway for browsers to set methods. E.g. Safari requires user to have a relationship with the site before even showing the prompt. Tech mechanism to assure at least some level of user understanding. For Google chrome, with FPS, perhaps a trust signal in verifiable list of related sites, public audits, would assure that grant is reasonable level of trust.  

Tess: In specs, we usually shy away from describing user interactions interface behavior. One of the challenges browsers will have is explaining, but we want to specify without constraining. 

Nick: my question is whether it’s possible to explain, not to specify the detail

Johann: include in the privacy considerations. 

Tess: when and if this goes to TAG review, wouldn’t surprise me if they ask about meaningful consent

Tim Cappalli: That’s why we’re driving toward specific APIs for specific use cases, so we can help users understand. SA for identity flows is a mid-term solution, not final, because of the prompt problem. 

Tess: when there’s a purpose-built API for a specific use case, it can be tailored. SA API as a general mechanism is for the use cases we didn’t envision with specific APIs, so yes, that can be challenging to prompt. 

Johann: running quickly through the rest of the [slide](https://docs.google.com/presentation/d/12BUOzSzXumx5zYWVbUDMQWx0O4pQR4w3j1QPNLlzM24/edit#slide=id.g1533373a0cf_0_200). Security improvements. Goal preventing cross-site iframes on the same page attacking the grantee of the SA request. 

Permissions policies.  Have to figure out how specific disallow interacts with grant. 

Better WPT tests

Erik Taubeneck: examples in location permissions, the spec doesn’t say what notification text. 

Theo Warren: re using notification of location as a good mental model. I’m dubious, because users might more easily understand location. 

<example of playstation.com asking for permissions to allow 2 other Sony domains (sony.com and sonyentertainmentnetwork.com) to access stored cookies>

Tess: Is this a good stopping point? 

Johann: We should soon be seeking TAG review. 

Tess: TAG hat on, we’d love to review.

Martin Thomson: List of issues doesn’t necessarily mean incubation is incomplete. They can move to a WG. 

Johann: final Q, what’s the standardization path? 

Tess: is it a bunch of patches on other specs, at the WHATWG? 

Domenic: then you definitely need WPT tests. 

Johann: This group has been a helpful place for discussion. Don’t want to lose that. 

Tess: would be happy to continue as discussion venue. Could talk with WHATWG about hybrid work mode. 


### CHIPS



Kaustubha Govind: Sorry I can’t be there in person. Cookies having independent partitioned state.ID extending cookie RFC. Sought feedback, including TAG review, TAG closed as satisfied. Requested Moz and Webkit review. Moz positive. We have a working implementation in chromium, origin trial. 

A few changes based on developer feedback. Initially wanted to scope to hostname, a bit more restrictive than cookies. Through the incubation process, we heard that was a big hurdle for developers to transition from cookies, so we removed. 

Next steps, we think we can resolve open issues. Looking for feedback on whether ready for graduation, and to where? Johann and Anne are hosting a “future of cookies” breakout Weds. 

John Wilander: want to make sure we don’t end up with WPT that require browser to support partitioned cookies by default.. Should be compliant to block 3p cookies. 

Kaustubha: 3d party dev has to opi in with partitioned attribute

Wilander: should be standards-compliant to block all 3p cookies, even when CHIPS is requested. We haven’t made up our mind if we’ll stick with blocked-by-default, or allow partitioned by default. We had partitioned cookies 2017-18, know it leads to more cookies. 

Kaustubha: re proliferation,.. Would be nice if we could align on partitioned. On chrome team, we’ve hoped that issues could be reconciled. 

Johann H: trying to understand the compliance question. If you have a partitioned test, then block, makes sense that test fails

Wilander: tests get used to make us look bad as “failing tests” even if we’ve said we think it’s a bad idea and choose not to implement. 

Johann: How do we write wpt for CHIPS that don’t fail if the intended behavior isn’t implemented? 

Domenic: sounds like a Web Platform Tests governance question

Martin Thomson: I think we’re at the point where what we have is testable. Could be worked in IETF 

We have options to disable cookies in these circumstances, we’re probably ok with WPT failing. What expectations exist on treatment of cookies on the basis of having this specified. 

Wilander: Safari has never allowed pure 3d party cookies. Suddenly starting to allow them may have compat issues, and proliferation of cookies could add issues. With opt-in, we might not see compat issues, but expect the proliferation issue. 

Kaustubha: I’m not aware of block-3d-party cookies tests, but think we need to write some for partitioning

Kaustubha: [#48](https://github.com/privacycg/CHIPS/issues/48), 10 cookies limit. [slide]

Because each 3d p site gets its own cookie jar, if you gave 180 cookies to each pair, could be huge. Looked at HTTP Archive, found 10 cookies reached the 90th or 95th percentile. But incubation process and dev feedback said that going from 180 cookies to 10 cookies was challenging, especially for CDN customers, where multiple customers on subdomains, sharing the limit. Added measurement to chrome. 

Histograms. We think samesite=none is an upper bound. 

[describing slide with histograms]

Some options to modify. How would these work?

Aram Zucker-Scharff: We do see unexpected truncation, usually not because an individual cookie is too long, but because there are too many cookies. Additional tooling around why cookies are rejected would be great. Probably few developers think of cookies in terms of size, rather, number. When they hear there’s a limit on #, they’ll likely use serialization methods that cause cookie contents to grow. With Web-dev hat, probably the preference would be to have smaller # of cookies with more data. Want to hear more from CDNs on this one. Often doing cache-splitting based on cookie content, how complex is that to process. 

Ben VanderSloot: Raw byte limit per partition could avoid weird incentives. 

Aram: +1

Sean Bedford: +1 . if you’re capping cookies, devs will go to “how can I get more”. You might see PSL being abused to get more cookies. 

Kris: Salesforce perspective, in favor of byte limit; with many domains, we’d be more in favor of more smaller cookies

John W: doing some quick math, 100MB cookies in memory would be a huge increase in memory

Tess: Thanks much to all presenters, editors, everyone

SamW: Thanks for joining in person and wearing masks. Hope you have a productive week!


## Attendees  (~84 people in the room + 20-some on Zoom):

1. Erik Anderson (Microsoft Edge)
2. Tim Cappalli (Microsoft Identity)
3. Ben Savage (Meta)
4. Paul Zuehlcke (Mozilla)
5. Benjamin VanderSloot (Mozilla)
6. Cameron Boozarjomehri (Mozilla)
7. Don Marti (CafeMedia)
8. Aykut Bulut (Google Chrome)
9. Brandon Maslen (Microsoft Edge)
10. Don Le (ARTICLE 19)
11. Johann Hofmann (Google Chrome)
12. Nicolas Pena Moreno (Google)
13. Cornelius Witt (eyeo) 
14. Ben Kelly (google)
15. Brian Lefler (Google Chrome)
16. Yifan Luo (Google Chrome)
17. Helen Cho (Google Chrome)
18. David Dworken (Google)
19. Elias Selman (Criteo)
20. Sam Weiler (W3C/MIT)
21. Theresa O’Connor (Apple, TAG, co-chair)
22. Maria Mandlis (Google)
23. Alex Turner (Google Chrome)
24. Chris Needham (BBC)
25. Mike O’Neill (Baycloud)           
26. Christian Biesinger (Google)
27. Alex Cone (IAB Tech Lab)
28. Rachit Sharma (IAB Tech Lab)
29. Sean Turner (sn3rd)
30. Max Gender (NewsCorp)
31. Yi Gu (Google Chrome)
32. Martin Thomson (Mozilla)
33. Lionel Basdevant (Criteo)
34. Matt Reichhoff (Google Chrome)
35. Sean Bedford (Meta)
36. Christine Runnegar (PING co-chair, remote)
37. Nick Doty (Center for Democracy & Technology)
38. Kaustubha Govind (Google Chrome)
39. Erik Taubeneck (Meta)
40. Zachary Tan (Google Chrome)
41. Jonathan Hao (Google Chrome)
42. Aloïs Bissuel (Criteo)
43. Vittorio Bertocci (Okta)
44. Risako Hamano(Yahoo Japan)
45. Arthur Sonzogni (Google Chrome)
46. Stefan Popoveniuc (Google Chrome)
47. Kaan Icer (Google Chrome)
48. Michael Knowles (Google Chrome)
49. Aram Zucker-Scharff (The Washington Post)
50. Wendy Seltzer (W3C)
51. Tim Huang (Mozilla)
52. Steven Bingler (Google Chrome)
53. Graham Mudd (Anonym)
54. Taiki Yamaguchi (Meta)
55. Daniel Huigens (Proton)
56. Eric Mwobobia (ARTICLE 19)
57. Kitior Ngu (Google Chrome)
58. Artur Janc (Google Security)
59. Shivani Sharma (Google Chrome)
60. Pragya Seth (Arkose Labs)
61. Sid Sahoo (Google Chrome)
62. Heather Flanagan (Spherical Cow Consulting)
63. Jeff Jaffe (W3C)
64. Hong Cai (BBC)
65. Russell Stringham (Adobe)
66. Matthew Finkel (Apple WebKit)
67. Jonathan Njeunje (Google) 
68. Jeremy Roman (Google Chrome)
69. Kris Chapman (Salesforce)
70. Jinkyu Seong (Hyundai Motor Group)
71. Jeffrey Yasskin (Google Chrome)
72. Tove Petersson (Google)
