# 13 October Privacy CG Meeting
* Chair: Erik Anderson
* Scribe: Cameron Boozarjomehri (first 30 minutes), Nick Doty (second 30 minutes)

# Agenda
* [Bounce tracking mitigations #34](https://github.com/privacycg/proposals/issues/34)
* [CHIPS](https://github.com/privacycg/CHIPS)
    * [10 cookies per cookie jar limit #48](https://github.com/privacycg/CHIPS/issues/48)
    * [Add explicit behavior requirement for "Partitioned" attribute on First Party cookies #51](https://github.com/privacycg/CHIPS/issues/51)
* [Storage Access API](https://github.com/privacycg/storage-access)
    * [Improving the Storage Access API security model #113](https://github.com/privacycg/storage-access/issues/113)
    * [Consider restricting storage access scope back to per-frame #122](https://github.com/privacycg/storage-access/issues/122)
    * [Consider requestStorageAccessFor Method #107](https://github.com/privacycg/storage-access/issues/107)
* Any other business

## Notes

### Bounce tracking mitigations #34
* Ben Kelly: Would like to move to the community group (have WICG and PrivacyCG). Is it a good fit for Privacy? One concern is this doc/workstream covers all nav tracking and we wanna keep bounce tracking separate in privacy CG.
* Erik: Slides are linked from the github issue. Ben covers some ideas and breakage mitigations
* John Wilander: Apple Webkit likes it in Privacy CG. Wanted to mention this space is moving a lot because trackers move a lot. We want to standardized an approach to get us where we wanna be privacy wise. Don’t want to limit ourselves, need to protect against tracking. Need to recognize this is a competitive space. Vendors may have their own ideas they want to keep working on before bringing to the community.
* Nick Doty: I agree it should be in Privacy CG. Question: Are we suggesting this is a set of recommendations or is this just an additional doc for bounce tracking specific mitigations.
* Ben Kelly: Our intent is to converge on a set of mitigations targeting bounce tracking. We are similar to Firefox but we want consensus on Bounce Tracking Mitigations that can be implemented on an interoperable way. Might not be a separate spec, just something incorporated into other specs. Looking for normative recommendations. Are specs/standards proscriptive or descriptive. Spec describing interoperable state. Competitive space, don’t want to restrict us. Always room for improvement. Incubation is important. Long Term goal to make browsers interoperable from browser interoperability standpoint.
* Ben Vandersloot: Mozilla is supportive of this as an effort to build a common baseline for bounce tracking. Makes sense as separative work item since navigational tracking is huge so maybe good to target bounce tracking.
* Erik: Microsoft agrees on this logic. Could be a separate repo or part of a broader repo. I’ll find a better place in the privacy CG.
* Ben Kelly: also great because means separated into own repo.

### 10 cookies per cookie jar limit #48
* Dylan Cutler: Issue 1, limit per domain per cookie. 10 is too small but we are trying to limit cookie jar memory impact. We are proposing to instead limit cookies by amount of memory needed for cookies. 10kb per domain per partition. There may still be 180 cookie domain limit, and would still enforce 4kb per cookie limit.


### Add explicit behavior requirement for "Partitioned" attribute on First Party cookies #51
* Dylan Cutler: Places where domain setting cookies same top level site. Do we still respect or ignore partitioned attribute. Those cookies only available on that site. Reasons to no ignore the partitioned cookies. For same site none cookie, can be accessible through Storage Access Api. Cookie set to be available only in site creed, makes sense to respect the partitioned attribute. Don’t wanna give different cookie behaviors for same site embeds, never clear if in a cross site or same site embed context. Partitioned attribute should be respected anyway, no special carve outs.
* Lee Graber: For clarity, if I embed a frame from same site as parent I will still get unpartitioned cookies also?
* Dylan: if req from same site embed or top level doc, then all cookies shared, not just “partitioned”. In order to prevent leaking, we need to treat partitioned keys as unique. This isn’t unprecedented, can set 2 cookies to same name set in cookie header.

### Improving the Storage Access API security model #113
* Johann Hoffman: Investigation started as announcement that chrome is doing storage access api to make 1st party sets more compatible across browsers. Started an investigation into security props of SAA. Found some concerns around adjacent cross origin iframes where access given to another iframe on site. Say Amazon merchant iframe and ad iframe on same site, could give ad iframe full credentials. Have APIs for if credentials sent or not. Reveals a lot about architecture of top level sites. Made a few suggestions: most actionable is restrict access so not storage and site (in chrome and Firefox). Webkit is supportive as well. Protects against cases with storage access grant (drive.google.com linking to payments or some other similar google product). Additionally, could require iframes to have permissioned properties attribute to same implicit per page mechanism. Would always need to enlist 1st party to allow list them. Need to specify on an image tag an attribute the enforces course for a specific request so server has to opt-in to be informed about req origin. Leads into per frame. Martin Thompson also suggested opportunity for request storage access going back to “per frame” again. Single iframe get storage access granted. All iFrames would need to opt-in explicitly. This is largely compatible with what people are doing

&lt;scribe switch>

* John Wilander: thanks for summary. No longer co-editor of SAA, replaced by Anne. This seems better aligned with our original implementation. Motivation for site vs. origin is compatibility. Should Storage Access be modern/new with partitioned data, or should it be a legacy bridge for keeping old stuff working? Sounds like additional research on security implications is that it should be more narrowed. Only worry is compatibility issues remaining. Google’s federated login mechanism uses five different iframes and would rely on site rather than origin scope. Would need that to be migrated to just use accounts.google.com for example. WebKit implementation clears granted storage access when an iframe navigates or gets detached, those should probably be included in spec to protect against security attacks. Not per-frame, because otherwise you could use a redirect chain to do something bad. User grant could still be fairly wide (site getting access to unpartitioned cookies) but the technical grant could be per origin.
* Nick: thanks for doing the security review. It does seem like requestStorageAccessFor adds to the risk because a top-level site can do this request that the embedded site may not be expecting and then do some CSRF attack. I wonder if that’s a general rule– doing a request on behalf of someone else is likely to lead to that sort of vulnerability. Maybe we shouldn’t use that model. Maybe the web platform should not have the case of asking for permission on behalf of someone else– I don’t know if that’s a “confused deputy” but it seems like it could lead to that sort of attack.
* Johann: request storage access with per-frame, should resolve the security concerns because these are all possible on the web today. Concerns would still exist for requestStorageAccessFor. Problem of requestStorageAccess is that it’s hard to use to fix compatibility issues – was hard for Firefox to develop those shims. Has to be initiated or cooperated with the top-level origin in some way. Feedback on requestStorageAccessFor – could there be consent/cooperation from the third-party on whose behalf access is being requested? Could be complex, requiring well-known or some other solution. \
Regarding site-site, doesn’t seem like a big problem. As long as it’s each frame has to request to opt-in, then an authentication site could just never call requestStorageAccess if they don’t want to be in that situation. \
Iframes losing access on navigation? I thought Safari preserves storage access through navigations. Maybe preserving the user permission, but the site would still need to call the API again.
* John: same-site navigation may preserve it, but cross-site navigations should clear it.
* Ben VanderSloot: generally supportive of these three security mitigations. requestStorageAccessFor as a separate method is a separate behavior; a separate API with separate developer ergonomics. A CORS requirement might mitigate the CSRF/confused deputy type attacks. A more concrete way for a third party to delegate to the first party in a consistent way would be nice. Per-frame access could have compatibility issues, removes some functionality that is already shipped; willing to experiment with per-frame access.
* Erik: a way for the embedded site to opt-in, is one option a requestStorageAccessFor pre-approval step, so that there isn’t a user prompt when the embedded site subsequently calls requestStorageAccess. If I embed accounts.google.com but don’t want them to have to show a UX and get a click, the top-level site can get a user engagement at an appropriate time and call requestStorageAccessPreapprovalFor. Top-level site and embedded site could communicate with postMessage to indicate that it’s a good time to ask.
* Ben V: not something we had considered, but is the kind of slight structure change.
* Johann: similar to the forward declare model. Main category is some helper, like a CDN or asset origin, which wouldn’t have user interaction. Well-known file, although a frequently-considered solution.

### Consider restricting storage access scope back to per-frame #122
* Johann: there are other advantages besides just security. Might be more stable as a long-term functionality.
* Erik: how would it work with Workers?
* Johann: have to decide whether it gets inherited by workers. Shouldn’t be a blocking problem. Integration with Permissions API – adjacent iframes may need to know when they can request and not prompt the user. Would be good to integrate with Permissions API.


### Consider requestStorageAccessFor Method #107

* Mreichhoff: more to come.

### Any other business

* W3C Workshop on Permissions, in December, hosted by Google in Munich. Want to get a diverse group of backgrounds, to talk about permissions in a very broad sense for the Web. Sign up and describe your work on permissions. Not formally organized by WebAppSec, but some overlap.

  Link: [https://www.w3.org/Privacy/permissions-ws-2022/](https://www.w3.org/Privacy/permissions-ws-2022/)

## Attendees
1. Erik Anderson (Microsoft Edge)
2. George Fletcher (Capital One)
3. Ben Kelly (Google)
4. Chris Needham (BBC)
5. Cameron Boozarjomehri (Mozilla)
6. Erik Taubeneck (Meta)
7. Andrew Aikens (TripleLift)
8. Dylan Cutler (Google Chrome)
9. Johann Hofmann (Google Chrome)
10. John Wilander (Apple WebKit)
11. Ben VanderSloot (Mozilla)
12. Nick Doty (CDT)
13. Mike O’Neill (Baycloud)
14. Vittorio Bertocci (Okta)
15. Stefan Popoveniuc (Google Chrome)
16. Lee Graber (Tableau/Salesforce)
17. Aram Zucker-Scharff (The Washington Post)
18. Gaurav Chhawchharia (Yahoo)
19. Alexandre Nderagakura (IAB Europe)
20. Sam Goto (Google)
21. Claude Vervoort (Cengage Group)
22. Sean Harrison (Google)
23. Brian May (dstillery)
24. Matt Reichhoff (Google)
25. Guillermo Movia (Article19)
26. Don Marti (CafeMedia)
27. Kelda Anderson (Microsoft Edge)
28. Allan Spiegel (Adobe)
29. Rachit Sharma (IAB Tech Lab)
30. Kale Smith (Roku)
31. Yi Gu (Google)
