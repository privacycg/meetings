Privacy CG Teleconference, 8 October 2020


[Agenda link](https://github.com/privacycg/meetings/blob/master/2020/telcons/10-08-agenda.md)
* Chair: Tanvi Vyas
* Scribe: Michael Kleber

<b>Proposal: [Site Groups / First Party Sets v2](https://github.com/privacycg/proposals/issues/22)</b>
* Paul Bannister
   * Heavily the same language as the original FPS proposal, taking into account things I and other people have brought up
   * (Mostly reading through beginning of linked doc)
   * Remove first-vs-third party as language; let's find a better term.  Site Groups is the proposed replacement
   * Browser UA policies are not in scope, but opinion = the "single organization's control" is not a valid element of the definition of a site group.
   * Using language of GDPR ("data controller") makes more sense than who is the organizational owner.  If multiple organizations form a single controller, could be a good boundary
   * Fairly large change: in FPS, any site within the set can get access to its own data when embedded in another site in the same FPS.  SG proposes that only the "owner domain" can get its own data when embedded.
   * Added into JSON file the requirement for a shared privacy policy and a SiteGroup name (which could be tied to branding on the site)
* Kaustubha
   * Thank you, great to see the interest
   * Q1 on ownership requirements difference: Is the primary motivation that ownership is difficult to verify, as written proposal says?  Is that an assumption we can validate?
      * Definitely ownership verification / understandability is one of the problems — Berkshire Hathaway owning lots of unrelated companies shouldn't confer special privileges, these amorphous conglomerates are not what a user expects.  Also consider other cases where part of a company gets sold to another: migration of ownership,  complexity of how a site works vs. the moment ownership changes.
   * Q2 on ownership privileges: Can owner reach into state owned by a member?  Restricting to owner embedded in member is an interesting proposal, e.g. works for Single Sign-On use case.  Not clear that this works for other use cases, like uploaded content (googleusercontent domain) and security boundaries (embedded in a Google doc).
      * Would like more feedback from other people on this point; not my area of expertise.  This is a way to increase privacy within the group — only one domain gets these privileges, rather than giving the priv away to everyone in the set.  Seems like a way to increase protection.
* John Wilander:
   * Thanks for bringing up the topics
   * Q1: Can a single domain be included in multiple Site Groups?
   * Q2: Can an owner domain be in multiple SGs?
      * A: Any domain can only be in one Site Group (either as owner or member)
      
<b>Work Item: [First-Party Sets](https://github.com/privacycg/first-party-sets)</b>

Issue: [Domains state purpose for being in First-Party Set](https://github.com/privacycg/first-party-sets/issues/28)
* Kaustubha:
   * Issue created from feedback in face-to-face.  Declaring use cases like SSO could make things clearer
   * Q's to John: How would requestStorageAccess use this?  Grant access automatically if in FPS?  Affect the UI of the FPS prompt?  Does this work for other use cases also, like joining across multiple ccTLD's?
* Wilander:
   * Currently in WebKit, storage access is granted to the page, only lasts until the page goes away.  If it's an SSO domain, then this could be relaxed — lasts for th whole browsing session, or some period of calendar time.  Also could affect language in requestStorageAccess prompt, where we could explain the purpose — "this is for SSO", "this is my ads domain".  Some worry that ad blockers etc. might trigger on those purposes, so not clear what the right answer is.  For category like CDN, could be a hint to the browser to make caching better.
* Kaustubha:
   * Brings up hard questions like localization, and how to make something like "Single Sign-On" a phrase many people could understand
* Paul Bannister:
   * Instead of enumeration every use case, maybe SSO could be granted its own special status, given its importance and widespread use
   
   
Issue: [Enforcement against formation of large sets](https://github.com/privacycg/first-party-sets/issues/29)
* Kaustubha:
   * Can sets get too large?  Important for user's understanding (can't understand 1000 domains!), also for preventing abuse.  Talked about ways to limit or to dis-incentivize large sets.
   * Would like to hear thoughts from the group on limits and incentives.
   * ccTLD could be treated differently than others in terms of a numeric limit
   * Other types of counter-pressure: have storage limits, which are per-domain today, instead be per-FPS.  (Cookies, storage, etc.)  Another possibility is entropy limits, e.g. Chrome's Privacy Budget where entropy budget gets deducted from with each thing that gives away fingerprinting information; if FPS is the privacy domain, then entropy budget could be shared.  Probably other APIs as well.
* Lisa LeVasseur:
   * In our research, people have 1:1 relationships with a particular service at a particular time, so conflating these across different services is very tricky.
   * Legal layer: When I'm on a domain, I'm bound by the privacy policy of the service I'm using at a time.  Is FPS an aggregation of legal policies also?  Worried that this could confuse people and legal obligations.
   * SSO across an organization's entire portfolio can get confusing
* Kaustubha:
   * (NB: Tess wrote a great document about using better terms than first/third party - https://tess.oconnor.cx/2020/10/parties)
   * People may well not know what different domains exist.  When you upload a large video to Google Drive, who knows what domain it's stored in.
   * Separate brands (Facebook/vs/WhatsApp) also a good question
   * I wonder if these can be solved by limiting the size of a set.  Trying to balance usefulness with the reality of how the mechanics of web site construction works.
   * Re. legal question, IANAL, but it's been floated to require common privacy policy from all the domains in a FPS, this seems to answer some part of that
      * Lisa: Yes, that seems necessary
   * SSO is one of those nice situations where the user is consciously taking an action (Sign In with FB button), clear what's going on.
      * Lisa: Me-2-B relationship: when I (the user) asks to be remembered, recognized, responded to.  Happens only once the user recognizes some benefit from doing so; often [always] coincides with legal obligations (ToS, contractual moment).  Needs to be clear to the individual that you are being r/r/r by multiple services, more than just the one you're interacting with right now.
* Paul Bannister:
   * What are the specific threats addressed by limiting size of group?  I don't see what risks are increasing for a set of 10 vs set of 1000.
   * Kaustubha:
      * This is why I'm more inclined towards resource limits — hard to find a number, but tying together / shared fate is about incentives.  But I'm fine with a limit too, we've heard it from other browsers.  Seems OK to start with a stricter set of criteria, and maybe loosen over time if need arises.
   * Wilander:
      * When we first had the idea of Storage Access API prompt, we thought about asking for multiple domains all at once.  That put a cap on a set of domains: need to prompt for all the domains at once, so e.g. for a prompt on a phone, ~5 domains seems plausible to present to the user.
      * Maybe that's the wrong approach; more important to know the sites you (the user) actually visit, vs. the set of all domains owned.
      * Agreed that we need to break out ccTLD's — that's something users could genuinely understand.  (though: what if there's some small country where Google does not own google.whatever?  Corner case.)
      * We think that if sets are allowed to be large, then they probably will be large, and so a browser like Safari probably cannot grant any relaxation of rules to such a large set of sites all at once, since don't believe it can comply with user understanding.  Reasonable to have a set that users can understand.
* Robin Berjon:
   * This seems to be headed in exactly the wrong direction.  From our (NYT's) perspective, our goal on privacy is to enforce purpose limitation — to split up a domain into multiple identities, rather than joining up across domains.  Agree that it would be necessary but not sufficient to have same privacy policy.  SSO is not indication that there's a conscious intent to share identity across contexts; people are surprised to find that they are recognized across contexts even when they give out a join key like email address.
   * I'm skeptical of any way to make this understandable to users.
   * (Google docs comment - Daniel Appelquist: +1 to robin's comments)
* Kaustubha:
   * We have discussed UI, perhaps in conversations you weren't in; can bring this back in a future meeting.  Need user understanding to be top-of-mind, though of course that's the job of browsers, not specs
   * Seems like you're assuming that a domain acts as a natural boundary, i.e. DNS has some special power.  This is challenging that notion — DNS isn't the right boundary for the real world, and we want to find the thing that is
   * (Google doc comments on the DNS sentence - 
       * Daniel Appelquist: This is dangerous from a web architecture perspective.
       * Davi Ottenheimer: agree. names are a historic standard of boundaries. family name (TLD), given name (server)... and having a register of those names (DNS) makes as much sense as any catalog of identities under an authority.
   * Robin:
      * Would need to see a proof that a viable UI is even possible.
      * DNS is indeed cracking and breaking.  Would be more valuable to break domains apart.
* [Kaustuba audio breaking up] — let's talk about UI again later?


Issue: [First-Party Sets naming](https://github.com/privacycg/first-party-sets/issues/27)
* Let’s move conversation to github, pick up next time.


<b>Work Item: [Storage Access API](https://github.com/privacycg/storage-access)</b>

Issue: [Require reload after granting requestStorageAccess to get at unpartitioned storage](https://github.com/privacycg/storage-access/issues/62)
* Wilander:
   * Maybe quick, just calling attention to the per-frame vs per-page storage access discussion back into question.
* Josh Karlin:
   * We are still very interested in the topic, very nervous about per-page — sudden change of state for an iframe that didn't know to expect it seems like a big attack surface, frames suddenly writing to different storage than they expected.
   * Discussion came up of pre-render: for pre-loading pages to get instant navigation, but need un-credentialed load for privacy reasons until the user actually clicks.  Can we use requestStorageAccess as the gate between the un-credentialed vs post-getting-credentials phase of a pageload?  Brings up some of the same concerns as above — could requestStorageAccess instead return a storage bucket, which the site could then choose to use?  Invisibly swapping state has a bunch of dangers.
* Wilander:
   * Per-page capability is now landed in WebKit code; both implementations are available, so we can make the choice, but there is a time limit
* AnneVK:
   * Also concerned about cross-origin grants.  Maybe better if impact is restricted to the origin that made the request, rather than affecting the whole site.
      * Wilander: probably could make that change
   * Re. prerender, seems like you want per-page, the whole page needs its state
      * Josh: Pretty different use case, e.g. even it's a thing in the top-level page.  But could use the same API as a gate


<b>Work Item: [The IsLoggedIn API](https://github.com/privacycg/is-logged-in)</b> (MOVING to next meeting, please engage in github in the meantime as these are new issues)
* Issue: [Support for federated logins, or the ability to transfer IsLoggedIn](https://github.com/privacycg/is-logged-in/issues/35)
* Issue: [Supporting display name and avoiding misuse of them](https://github.com/privacycg/is-logged-in/issues/36)


<b>Proposal: [Standardizing Do Not Sell Proposal](https://github.com/privacycg/proposals/issues/10)</b>
* Spec link:https://globalprivacycontrol.github.io/gpc-spec/
* Ashkan Soltani (Ash):
   * I was at the FTC and came up with DNT at that time (2010)
   * In 2018 I contributed to the California CCPA, which explicitly permits the right of a user to delegate another person to invoke their rights.  We intended that to be DNT, but the Atty General took a different view (was being widely ignored, didn't clearly communicate intent).  So we want to fill in that hole in what CCPA expected to exist
   * Pulled together a few groups interested in designing the draft proposal, get feedback, consider standardization.
   * Strange place since CCPA law contains some requirements, though we don't want the standard to be tied just to this one law.  Tricky since different laws may have different intents being expressed.
* Sebastian Zimmeck:
   * This seems like a great group for discussing this, good collection of people and perspectives.  Please let us know what you think.
* Tanvi: Due to time constraints, today probably should just be an intro, and full discussion will have to wait for another time
* James Rosewell:
   * There was a debate about policy in W3C in 2016 which provides some insight into the W3C’s role in policy (https://www.w3.org/2002/09/wbs/33280/techpolig2/results). [NOTE: that is a W3C-Member-only link, as a charter review.] It seems that the position at the time was that the W3C should not progress a policy interest group. I would welcome revisiting this question before progressing this proposal. Also GDPR needs to be considered.
* Mike O'Neill:
   * Why no consent, i.e. the opposite of Do Not Sell?  The one thing you want in Europe is a consent signal, GDPR PoV.
      * (Google doc comments on consent signal - Davi Ottenheimer: great question of what the default is when the header is missing or corrupted.
   * Ash: New law in CA this November.  Want something that is extensible to other jurisdictions.  Want a generalized control, though we did want it to fit with CCPA that required this one specific thing per language in the law.
      * (Google doc comments on Ash's statement - Christy Harris: Note CCPA is law now. Ballot initiative in November will determine if new law (CPRA) passes and will become effective in 2023.
* Aram Z-S:
   * Anyone on the publisher side: we're adopting this at Washington Post, and we're happy to talk to you
* Tess:
   * Surprised the Sec-GPC header doesn't use Structured Headers.  Issue already filed
   * When a structured header represents a boolean and the header is absent, the boolean is true, which maybe is not what you're looking for
* Robin:
   * We (NYT) are implementing this signal, using it for both CCPA and GDPR.  Would love to share experiences.




<b>Any Other Business</b>
* TPAC Agenda and meetings that may be of interest to this group
   * Link to how to sign up for TPAC: https://www.w3.org/2002/09/wbs/35125/TPAC2020/
   * Lot of different groups having breakout meetings.
   * kleber: Call folks attention to the meeting on Tuesday of next week the 13th.  Turtledove + Sparrow proposals.  ("Web Platform Incubator CG - TURTLEDOVE + SPARROW proposals: 13 October 15:00-16:00") Originally in the WABG and now in the WICG.  We’ll have an hour to talk about them.  May be interest here because questions on how to allow a somewhat trusted server side component to an API that is dedicated to privacy rather than relying solely on on-browser processing. 
   * Sam: register for TPAC. Encourage folks request your own breakouts
   * Ben: “Secure Multi-Party Computation” at the Improving Web Advertising BG Meeting. We’ve discussed a lot in the WABG.  Discuss what it is and what privacy guarantees it comes with. Topic to discuss: Firefox PRIO (built on MPC). Also plan to discuss the Chrome Aggregate Reporting API - discuss the privacy and security guarantees of such a system - especially with an eye to the combination of MPC and global differential privacy. Date not locked down, will be on Oct 21 or Oct 22. https://github.com/w3c/web-advertising/blob/master/meetings/TPAC2020.md#draft-agenda 
* F2F Survey Results - Push to next week/time.
* Welcome to a few new members here.  We are likely not going to have time for intros, but it would be helpful if you could add a sentence or two about yourself in the attendee list if you are new to the group today.
* Reminder about ad-hoc meeting requests - file an issue here https://github.com/privacycg/meetings/issues/new and if there is interest, chairs can help set up and facilitate.



Attendees (sign yourself in):
1. Anne van Kesteren (he/him; Mozilla)
2. Allan Spiegel
3. Aram Zucker-Scharff (The Washington Post)
4. Arnaud Blanchard (Criteo)
5. Arthur Edelstein (Mozilla)
6. Ashkan Soltani (Georgetown)
7. Ben Savage (Facebook)
8. Brad Slayter (DuckDuckGo)
9. Brad Rodriguez (Magnite)
10. Brian Campbell
11. Cezary Cerekwicki (Opera) 
12. Charan Mann (ForgeRock) 
13. Chelsea Gong
14. Chris Needham (BBC)
15. Chris Pedigo (DCN)
16. Chris Wilson
17. Christy Harris (Future of Privacy Forum)
18. Daniel Appelquist (Samsung)
19. Davi Ottenheimer (Inrupt) : about me → https://www.flyingpenguin.com/?page_id=87 or https://inrupt.com/meet-the-inrupters 
20. David Benjamin (Google)
21. David Harbage (DuckDuckGo)
22. Devin Rousso (Apple)
23. Diarmuid Gill (Criteo)
24. Eric Mwobobia (ARTICLE19) 
25. Erik Anderson (Microsoft)
26. Erik Taubeneck (Facebook)
27. George Fletcher (Verizon Media)
28. Harneet Sidhana
29. Henry Lau (Privolta)
30. Jack Frankland
31. James Rosewell (51Degrees)
32. Jason Kint
33. Jason Nutter (Microsoft)
34. Jeddy
35. Jeff Bezos (Amazon) Really ?
36. Jeffrey Yasskin (Google Chrome)
37. jlukas
38. Joel Meyer (OpenX)
39. Joel Odom (Salesforce)
40. Johann Hofmann (Mozilla)
41. John Wilander (Apple WebKit)
42. Josh Karlin (Google)
43. Justin Brookman (Consumer Reports)
44. Kate Cheney (Apple)
45. Kaustubha Govind (Google)
46. Kushal Dave (Scroll)
47. Lisa LeVasseur (Me2B Alliance)
48. Mark X
49. Marshall Vale (Google)
50. Max Gendler (NY Times)
51. Melanie Richards (Microsoft)
52. Michael Kleber (Google)
53. Mike Lerra (Google)
54. Mike O’Neill (baycloud)
55. Naima Conton
56. Nick Doty (UC Berkeley)
57. Paul Bannister (CafeMedia)
58. Paul Jensen (Google)
59. Peter Dolanjski (DuckDuckGo)
60. Przemyslaw Iwanczak
61. Robin Berjon (NY Times)
62. Roten Dar
63. Rowan Merewood (Google) 
64. Russell Stringham (Adobe)
65. Ryan Avecilla (Neustar)
66. Sam Weiler (W3C/MIT)
67. Sean Harrison (Google)
68. Sebastian Zimmeck (Wesleyan University)
69. Steven Valdez (Google)
70. Tanvi Vyas (Mozilla, co-chair)
71. Theresa O’Connor (Apple, co-chair)
72. tbergema 
73. Theodore Olsauskas-Warren (Google)
74. viraj
75. Wendell Baker (Verizon Media)
76. Wendy Seltzer (W3C)
77. Brad Lassey (Google)
78. Brian Campbell (Ping)
79. MG
80. Allan Spiegel (Adobe)
81. Michael Kleber (Google Chrome)
82. Lucas Adamski


