# 2023-04-27 Privacy CG Meeting
* Chair: Erik
* Scribe: Cameron

# Agenda
* [Login Status API](https://github.com/privacycg/is-logged-in)
    * [Should FedCM use the Login Status API? #53](https://github.com/privacycg/is-logged-in/issues/53)
* [Global Privacy Control (GPC)](https://github.com/privacycg/gpc-spec)
    * [Clarify .well-known response describes the sites compliance for the session #48](https://github.com/privacycg/gpc-spec/pull/48)
    * [Ensure consistency between HTTP and JavaScript #49](https://github.com/privacycg/gpc-spec/issues/49)
* Any other business
    * (Johann) if time: [How Google Chrome is implementing the Storage Access API](https://github.com/cfredric/chrome-storage-access-api) 

## Notes

## [Should FedCM use the Login Status API? #53](https://github.com/privacycg/is-logged-in/issues/53)
* Sam Goto: When Login status started a year ago, good amount of overlap with FedCM. Now FedCM has a component that looks very similar to login status API. Fx has highlighted this similarity in design and goal. The question is: “Is now the right time to merge these APIs or not?” Chrome has implemented IDP Sign In Status API as a prototype, and about to launch IDP sign in status api. Primarily to be used by federation and federated identity. FedCM is more narrow, loginstatus is more broad but the principles are the same.
* Cameron: should the Login Status API be handed off to FedID CG? Maybe it would be better to consolidate over there/hand over to them.
* Sam: another interesting question is where FedCM would use the Login Status API. As the Login Status API is spec’ed is it’s underspecified– it describes the bit but not how it is used. Can’t decouple the API from the abuse threat model. Cover how to prevent it from being abused for either purpose. But wanted to ask the question and see if they should be consolidated. Johann also suggested may be other cases for login status where it could be used for circumstances to support FedCM or Storage access API
* Johann: Some context, originally this came from a storage access discussion to present problem in how to only prompt logged in users. We have a known breakage case where spotify wants to show full songs in embedded iframes to logged in users. Could be done by storage access flow but only want to do it for logged in users. There’s a mode that says “only prompt if logged in” but don’t share that logged in, and this seems solid off hand.The whole proposal has expanded to cover a variety of extra cases that come from setting this bit, so wouldn’t want to tie other behaviors to it. Want to limit scope for sec and privacy. My gut feeling is we can probably keep login status in privacycg if more than FedCM
* Nick Doty: Some advantages for user clarity that there’s only 1 login status api to make sure 2 api’s don’t get out of state. Either way should consolidate
* Aram: I agree with Nick, want to converge these to 1 spec. They look close enough that directionally they seem like a good idea. These specs should converge. I think best place to do this is outside of FedCM since number of use cases outside of FedCM. The expansion of it through FedCM could broaden focus. Would like to hear from John Wilander.
* Brian: I agree, shouldn’t move but must converge.


## [Clarify .well-known response describes the sites compliance for the session #48](https://github.com/privacycg/gpc-spec/pull/48)
* Aram: Both editors and privacy cg have gone back and forth on its use in the spec, and its req or non-req/phrasing and whether its response should represent a session user, etc. Would change whether it responds to the user or not. The .well-known would respond with the gpc status for the publisher based on geography. Would clarify true or false as whether the site intends to exercise the maximum opt-out the site would offer that the user opts in to. Lawyers like the idea that sites can tell they have awareness of GPC and advocates like the idea that it is nice to be able to scan for what sites are honoring it.
* Cameron: Mozilla’s opinion is it should not be tied to the spec. .well-known is its own can of worms. The question of validity and ongoing reliance on the signal is not as important as ensuring the GPC signal is built into the site. Not necessarily saying we don’t like the .well-known to determine if a site is honoring GPC based on its own signal, but doesn’t make sense to be tied to the spec.
* Aram: is there a version of .well-known that Mozilla would want to fit into the spec in your view.
* Cameron: no, our position is we want to make sure the spec is as deliverable as possible. Adding .well-known just muddies the conversation. We understand the importance/don’t want to downplay it, but once it’s in there people will nitpick it and will affect the GPC conversation.
* Nick: is the idea that we could have a separate spec or guidance for GPC .well-known. What’s most relevant to me is current implementation experience. They have implemented .well-known to be more static, and that should be a motivating factor.
* Cameron: from prior discussion, to your point– it’s not static. A lot of situations where it’s communicating something but giving a false sense of how much opt out is happening for a variety of reasons. Hard to go to a site that says I’m opted out when I’m at my house, but then I’m at the coffee shop for whatever reason it’s not that anymore– could be a lot of reasons VPN or otherwise– all I have is a binary signal. And making this more dynamic feels like a barrier to adoption. If we can make .well-known a nice to have/standardize in some other way, but saying to do GPC you must do .well-known and the .well-known needs to be implemented this way/flagged this way feel cumbersome.
* Aram: I think that opens 2 questions to me, where it is optional
* Cameron: I’d like Martin to be part of the conversation, so some of this is with a grain of salt. Want to make sure as long as it’s not a prerequisite to do GPC, then optional makes sense.
* Aram: this was introduced to show that the option implementation for well-known isn’t a good fit for a spec
* Jeffrey: This specifically does not look like a good change because “session” is not defined, and also it’s in use and the lawyers of those sites approved it for one reason and it’s not fair to pivot on that
* Cameron: +1 to Jeffrey
* Chris: what is the expectation for something under .well-known. Itg makes it problematic to be per user/session. .well-known has a specific meaning. We would treat that as static per site info, and something else would be in a different URL namespace.
* Aram: Describes .well-known, quoting https://datatracker.ietf.org/doc/html/rfc8615 - _Rather, they are designed to facilitate    discovery of information on a site when it isn't practical to use  other mechanisms; for example, when discovering policy that needs to be evaluated before a resource is accessed, or when using multiple round-trips is judged detrimental to performance._
* Chris: I would suggest per user/session info should not be in a response to .well-known
* George Fletcher: WebFinger is a dynamic way to give an email and get back an open-id provider on a .well-known resource. Didn’t get much adoption and took a ton of effort for implementation. Would need to do a lot of router tricks. Would be better to say we do support this function and here is a different endpoint to check. I say .well-known should be static.
* Aram - I second the implementation difficulty. This is a problem for a number of sites, and rely on 3rd party hosting systems and manage consent that run on page but have no access to server config. Dynamic response is very difficult. Might be better to describe a route for dynamic response. Is there a need for a broader spec that gives users info about consent status.
* Cameron: on FedCM side for research and education groups, not just institutions, but proxies behind proxies in institutions– when you’re 4 layers deep and no control over the front endpoint, no way to consistently give this info back dynamically. I, Cameron (not Mozilla), like the idea of giving a sense of if it’s honored. But worried about simplicity of the signal, just if it’s honored, not how it’s honored. A user getting different signals doesn’t help them understand it’s honored here but not the next time. 
* Aram: The static and dynamic imply they are in a private state when they are not?
* Cameron: yes don’t want to be misleading.
* Ashkan: CPPA regs regarding ‘status’ [https://cppa.ca.gov/regulations/pdf/cppa_regs.pdf#page=37](https://cppa.ca.gov/regulations/pdf/cppa_regs.pdf#page=37)
* Aram: I am hesitant to close this out without some directionality. It seems the CG is not in favor of this behavior and have a consensus to close this PR without merging (abandon) from the point of the community group.
* Ashkan: I concur with the difficulty of low level dynamic .well-knowns and recognize it is difficult without doing webserver/router tricks it can be hard to do. Likewise showing the users opt-out status is covered in CPPA, and say you original opted-in to this thing are you sure you still want to globally opt-out. The concept is important but not clear it needs to be in the .well-known.
* Aram: I’ll note, we have heard from lawyers that they are more comfortable with adoption when they can say they adopted it in someway with a timestamp. But sounds like we are abandoning this PR.


## [Ensure consistency between HTTP and JavaScript #49](https://github.com/privacycg/gpc-spec/issues/49)
* @ari can discuss: Was looking at GPC from browser implementation and wanted to know, what happens if user changes pref after browser loading. Currently 2 answers in spec, for JS the signal is frozen from the moment the resource load occurs, but for HTTP will send current pref with no caching. So could be cases where a long lived tab gets 1 thing in http but another in JS which loaded in a different context. My intuition is to align HTTP with JS to both use resource cached on site load. 
* Nick: Not so worried about mid load, but post load is valid. Basing it on navigation seems simplest. So if the user changes pref user may need to reload page and let site make decisions when loading resources and we can only notify that on page load (don’t want to make it dynamic mid flight)
* Aram: I understand interest in reflecting consistency. Generally the way this world in practice is, you get 2 signals, you take the more conservative “opt-out” path. That’s how this works in practice when the two values are unaligned.
* Chris: Both should be returning same info, and users current info, and if we optimize for implementation then we need to put users preference first (if we got 2 ways of getting the same info they should reflect the same state as it changes)
* Cameron: are you echoing Aram’s point of taking more conservative path?
* Chris: no, if two ways to get the info they should both reflect it as it changes. Even if it had changed since the original page load.
* Cameron: my point was echoing what Nick said– they should reflect each other and “what is the last site load?” If signal comes from HTTP header that will be the source for the rest. If you load a page that reflects an opt-out, that should carry through the rest of the page. JS and HTTP should be the same.
* Jeffrey: wanted to mention comparison with permissions. In the past chrome sets permissions on page load, and if there is a change then prompted to “re-load” to honor the changer
* Aram: are you saying the message is necessary?
* Jeffrey: the message isn’t necessary but no change would take effect til page reloads
* Ari: agree on that alignment, for implementation I think they are equivalent and I think prompting to reload on a change in CPG pref also makes sense (or the browser could auto reload those pages). But it needs to be top down, don’t want the page to start as GPC opt-in then have later subframes be opt-out (or vice versa) unless the whole page reloads
* Chris: +1 on prompting user to show page needs to reload to respect wishes.
* Aram: We generally attempt to avoid unlimited scope setIntervals, so I think what was just stated by chris would be close to how it would work.
* Ari: I think that clears the path to add the notice around the reload makes sense for the discussion.
* Aram: +1, this group seems to be at consensus. Can you please update the PR to note that?
* Ari: yes will prompt user on resource changes.


## Attendees
1. Erik Anderson (Microsoft Edge)
2. Tom Van Goethem (Google Chrome)
3. Aram Zucker-Scharff (The Washington Post)
4. Sam Weiler (W3C)
5. Nicolas Pena Moreno (Google Chrome)
6. Chris Needham (BBC)
7. Nick Doty (CDT)
8. Paul Zuehlcke (Mozilla)
9. Tim Huang (Mozilla)
10. Cameron Boozarjomehri (Mozilla)
11. Brian May (Dstillery)
12. Yi Gu (Chrome)
13. Michael Kleber (Google Chrome)
14. Chris Fredrickson (Google Chrome)
15. Johann Hofmann (Google Chrome)
16. Darryl Green Jr (The Washington Post)
17. Benjamin Case (Meta)
18. Ari Chivukula (Google Chrome)
19. ChangSeok Oh (ByteDance)
20. Jen Schutte (P&G)
21. Sam Goto (Google Chrome)
22. Russell Stringham (Adobe)
23. Ashkan Soltani (CPPA)
24. Alexandre Nderagakura
25. Kaustubha Govind (Google Chrome)
26. Jeffrey Yasskin (Google Chrome)
