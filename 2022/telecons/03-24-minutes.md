# Privacy CG Teleconference
* Chair: Tanvi Vyas
* Scribes: Kaustubha Govind, Nick Doty, Tanvi Vyas

# Agenda
* [Navigation-Based Tracking Mitigations](https://github.com/privacycg/nav-tracking-mitigations)
   * [PR #18 - Add description of fourth Brave bounce tracking mitigation](https://github.com/privacycg/nav-tracking-mitigations/pull/18) 
* Any other business
   * [First-Party Sets](https://github.com/privacycg/first-party-sets) - Update on [PR #84](https://github.com/privacycg/first-party-sets/pull/84)
   * [Cross-Site Cookies Standardization Ad Hoc](https://github.com/privacycg/meetings/issues/16), Thursday March 31, 2022 9:00 am PT

# [Navigation-Based Tracking Mitigations](https://github.com/privacycg/nav-tracking-mitigations) - [PR #18](https://github.com/privacycg/nav-tracking-mitigations/pull/18) 
* https://privacycg.github.io/nav-tracking-mitigations/#mitigations-brave 
* Pete: Mitigation section text is mostly about what is shipping in browsers. New PR talks about a mitigation that Brave is shipping. SImilar to what Mozilla/Gecko does where when you navigate to a suspected bounce tracking site, clears data. Difference is that we do it 30 seconds after navigating away. Gecko does for 24 hours.
   * We use community-sourced lists based on crawled data. Mozilla uses Disconnect list.
   * How this interacts with other nav tracking protections: (a) Strip-off query parameters based on a curated tracking param list (b) Unlinkable tracking (c) Known aggressive tracker, we allow you to navigate away or create an exception. (d) a certain category of known tracker; we’ll bounce you over the intermediate tracking navigation and take you to the destination directly.
   * Brian Lefler: The other differences seems to be that you exempt on state, Firefox exempts on activation. Would you be able to align on thar?
   * Pete: This is a stop-gap measure. When we have the ability to have a per-navigation storage (instead of session/persistent), if you could have different sets of navigation with temporary storage area. Would like to be able to not clear all storage on the site; but just the state stored on that bounce navigation.
   * John Wilander: We have a similar protection feature. Timer checks once-an-hour, mostly a performance thing, since it involves a disk operation. Also don’t do this if user is actively using the browser. For the redirect chain which involves collusion; we realized that you could have an intermediate tracker that is only intended for a specific pair of domains; where it doesn’t have large fan-out, but the fan-out happens later. We also detect that.
   * Pete: About the chain thing; I assume we catch that in the crawl data and capture that in our lists; but need to confirm.
   * JohnW: What partition key do you use for the storage?
   * Pete: We currently use the first-party storage itself; but we ideally want to use distinct storage area for bounce navigation; vs. direct user navigation to that site.
   * Paul@Mozilla: Was wondering how you compile the lists, and whether you have a policy on what gets added to the list
   * Pete: [Link to paper](https://arxiv.org/abs/2203.10188) that describes what gets crawled, with a human pass on top of it. Also source from crowdsources list; some contribute directly to Brave. A combination of people-provided and crawled lists.
      * We would never apply these on same-site navigation.
   * Heather Flanagan: FedIdCG is very interested in things that touch on link decoration. Would you and your co-editor come chat with us about this?
      * Pete: Sure
   * BrianL: Brave/Mozilla, would you consider rolling this out for all cross-site navigations, not just based on a list? Wondering why you don’t do that today?
      * Pete: Only based on user activation, or previous state?
      * BrianL: yes
      * Pete: The reason we used lists is primarily ease of implementation, but the real default state to get to is that the moment you leave the site, we would clear the states for all sites unless the user says they want to keep it. This would something the user agent enforces on the site.
      * Brian: Okay; so list is to limit breakage; but conceivably, you would do something like this.
      * Pete: Yes, that’s the goal two steps out.
   * Brian May: Assume that list provides pretty good guardrails; but I was wondering if you are seeing any problems with the approach.
      * Pete: The list sometimes has to be modified; not a perfect record, but works for most part,
      * BrianM: What was going wrong there?
      * Pete: For other mitigations, such as query param stripping, different site were using the same query param; so we removed the ability for someone to unsubscribe from a mailing list. We had to back this out.
      * BrianM: Yeah, was wondering if there were unintentional consequences.
      * Pete: Can share [Link] to this on Slack.
      
# Any Other Business
* [FPS PR 84](https://github.com/privacycg/first-party-sets/pull/84)
   * Harneet: We discussed this ~4 weeks ago. Just wanted to update that we wanted to push the update at the end of the week. If you have outstanding feedback, please provide it.
   * Kaustubha: Anne and Martin had given feedback and are planning to address that first.
   * Nick Doty: Some of the conversation has changed a lot over time. Some of the conversation is about whether it is a good application, good for users, etc. Those questions remain, but makes sense to push PR if intention is to document.
   * Harneet: Correct
   * Nick: That doesn't mean that everyone supports the application.
   * Kaustubha: We want to make sure that we’re capturing the different browsers positions correctly, not necessarily the position of the community group.
   * Nick: Curious about SSO, and exceptions to navigational tracking blocking. How is that different for FPS from other types of federated login situations.
   * {switched to Tanvi scribing}
   * Kaustubha: I think in terms of login - 3 models at least.  1) federated login, for most part seems to conform to openid type protocol.  Most federated login providers including enterprises conform to that, sometimes differentiating work flows.  Federated auth like google sign in, okta, microsoft identity platforms has a few.  OpenID, oauth stuff.  2) first party single sign on - okta provides customer specific end point.  Hiking trail maps website, may be customer of okta.  Dedicated endpoint on okta.  And then essentially click login, go to hikingtrails.okta.com and that is considered cross site.  Okta is providing identity for hikingtrail.  (Vs: Different then google sign in and federated auth.  For federated identity - you are using that as your sign in - dedicated to the site.  Ex: twitter or google) 3) first party sign on, large company, own independent.  Sony, playstation, google, microsoft. 
   * Kaustubha: I think of hiking/okta case, could have more than one domain.  Like bikingtrails and they are the same company. Potentially use the same okta endpoint for hiking and biking trails.  First party sets could be useful for both.  We haven’t figured out exactly what the okta case is going to be using. (Another on storage partitioning where we need to work this out?).  Google.co.uk, etc.  Those don’t conform to oauth.  Like the wild west.  Federated CM has had conversations with folks that built FP single sign on and its a very unconstrained problem.  Don’t necessarily conform to oauth.  FPS could be the primitive for that (Case 2 above).  A lot happens via top level redirects.  With bounce tracking preventions, the login domain might show up as a bounce tracker, and could break auth for users. (maybe john wilander could speak to that).  But with FPS, the website provides info that says these are the domains being used for login among the universe of domains they have, potentially the browser could say - i see this bounce tracking redirect happening but i know its for login, so maybe surface that to the user or just accept it (up to user agent, haven’t worked out details).  Treated differently than bounce tracking in some way.
   * Nick: I can see how link decoration could apply.  Some reason that applies more for cases when need FPS.  Seems like that would apply whenever.  Is there some reason a FPS is especially useful here.  Does the tracking look different if 4 websites trying to login to instead of 1.
   * Kaustubha: Okta is clearly not the same company as hikingtrails and bikingtrails.  In the future… CHIPS… if the top level site is in a FPS, the partitioned key could be the FPS owner.  If we allow hikingtrails.okta.com to share same partition with hikingtrails.com and bikingtrails.com.  As long as the state was partitioned… If went to okta.com in a different context, no way to correlate the state.  Multiple software as a service providers that cater to folks with multiple domains. FPS - if you have domains that are really the same party, then in set.  But we will allow applications that work with CHIPS.  Still use third party provider to make these restrictions, but {missed}.  FPS is a primitive, listen I have multiple domains but these are the same entity, user understands them as same entity, whether third party or in house providers.  FPS acts as a primitive.  When you do privacy mitigations, treat them as one privacy unit.
   * NIck:  I can see how … Still don’t see why need FPS here.  If want bikingtrails, can click login button again.  Maybe a lot quicker.  Not sure why need FPS
   * Kaustubha: if you have multiple accounts with hikingtrails.com.  Maybe better example is consent management.  Rather than login, consider consent management.  Cookie/GDPR prompt only once per party.  CHIPS and FPS could work.  What I was trying to say with okta example, is that FPS enables first party single sign on.  If not building own in house identity management, need to build something for okta to build into this new world.  Website itself doesn’t have to do much.  Way web is going - FPS allows us to have the fps do the minimal amount of work. And third parties will have to adopt new apis etc.  For first parties, If multiple domains, tell us, and then things will keep working.  Moving whole ecosystem along is a multi-decade thing otherwise.  Also see FPS as a way to make this happen sooner, rather than decades for whole privacy model to move.  One of the questions on the PR to Martin and Anne - ….
   * Nick: appreciate the context.  Glad thinking through login systems through third party or federated provided.
   * {back to Nick scribing}
   * Vittorio: just high-level comments. Taxonomy of logins: Fed ID CG has a table of different use cases (https://github.com/fedidcg/use-case-library/wiki/Primitives-by-Use-Case). First-party/federated may not be relevant for the provider of authentication. Third-party cookies might not be needed for login, but are used for other purposes. SAML highly reliant on redirects. If you have new APIs, it would be much harder to move them. Invitation to a session with Fed ID CG, or a separate discussion. FPS is an interesting proposal, but hard to cover all the scenarios – outsource something to Visa, but it might bounce you around to multiple domains. Protocols are designed so that there is flexibility for implementation of each part along the way. Not easy to pre-determine the full list of domains that will be interacted with to define statically for a first-party set.
   * Lee: re: taxonomy/scenarios, hiking trails site might embed their content in other sites in order to make money in using their services nested in other domains/sites. Even a first-party site will have third-party applications as part of a potential business model.
   * Kaustubha: ‘authenticated embeds’ as a use case; Storage Access API as a way to work around that because of interaction by the user.
   * Lee: can’t rely on acknowledgement by the user. We all work for large companies, and users can’t/shouldn’t acknowledge in every case where content is embedded.
   * Kaustubha: Chrome would generally recommend enterprise policy for enterprise use cases, which will involve lots of private domains, allow-listing, etc.
   * Lee: good tool, but my concern about enterprise policy is ~~~. variation between browser implementations makes it difficult to give consistent implementation or explanation to end users about what they need. Browser vendors need to agree.
   * JohnW: not the case that users are prompted on every page, browser remembers the user’s choice.
   * John: if FPS would be used, should also describe which domain is used for single-sign-on for that domain. Then we could combine that with isLoggedIn. And then we could relax navigation tracking blocking in those situations where the user is logged in for that particular domain. Proposal from Microsoft to access cookies under a particular relying party, as a new of using the Storage Access API.
   * James: for GDPR implementation, first and third-party aren’t a thing. Should only consider sole and joint data controllers. Have a PR/text proposal change to achieve this. In any case we should be adopting GDPR definitions of joint controller quickly and align to GDPR language.
   * Tim: enterprise policy cannot be a dependency, should only be a fallback.
   * Anne: Firefox currently uses entity lists to protect users from tracking on the web, but not a long term solution. Long-term intend total cookie protection to be rolled out to all users that won’t have entity lists. Should compare the long term aspiration of what we ship, which doesn’t include entity lists.
   * Kaustubha: what about lists of trackers?
   * Tanvi: for total cookie protection, partition all third parties. For tracking protection/entity lists, cookies wouldn’t be blocked if on the same list, but will still be partitioned.
   * Kaustubha: Chrome will not be using blocklists, want features just to apply to the entire Web, which makes site compatibility issues more likely. If total cookie protection is rolled out more widely, might encounter some breakage. That would be closer to our goal for third-party cookie deprecation scheduled for 2023. Beyond third-party cookies, considering navigational tracking and comprehensive protections against tracking. Having something like FPS will help us constrain the problem/use case.
   * Tanvi: for FPS, there are multiple use cases listed. But for total cookie protection, we would apply partitioning completely across the web. Not sure what that will mean for other features (like blocking), but interested in web compat in those cases. Safari’s experience show it’s possible.
   * Kaustubha: we’ve talked to lots of developers, including sites that just tell Safari users to use an app on iOS. We just might be diminishing some cases/developers.
   * Tanvi: if we can surface those breakages, so that we can all see them and figure out solution.
   
   
* [Cross-Site Cookies Standardization Ad Hoc](https://github.com/privacycg/meetings/issues/16), Thursday March 31, 2022 9:00 am PT
   * [chairs] Will update the calendar today. Keep in mind that we are in a time vortex. We mean this time next week Pacific Time.
   * Tanvi: Would be great if each browser vendor could describe their approach to mitigate cross-site tracking; and what their plans are coming up. So do hope to see participation from user agents.
  
# Attendees
  
  
1. Allan Spiegel (Adobe)
2. Aman Bhurji (CJ Affiliate)
3. Anne van Kesteren (he/him; Mozilla)
4. Anuvrat Singh (Amazon)
5. Ben Kelly (Google Chrome)
6. Brian Campbell (Ping)
7. Brian Lefler (Google Chrome)
8. Brian May (dstillery)
9. cbiesinger (Google)
10. Chris Needham (BBC)
11. Christine Runnegar (PING co-chair) (second half)
12. Dan McArdle (Google)
13. Dan Sinclair
14. Daniel LaLiberte
15. Don Marti (CafeMedia)
16. Emily Lauber (Microsoft)
17. Erik Anderson (Microsoft Edge)
18. Erwan Loaec
19. Garima Dhingra (Google Ads)
20. George Fletcher
21. Grant Nelson (TripleLift)
22. Harneet Sidhana (Microsoft)
23. Heather Flanagan (Spherical Cow Consulting)
24. Helen Cho (Google Chrome)
25. Hitesh Lad (CJ Affiliate)
26. James Rosewell (51Degrees)
27. Jason Nutter (Microsoft)
28. Jeegar Shah (CJ Affiliate)
29. Jeffrey Yasskin (Google Chrome)
30. Johann Hofmann (Google Chrome)
31. John Sabella (PubMatic)
32. John Wilander (Apple WebKit)
33. Kaan Icer (Google Chrome)
34. Kadin Van Valin
35. Kaustubha Govind (Google)
36. Lee Graber (Salesforce / Tableau)
37. Matthew Liu (The Washington Post)
38. Nick Doty (Center for Democracy & Technology)
39. Paul Zühlcke (Mozilla)
40. Peter Kotwicz (Google)
41. Peter Snyder (Brave, PING)
42. Russell Stringham (Adobe)
43. Sam Weiler
44. Sam Goto (Google)
45. Sean Harrison (Google)
46. Stefan Popoveniuc (Google Chrome)
47. Tanvi Vyas (Mozilla, co-chair)
48. Tara Whalen (Cloudflare) 
49. Theresa O’Connor (she/her, Apple, co-chair)
50. Tim Cappalli (Microsoft Identity)
51. Tim Huang (Mozilla)
52. Vittorio Bertocci (Auth0 | OKTA)
53. Wendell Baker (Yahoo)
54. Wendy Seltzer (W3C)
55. Yi Gu (Google Chrome)
