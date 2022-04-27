# 10 February 2022 Privacy CG Teleconference

* Chair: Tess
* Scribes: Sam & Nick

# Agenda

* [Storage Partitioning](https://github.com/privacycg/storage-partitioning)
    * Status update
* [Login Status API](https://github.com/privacycg/meetings/blob/main/2022/telecons)
    * [Updating OS-integrated surfaces](https://github.com/privacycg/is-logged-in/issues/51)
* [First-Party Sets](https://github.com/privacycg/first-party-sets)
    * [CNAME to third-party domain in FPS](https://github.com/privacycg/first-party-sets/issues/79)
* Any other business
    * [PATCG February 2022 Virtual Meeting](https://github.com/patcg/meetings/tree/main/2022/02/09-telecon)

# Attendees:

1. Alex Cone (IAB Tech Lab)
2. Allan Spiegel (Adobe)
3. Anne van Kesteren (Mozilla)
4. Angela Ballard (CJ Affiliate)
5. Aram Zucker-Scharff (The Washington Post)
6. Ben VanderSloot (Mozilla)
7. Bill Densmore (ITEGA.org)
8. Brian Lefler (Google Chrome)
9. Brian May (dstillery)
10. Brianna Goldstein (Google)
11. cfredric
12. Chris Needham (BBC)
13. Christine Runnegar (PING co-chair)
14. Christy Harris
15. Dan McArdle (Google Chrome)
16. Daniel LaLiberte
17. David Dabbs (Epsilon)
18. Don Marti (CafeMedia)
19. Dongoh Park
20. Emily Lauber
21. Eric Mwobobia (ARTICLE 19)
22. Erik Anderson (Microsoft Edge, co-chair)
23. George Fletcher
24. Harneet Sidhana (Microsoft Edge)
25. Jaime Perez
26. James Rosewell (51Degrees)
27. Jeffrey Yasskin (Google)
28. Joel Odom (Salesforce)
29. Johann Hofmann (Google)
30. John Wilander (Apple WebKit)
31. Joshua Ssengonzi
32. Kaustubha Govind (Google Chrome)
33. Kris Chapman (Salesforce)
34. Lola Odelola (Samsung Internet)
35. Marijn Kruisselbrink (Google)
36. Michael Kleber (Google Chrome)
37. Nick Doty (Center for Democracy & Technology)
38. Paul Bannister (CafeMedia)
39. Paul Zühlcke (Mozilla)
40. Ralph Brown (Brown Wolf Consulting LLC)
41. Russell Stringham (Adobe)
42. Sam Weiler (W3C/MIT)
43. Sam Goto (Google)
44. Sean Harrison (Google)
45. Steven Valdez (Google)
46. Tanvi Vyas (Mozilla, co-chair)
47. Tara Whalen (Cloudflare)
48. Theodore Olsauskas-Warren (Google)
49. Theresa O’Connor (Apple, co-chair)
50. Tim Huang (Mozilla)
51. Wendell Baker (Yahoo)
52. Wendy Seltzer

## [Storage Partitioning](https://github.com/privacycg/storage-partitioning) -

Presentation link - [https://docs.google.com/presentation/d/1i7KvTtIS2JhAadQsdWLFpMzNmgXmUbXSfPuO_wYX6d8/edit#slide=id.p](https://docs.google.com/presentation/d/1i7KvTtIS2JhAadQsdWLFpMzNmgXmUbXSfPuO_wYX6d8/edit#slide=id.p)

Anne: We call it “state” since it encompasses more than storage.  Goal: make all authority derive from the address bar - from what the user sees in the addr bar.  Example: two tabs that look disjoint to the user but have a channel between them.  After: no more back channel.  Encompasses many things ; long list in doc.  Want to talk re: networking, cookies, storage, and misc.  I summarized Safari, Chrome, Firefox; I’d love more details from others.

Networking mostly integrated into Fetch std.  Some work arounds to be finished.  There might be a long tail of work.  HTTP cache is partitioned, connections partitioned.  FF and Safari have shipped.  CHrome shipped HTTP cache and I saw an intent to ship for the rest.

Erik: edge also partitioning cache.  Active thread re: sockets.  Lots of discussion re: partitioning keys.  Safari and FF are just top-level, right?

Anne: yes.

Erik: we’ll likely do something similar, but not sure on timeline

Nick: example of addl. Keys?

Anne: multiple layers of nested browsing contexts.  Chrome has experimented with top-level site, but they also use origin/site of nested doc.  And an add’l bit if nested doc has any cross-origin ancestors …. Or maybe an add’l bit…. Storage and networking bits not yet aligned.

Nick: I’d love to see docs.

Erik: see link in Slack.  [https://developers.google.com/web/updates/2020/10/http-cache-partitioning](https://developers.google.com/web/updates/2020/10/http-cache-partitioning)

Anne: Cookies not standardized yet.  Not sure where.  Maybe in Fetch.  Safari blocks cookies; storage access brings them back.  It doesn’t give IETF spec sufficient context.  Not sure where to address.

Kaustubha:  we heard from John that they’d be open to an opt-in model.  Would FF be interested in that also?  John was suggesting Storage Access API with a param.  We prefer an attribute.  CHIPS relies on a new attribute.  We agree on the rough shape; this is a deployment and syntactical difference.

Anne: discussing where to standardize.  Either partition or block is acceptable.  FF plans to partition cookies with Storage Access to bring them back.  Chrome plans to block and have CHIPS as opt-in.

Tess: Edge, too.

Erik: CHIPS makes a lot of sense.  Storage partitioning, too, but need to think about interaction.  Likely to support both for a while to sort interop.

John: Kaustubha is correct.  Open to opt-in partitioned cookies.  Would prefer Storage Access, open to CHIPS.

Tanvi: we’re not all doing the same thing, but open to talking about Cookie behavior.  Need to sort Storage Access; maybe an ad-hoc focused on handling of Third Party Cookies.

Anne: maybe a followup mtg for cookies.

Tess: I’ll follow up and try to get Brave people.

Anne: storage foundation defined in the Storage WHATWG std.  Work to be done, but it’s there. Some downstream specs updated, e.g. indexdb.  Plan is to update keying to include not just origin but also top-level site.  Will update other specs to use this abstraction.  Elegant.  Work had stalled but picking up again.  Fixing broadcast channel.  Looks good.  Safari has partitioned.  FF plans to partition w/ storage access ; plans to move to always partitioned.  Chrome plans to partition.  Aligned except for add’l keying.  E.g. bit re: cross-origin ancestors.  E.g. A1 nesting B nesting A2, should A1 and A2 share storage.  Might be better if they do not.  Some compat concerns.

Erik: Edge not doing anything different.

Anne: haven’t shipped?

Erik: not partitioning.

Anne: assorted state e.g. blob URL store - have a plan, but haven’t experimented.  Chrome has metrics, but not ready to ship

Marijn: not sure what we (Chrome) will do.

John: haven’t looked at blob URL store - we are aware of it.

Anne: window.name largely solved.  I think all have shipped it.  CHrome had the most issues

Kaustubha: we were working on that, not shipped.  Some usage metrics were high for comfort of blink api owners.  They wanted a slow roll-out.

Anne: somewhat disappointing.  Permissions mostly solved in most implementations, by Permissions Policy.  Any API requiring a permission can’t get permission in a cross-origin context.  Top level can delegate ability to request a permission.  If delegee already had permission, it bubbles up.  A is blamed for actions of frames it decides to embed.  Authority derives from addr bar.

Sam: would a top-level embed an origin that they think already has permission. Would that permission bubble up to the top-level?

Anne: no (opposite). If A embeds B, then permission is requested for A and can delegate the ability for B to ask for permission. If A already has permission, then B just gets it with no prompt.

John: I don’t know state of permissions work.  I’ll find out.

Kaustubha: with permissions, not deterministic… permissions sounds different from other things because non-deterministic.

Anne: it’s a bit you can read.

Kaustubha: we’ve talked about ability to form an identifier…

Anne: Not high-bandwidth, but it is a problem.  But permissions store is a cache of sorts that should not be shared.  No one has a good soln for pop-ups.  They require a click, but they break the partitioning efforts.  Potentially we have a connection to the partitioned child.  Tricky because you can’t just partition, e.g. tabs following a google search.  Would be bad to partition them all.

Kaustubha: what’s the communication channel with popups?

Anne: postMessage or direct script access.

Kaustubha: should we partition browsing context groups? Large compat implications, but that could be one way to address it.

Anne: if federated login is generally figured out, pop-ups wouldn’t be needed for that.

Kaustubha: another login flow for vanilla OAuth logins [?]. Opt-in partitioned pop-up that wouldn’t be affected in the future by state partitioning. For everyone else, warning of potential deprecation in the future of opener relationship.

tess/anne: worth trying.

Aram: web pages embed things that they don’t know if they’re going to ask for permission. Should be able to restrict embedded sources from asking for a permission or triggering a popup.

Tess: Permission Policy already has this, yes.

Anne: nested contexts don’t get to prompt, unless the top-level delegates.

Anne: if you’re working on a new feature that might allow communication, let Anne know.

Sam: should we be asking all feature authors to document in security/privacy considerations?

Tess: we should ask that in the S&P Questionnaire, will check.

## [Login Status API](https://github.com/privacycg/meetings/blob/main/2022/telecons) (isLoggedIn)

### [Updating OS-integrated surfaces](https://github.com/privacycg/is-logged-in/issues/51)

John Wilander: issue 51. Taking the idea further for PWA and push notifications/service worker. Want to stop sending push notifications to a client if the user isn’t logged in to that webapp any longer. Yes, it could be a way to make the OS aware if the user is still logged in or still needs that functionality.

Tess: what about sites where you sign up for notifications but don’t have an account? Like a shopping site where you say, notify me when this product is available. Rather than email address (which could be used for other kinds of tracking). You still might want push notifications even when you’re not logged in.

John: more about when you might get sensitive information, like direct messages for your account.

## [First-Party Sets](https://github.com/privacycg/first-party-sets)

### [CNAME to third-party domain in FPS](https://github.com/privacycg/first-party-sets/issues/79)

Russell Stringham: sites use a third-party expert in some particular functionality, like Adobe analytics. Customers create a CNAME in order to look like a part of the first-party. With some privacy plugins or browser cookie settings, cookies may be limited. Would it be possible to declare inside a first-party set that this CNAME sent to a third-party is controlled and owned by the first-party and is not being used for cross-site tracking, so could get the same permissions as if part of the first-party site. Alternative is creating an internal proxy by the first-party which would get the same functionality, but has perf implications. Generally want a way to indicate (through FPS or otherwise) that a site is generally part of a first-party.

Kaustubha: CNAME already a partition (in how cookies are limited). But for browsers that have blocklists, CNAME could be a way of working around the blocklist. Safari’s blocking/detection of CNAME seemed more concerned about security threats than privacy threats. Chrome doesn’t have any plan to intervene on cnames. Want to understand from other browsers what the privacy threat is.

Sam: want an exception to treatment of cnames, via any mechanism? Russell: yes. Sam: given that someone could publish A records, so it seems like a weak protection anyway. But how you can technically distinguish between sites that are using this for tracking or for non-tracking?

Russell: doesn’t really give you the ability to do cross-site tracking with cookies. Don’t have any way of linking the data from the same user. Maybe we could make a declaration about who the data controller is.

James Rosewell: really about data controller - data processor relationship, make sure there’s no interference with a b2b relationship. PR needs to be fixed on formatting, but will do so. Per Sam, how does the browser verify that it’s really a data controller relationship? My work is in [swan.community](https://swan.community/) which explains one method of providing the data controller / processor relationships and privacy policies to web browsers. (Also note - [Communicating people’s choices to all parties · Issue #13 · privacycg/nav-tracking-mitigations (github.com)](https://github.com/privacycg/nav-tracking-mitigations/issues/13)

John: security implications of cname cloaking. Privacy concern is script-writeable storage of tracking scripts running in the first-party context for long periods of time. Lots of vendors moved to cname cloaking to get longer term cookies, so Safari provided the same limitations on cname cloaked third parties.

Kaustubha: a third party tracker could just use partitioned storage?

John: in webkit, partitioned storage is always ephemeral.

Kaustubha: so your model is that no third party should have non-ephemeral storage? Even if the first party site declares that it’s a trusted party?

John: long-term goal is to distinguish true first parties, the site owner should have the capability to do it. But the fact that you dropped a third-party script into the first-party context years ago, and that script can change over time – in that case we’re not sure that’s really what the first party wants. That’s the goal, but open to the means.

Russell: a first-party set might be one way to declare that I really want this.

John: any change to storage should be in the first party’s control – don’t want changes to happen without the first party’s understanding.

Brian May: in general, cnames being used in this way seems like bending the rules. As a developer, I’m assuming I’m still in the first party context, seems like we’re not being up front with people.

Aram Z-S: +1 to Brian and John. if our goal is to move authority to the address bar, use of cnames makes that less clear. From a publisher perspective, we see this cname cloaking as a security risk. Once you give this privilege, then these operators could do all sorts of other things on that space, and it’s hard to observe and control.

## Any other business

### [PATCG February 2022 Virtual Meeting](https://github.com/patcg/meetings/tree/main/2022/02/09-telecon)

Aram: everyone is invited! Working on next steps, documents we’re putting together, and review of proposals in the space of attribution and measurement. Had some presentations and conversation, with more continuing this evening. Anyone interested in the intersection of privacy and ad technology.
