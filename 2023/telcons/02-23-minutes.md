# 2023-02-23 Privacy CG Meeting

* Chair: Tess
* Scribe: Pete

## Agenda

* [Global Privacy Control](https://github.com/privacycg/gpc-spec/)
    * [Dates in the support resource #32](https://github.com/privacycg/gpc-spec/issues/32)
    * [Does this really need a GPC support resource #33](https://github.com/privacycg/gpc-spec/issues/33)
    * [Fingerprinting #36](https://github.com/privacycg/gpc-spec/issues/36)
* Storage Access API update
    * [SAA is per-frame again](https://github.com/privacycg/storage-access/pull/141)
    * [Use the Permissions API](https://github.com/privacycg/storage-access/pull/138) 
    * [Some description of how we integrate with cookies (pending cookie layering)](https://github.com/privacycg/storage-access/pull/145)
    * [Some P&S Considerations](https://github.com/privacycg/storage-access/pull/142) 
    * [Remaining work before we want to go for HTML](https://github.com/privacycg/storage-access/issues?q=is%3Aissue+is%3Aopen+label%3A%22resolve+before+graduation%22)
    * [Chrome's new (supportive) position on SAA](https://github.com/privacycg/storage-access/pull/165) 
    * [TAG Review completed satisfactory](https://github.com/w3ctag/design-reviews/issues/807)

## Attendees:

1. Cameron Boozarjomehri (Mozilla)
2. Nick Doty (CDT)
3. Theresa O’Connor (Apple, co-chair)	
4. Pete Snyder (Brave)
5. Chris Needham (BBC)
6. Ari Chivukula (Google)
7. Johann Hofmann (Google Chrome)
8. Paul Zuehlcke (Mozilla)
9. Tim Huang (Mozilla)
10. Justin Brookman (Consumer Reports)
11. Artur Janc (Google)
12. Christine Runnegar (PING co-chair)
13. Don Marti (CafeMedia)
14. Allan Spiegel (Adobe)
15. Sebastian Zimmeck (Wesleyan University)
16. Tom Van Goethem (Google)
17. Erik Taubeneck (Meta)
18. (The Washington Post)
19. Erik Anderson (Microsoft Edge)
20. Eric Mwobobia (ARTICLE19)

## Dates in the support resource #32

* Tess: this looks like a straightforward bug and we have a PR. 
* Justin: should be quick, Sebastian anything to say
* Sebastian: yes just a bug fix, we thought the .well-known would be the main
  discussion point
* Tess: Unless anyone wants to discuss #32 and #36 lets move on to #33

## Does this really need a GPC support resource #33 [https://github.com/privacycg/gpc-spec/issues/33](https://github.com/privacycg/gpc-spec/issues/33) 

* Tess: Martin raised this but hes not here. Who can summarize GPC side?
* Justin: Originally the .well-known resource was a way sites could optionally
  describe if/how they’re respecting GPC. Many sites are updating .well-known,
  but not all of them. Aram is here
* Aram: WN resource allows sites to say they’re using GPC where applicable.
  Optional signal browser UI/extension could surface it to show the signal
  being sent by the browser. Does not indicate what the site is doing it, just
  that the site is following some GPC-relevant policy. WN is not universal;
  some WN say false, some GPC respecting sites don’t include WN. WN seems
  useful to regulators and privacy advocates to understand how sites interact
  with GPC. Last updated field in WN indicates that / if the site has updated
  its GPC compliance recently (e.g., in response to new legislation /
  obligations). Some folks have said WN isn’t mandatory, so we ought to remove
  it. Some disagreement between whether its a static signal describing a site,
  vs describing something else (ed: session). The editors would appreciate
  feedback from possible adopters and implementors
* Justin: Pete S submitted a PR to suggest that the WN signal describes the
  session and not the site
  [https://github.com/privacycg/gpc-spec/pull/48](https://github.com/privacycg/gpc-spec/pull/48)
* Pete: main question is whether there is an optional signal. But yes,
  currently ambiguous whether it’s a site-wide policy or what they’re doing for
  the current user. I would prefer a signal for the current user.
* Cameron: I agree with MT, should not be a mandatory signal. Concern about
  adoption, and whether sites will be deterred from supporting GPC if it
  requires the WN
* Nick: I can’t speak to implementer concerns, but does the lastUpdate field
  make it easier to screwup / deter implementors. Since we’re not describing
  what compliance means, is this useful? Would dropping the lastUpdate field
  make compliance / implementation easier
* Sebastian: Original thinking on LU was added incase new meaningins were
  attached to the GPC signal (ed: new policy/laws/etc), the site could use this
  to indicate the site was aware of the new meaning. Re adoption of WN, there
  are a number of sites who have adopted. For example, weather.com, lego.com,
  etc. Seems good! IMO this only means that the site respects GPC, nothing
  more. I think this is good bc the user gets feedback, and it promotes GPC.
* Aram: re implementors: i agree WN should not be required bc it can be
  difficult for some implementors. Most publishers are small2med on someone
  else's platform using a CMP to deal with compliance; these pubs dont have
  easy access for adding a WN. This is what Im against requiring it, and why a
  static response is needed. Its useful for understanding adoption, and helps
  users understand whats going on around privacy. Re LU: versioning is bad. WN
  would be updated when a site changes how it processes GPC. Re Bradon:
  browsers should surface it so users can make informed browsing decisions.
  This could only be done with the WN signal
* Cameron: Please don't make this mandatory. Re signaling, it might be wrong
  and not representing the sites current behavior. Browsers trust sites to do
  what the signal says. If the site sends a false signal that it is honoring
  GPC for users when it might not be then that mismatch would lead users to
  trust a false signal.
* Justin: Re adoption: fwiw, was 6k at the beginning of the year. Now it’s 21k.
  CA and other jurisdictions require enforcements. That might make
  session-specific signal useful. Otherwise, might be useful to have a less
  binary response, either dynamic or indicating jurisdictions.
* Aram: Robin previously mentioned that it might be worth removing the WN from
  this spec, and move it to some broader privacy-signals spec that could
  describe privacy policies in addition to GPC
* Cameron: decoupling sounds good, to reduce hurdles to GPC adoption
* Sebastian: three points:
    1. GPC sig doesn't need to be more detailed. Signal is site saying i
       respect whatever the law is. WN doesn’t give any meaning. No need to
       differentiate further
    2. I've worked with 10-15 sites to help them implement GPC and the WN
       resource. It seemed easy. If its difficult id like to understand more
    3. Re decoupling: GPC isn’t required, so why decouple
* Justin: decoupling seems like boiling the ocean. GPC is defined and straight
  forward. I don't think WN will affect adoption because its already required
  in some jurisdictions
* Aram: are there folks who require decoupling to advance this? If optional WN
  is a blocker, it seems not worth it. If optional WN isn’t a blocker though,
  it seems useful (if non technical) for activists and journalists, and for
  extensions to surface to users. For some users GPC will change people’s
  decisions, and some people care about it very strongly (based on my
  experience at the Washington Post). Maybe WN isn't the only way to do signal
  that to users. If its a blocker lets remove it
* Tess: re measurement of adoption: If WN is optional, its not an accurate
  measurement, no? And if there are more jurisdictions that require it, that
  seems like it makes the signal less useful. Maybe dropping it is the right
  move
* Aram: Measurement isn’t trying to measure everyone who’s using the signal;
  its still interesting and useful to measure directionality of whats going on
  outside the standards space (e.g., compliance). That could happen w/o WN, but
  WN makes it standardized. Bradon’s scans has been useful to regulators
* Cameron: many people here want GPC to be successful, this seems like a
  distraction from the core thing we want. I personally like WN, and think
  sites should be able to signal. But shouldn’t be required, because it would
  complicate adoption and still not be an accurate measure of the ecosystem.
  TL;DR: Simple is better
* Sebastian: re Tess’s measurement point: for the reasons you said it should be
  mandatory, so we can measure better. What are the concerns implementors face,
  it’d be helpful
* Aram: difficulty is mostly relevant to small to medium pubs who don’t have
  control of the .WN path/dir. In time tools could make that easier (WP
  plugins, etc) but some pubs have 1 or &lt;1 engineer, and rely on CMPs to
  handle. This is mostly bc of the weird history of handling consent management
  on the web, which has been delegated to 3ps, who generally don't have access
  to .WN paths. So pubs need contractors or limited engineer time. They’ll do
  it if they have to, but compliance is difficult and costly. It'd be cool if
  WN was universal, but its a lot to ask. Advancing GPC w/o WN, is better than
  paused GPC w/ WN
* Justin: Tess’s point is helpful, we should assume its adopted where
  mandatory, however, its useful to measure adoption outside of jurisdictions
  where its mandatory (e.g., Virginia). So adding more data to WN, or making ti
  clear that WN describes policy to the requester, and not all requesters,
  seems maybe useful.
* Aram: Two questions as an editor:
    1. Is decoupling GPC support signal from GPC spec (and into some other
       compliance signal spec) useful for adopting / advancing GPC
    2. Would the group take up a separate doc
* Tess: re taking up another spec, policy would be the same as existing policy;
  open a proposal and see what the group says. Re is this a blocker, hard to
  answer. Nothing in PrivacyCG or W3C process that makes it a blocker. However,
  if folks feel strongly about it, it’ll make advancing things more difficult.
  Would be good to have MT’s thoughts. But re process, what people (PrivacyCG
  members) look for are are you following process, considering input and doing
  the work. Best possible answers!
* Erik: sounds good
* Cameron: re MT, he seems to think WN shouldn’t be a blocker, and if it
  becomes a blocker we should spin it off
* Aram: if no one is saying WN is a blocker, whats the next step to give folks
  a last (next?) chance to declare if its a blocker before advancing
* Tess: editors choice for when to close the issue. You could also choose to
  notify folks the issue is going to closed in, say, 7 days unless someone
  objects. However, the group is big and there are many timezones, so best to
  give folks 48 hours or so, and best to be mindful of weeks. Seems to me
  you’ve done enough due diligence here though.
* Aram: thanks, we dont need to close it now, but sounds like a good plan.
  We’ll probably close it next week
* Sebastian: please feel free to add discussion to the issue

## Storage Access API update

* Johann: i was going to give an update on Storage Access API (SAA). A lot of
  work has been done. Chrome started engaging on SAA at TPAC. We also added a
  security considerations sec. The big change we made was to move SAA back to
  per-frame model, and to remove “auto propagation” model. We’ve also aligned
  SAA with existing permission model. We’ve also updated the spec to deal with
  cookies, though its still handwavey. Lots of other small text changes and
  added Web Platform tests. Other implementors will need to change too. Chrome
  submitted a position on SAA, and supports it. We prev said only FPS, but now
  we’re looking into prompts. Finally, TAG reviewed SAA and seemed happy with
  it.
* Nick: has anyone tested if users understand what the permission / prompt
  means. Im not sure i understand it and i expect many users wont either
* Johann: good Q. We should have more conversations about how to talk to users
  about privacy, also around FedCM. Chrome is (might?) do user studies. Some
  browsers have already done SAA w/ prompt, and there might be a need for the
  prompt at least in the short term. Longer discussion to have.
