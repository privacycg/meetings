# Privacy CG telcon - 12 March 2020

* Chair: Tess
* Scribe: Pete

## Attendees (sign yourself in)

* Erik Anderson (co-chair, Microsoft)
* Tanvi Vyas (co-chair, Mozilla)
* Theresa O'Connor (co-chair, Apple)
* Zach Edwards - GitHub: thezedwards
* Martin Meyer (Akamai)
* Pete Snyder (Brave)
* John Wilander (Apple)
* Mark Xue (Apple) - Github: mmx1
* Nick Doty (UC Berkeley) - Github: npdoty
* Pranjal Jumde (Brave)
* Kristen Chapman (Salesforce) - Github: kdeqc
* Christy Harris (Future of Privacy Forum) - Github: ItsChristy
* Sam Weiler (W3C/MIT)
* Jacek Oleksy (Opera) - Github: pr0t4zy
* Steven Englehardt (Mozilla)
* Christine Runnegar (PING co-chair)
* James Hartig (Admiral)
* Kushal Dave (Scroll)
* Michael Ten-Pow (Amazon) https://github.com/mtenpow
* Brandon Maslen (Microsoft)
* Aram Zucker-Scharff (The Washington Post) - GitHub Username - AramZS
* David Senecal (Akamai) - GitHub: dsenecal04
* Ketan Deshpande (Amazon) - Github: deshpa
* Arthur Edelstein (Mozilla)
* Peter Saint-Andre (Mozilla) - GitHub: stpeter
* Diptendu Bhowmick (Amazon) - GitHub: Diptendu
* Daniel Appelquist (Samsung) Github: Torgo
* Anuvrat Singh (Amazon) GitHub: anuvrat
* Thomas Bergemann (Axel Springer) Github: tmountainman
* Carlos Bracho (Axel Springer) Github: adtechnology
* Johann Hofmann (Mozilla) - GitHub: johannhof
* Wendy Seltzer (W3C)
* Wendell Baker (Verizon Media) https://github.com/wcbaker
* Jack Frankland
* Alan Stearns (Adobe)
* Chris Needham (BBC) GitHub: chrisn

## Agenda

* The [privacycg organization](https://github.com/privacycg) on GitHub
    * [membership](https://github.com/orgs/privacycg/people) is open to all CG [participants](https://www.w3.org/community/privacycg/participants)
    * its [teams](https://github.com/orgs/privacycg/teams) ([all](https://github.com/orgs/privacycg/teams/all), [contributors](https://github.com/orgs/privacycg/teams/contributors), [editors](https://github.com/orgs/privacycg/teams/editors), [chairs](https://github.com/orgs/privacycg/teams/chairs), [w3t](https://github.com/orgs/privacycg/teams/w3t))
* [Storage Access API](https://github.com/privacycg/storage-access)
    * [Active or passive storage access after explicit user opt-in](https://github.com/privacycg/storage-access/issues/2)
    * [Per-Frame or Per-Page Storage Access](https://github.com/privacycg/storage-access/issues/3)
    * [Age Out Of Granted Storage Access](https://github.com/privacycg/storage-access/issues/5)
* Proposals
    * [Full storage partitioning / double-keying](https://github.com/privacycg/proposals/issues/4)
* Meetings
    * [Request for meeting for JS Membranes proposal](https://github.com/privacycg/meetings/issues/2)
    * New zoom meeting info
* [Spring 2020 Virtual F2F](https://github.com/privacycg/meetings/issues/3)

## Introductions

(many friendly new people attending and introduced themselves)

## The [privacycg organization](https://github.com/privacycg) on GitHub

Tess: The PrivacyCG Github or keeps track of agenda issues, proposals, etc, like other standards groups.
…: PrivacyCG admins need to seperatly add people to the PrivacyCG GH org
…: more than 100 people from over 50 orgs (!)
…: If you are not currently a member of the GH org, please let Tess know your GH account, and Tess will invite you

Aram: Can we add our GH account to the minutes here?

Tess: Yes, great idea!

## [Storage Access API](https://github.com/privacycg/storage-access)

### [Active or passive storage access after explicit user opt-in](https://github.com/privacycg/storage-access/issues/2)

JohnW: Long disucssion on GH issue.  This is part of an ongoing disucssion of the scope of Storage Access API (SA API).
…: SA API prompts the user for access under a 1p eTLD+1.  But what should the scope of that access be?
…: If frame-lifetime access, no need to have explicit lifetime.  But if its page length, then there needs to be a lifetime
…: (e.g. one day, month, etc)

### [Per-Frame or Per-Page Storage Access](https://github.com/privacycg/storage-access/issues/3)

JohnW: This issue discusses whether SA API should be per-frame or per-page.
…: Current convo is between implementors (Gecko, WebKit, MS Chromium)
…: WebKit, Moz and MS folks are talking with site owners about the kinds of 3p APIs they need to support.
…: Current apps assume 3p storage, which SA API breaks.
…: Also looking to understand auth widget uses.
…: But open question is if there are _not legacy_ uses that need elevated storage access.
…: Difficult bc only the frame that calls the API knows that it has access.
…: Other page frames don't know if the frame has access.
…: Eager to have people share their use cases for SA API / 3p frame storage.

### [Age Out Of Granted Storage Access](https://github.com/privacycg/storage-access/issues/5)

JohnW: Related, whats the lifetime of elevated storage access?

Pete: Any convo about SA API lifetimes being aligned with permission lifetimes?

JohnW: Conversation goes back as long as 2016 (re: permissions and SA API).

Pete: Might be easier for users to understand if it also had lifetime of other permissions.

JohnW: Current behavior is to re-up the lifetime of a SA API on renegagement, but this isn't standardized anywhere.
…: Will add this disucssion to issue.

Steve: Moz prefers per-frame lifetime.  Makes it easier to implement since it doesn't require cross frame syncing of storage privilage changes.
…: eg, federated login would be difficult / broken as is if lifetime is per frame
…: so more feedback would be appreciated.

NickD: Why wouldn't this work from the main page content?

Steve: Current federated login folks (at least a few years ago) have iframe postMessage back the login token
…: but on future navigations, the frames don't have access anymore, bc would need new user interaction.
…: Not ideal to have sites store all state in the main frame (since other parties can access).

NickD: So the issue is should 3p frames have storage access w/o user gesture / activiation.

Steve: Yep!

JohnW: Might be best to discuss in seperate issue, but this sounds more like SSO than federated login.
…: Since federated login can be 1p auth which is expected to be stored in first party, while SSO targets auth using  3p.

Tanvi: Could also be a seperate ad hoc meeting.

Tess: Lets start with an issue

JohnW: We want developers involved, not just implementors

### Q&A

Kris: Speaking as a developer (Salesforce), we prefer per-page, b/c each product might have its own
…: state, and there might be one party on the page we want to have access, and other folks on the page
…: to piggyback on that access.

JohnW: This sounds complicated, but if there are multiple 3p frames on the page, they can talk to each other.
…: Could you (Kris) describe this in a generalized way in an issue?

Kris: Yep, will do!

James: Also as a developer, we often have multiple iframes on a page, and would like to not have to have them
…: talk with each other.  Could we have all frames from the same 3p get elevated access if one frame from the
…: 3p gets access?

JohnW: Haven't thought of it before, sounds interesting.  Lets discuss more in an issue.
…: Seems like trend is towards per-frame is the default behavior.
…: Need more data points.  But need more info on what would work or break.

Anne: What does frame cover when dealing with nested frames?  If an embedded frame gets access what happens?
…: Seems like this should be syncronized, e.g. all frames from the 3p get elevated access if one frame
…: from that 3p gets access.

JohnW: WebKit currently only allows immediaediate children of the top frame to get access.

Anne: So nested frames see different state than immediately nested frames?
…: Sounds bad for interop / webcompat. (In particular the bit about WebKit relying on implementation details.)

Tess: Has WebKit seen related bugs?

JohnW: Dropbox does something related (w/ nested frames), and conversation w/ Drobox is ongoing.
…: Seems like there should be access for nested iframes.

Tess: Is there an issue?

JohnW: No

Tess: Seems like there should be.  Anne?

Anne: filed https://github.com/privacycg/storage-access/issues/14

JohnW: Question is whether access should be granted under the immediate parent frame, or under 1p / top level frame.

Anne: Is this due to imprecise termonology?

JohnW: Okie, issue about improving terms sound good?

Anne: Termology problems abound.  It seems like all 3p frames under the same origin should have the same state.

JohnW: If you open an issue I can point you to relevant discussion.
…: Cookies (which need to by syncronized) are one issue.  But async storage (like IndexDB) are even trickier, since
…: you need to syncronize state.

Kushal: Re preserving access across navigation / sessions.  One thing we (scroll.com) have seen is that
…: users expect to be logged out of all sites when they clear state.
…: However because scroll.com doesn't have access to all storage under all sites by default
…: scroll.com can't log users out for them.

Aram: better termonology would be ideal.  Developers might not understand how this functionality would
…: effect their use cases. 

#### Comments from the Zoom chat log

Zach: IMO on the storage access API - all modern user data privacy frameworks are requiring organizations identify categories of data sharing and most of these user data legal frameworks are requiring that orgs let users segment their access/sharing. The storage access API should tie directly into user data consent-sharing business categories and only let a 1st party request scope/storage into a 3rd party iframe when the consent string is also sent along with the storage access into 3rd party iframe.  Data revocation & deletion requests into the storage access API must have consent IDs corresponding back to users 1st party context and the 1st party data controller is responsible for submitting the 3rd party storage access deletion requests to data processors using the storage access API. 

Zach: Users should be able to pick/choose which 3rd party companies are allowed to receive access from their 1st party visit and pass it through the Storage Access API and also be able to submit deletion/access requests through the 1st party in a way that makes it possible for the 1st party to submit the deletion request to the 3rd party via a user’s consent strings/IDs.

## Proposals

### [Full storage partitioning / double-keying](https://github.com/privacycg/proposals/issues/4)

Tess: This was added by Anne

Anne: Who is not familiar with cache storage partitioning?

Tess: Some folks likely :)

Anne: If site A is in a tab, and B is in a tab, B should not be able to determine you visited A.
…: Cache partitioning is a useful protection against these cross origin leaks.
…: Ideal is to use the top level site as an additional storage key (e.g. each top level origin gets its own cache bucket).
…: Difficult to make this work with cookies and SA API.
…: Safari seems ahead in cache and storage partitioning.  SA API only affects cookies in Safari currently (?)
…: but eager to see what others think.
…: Research documents "XS leaks" by probing cross-origin cache (e.g. credit cards #s, Gmail).
…: You can likely learn other things too.  Plus, security.

JohnW: Safari supports bringing this behavior into PrivacyCG.
…: Currently Safari partitions caches and storage since 2013.
…: Cookies are diff, only did for one year (2018-2019) but saw breaking
…: Bc servers would see inconsistant cookie state.
…: Safari conclusion -> partitioning works best for client side state.
…: things that touch the server are trickier.

Anne: But only for stateful storage?  Not caches?

JohnW: Yes, Safari has been doing partitioning for a long time.
…: developers have already adapted to partioning behavior.
…: Safari is interested in more cross-browser behavior
…: But unsure about key-scope (origin vs eTLD+1) or storage scope (e.g. HSTS)
…: If there's a wide interop effort, we will try to match it.

NickD: Thank you for the explaination; the issue proposal gave me a diff understanding
…: thought it was for privacy through explicitly stored values by embedded third parties
…: but I appreciate that it covers more threats (cache probing, history sniffing, etc) by first parties
…: Good for an explainer to be explicit on which threat model

Anne: [Ongoing work in WHATWG](https://github.com/whatwg/fetch/pull/943).  E.g. HTTP cache, extending to other caches
…: would be good to have explainaner, and document the threat model.

Tess: Goal here is to have a single place to track all the related subissues

Anne: So far only targeting simpler things (caches), where there is less chance of breaking sites.
…: for storage (e.g. things like indexDb), its trickier.

Kris: First party sets are useful here, to link sites together, to enable non malicious uses.
…: there are also data sharing relationships between diff 1ps.
…: would be good to have way in spec to define this relationship.

pes: 1p sets pushes against privacy benefits
…: users wont always share 1p connection.

Kris: Need trade off, users dont want to be reprompted a lot too

Aram: Also need to consider site owners who operate under multiple eTLD+1
…: (e.g. Conde Nast and signon)
…: would be good to have API allow a single signon across these multiple sites.
…: Its not outside user expectations that one org covers multiple domains, and have the API respect that.

JohnW: First party sets was first proposed by Apple (then called associated or affiliated domains) in WebAppSec
…: Since then, Apple has concerns about collapsing lots of domains into a single entity (re API).
…: Apple has filed issues.  One proposed solution is to not allow dynamic sets.
…: The counter proposal is static lists (a la public suffix list).
…: There would at least be a place folks could look to see what the related domains are.
…: But there is concern b/c users may not share our understanding of which origins are related.

Tess: time is short, we have to move on, please move convo to GH

## Meetings

### [Request for meeting for JS Membranes proposal](https://github.com/privacycg/meetings/issues/2)

Tess: Brave asked for a conversation re this proposal
…: If you're interested in discussing, please comment on the issue to figure out dates / times / etc

### New zoom meeting info

Tess: next, zoom meeting info.  Below is the new meeting info (posted by Tanvi)

Tanvi: we're moving to a new Zoom meeting so that Tanvi (and chairs generally) can mute participants and
…: so that there isn't a possible conflict with PING folks using this zoom account at the same time.

```
Topic: Privacy Community Group Teleconference
Time: This is a recurring meeting Meet anytime

Join Zoom Meeting

Meeting ID: 769 153 986
https://mozilla.zoom.us/j/769153986

One tap mobile
+16699009128,,769153986# US (San Jose)
+16465588656,,769153986# US (New York)

Dial by your location
        +1 669 900 9128 US (San Jose)
        +1 646 558 8656 US (New York)
        877 853 5257 US Toll-free
        +61 2 8015 6011 Australia
        +61 3 7018 2005 Australia
        +61 8 7150 1149 Australia
        1800 893 423 Australia Toll-free
        +1 647 558 0588 Canada
        855 703 8985 Canada Toll-free
        +33 1 7037 2246 France
        +33 1 7037 9729 France
        +33 7 5678 4048 France
        0 805 082 588 France Toll-free
        +49 30 5679 5800 Germany
        +49 695 050 2596 Germany
        +49 69 7104 9922 Germany
        0 800 724 3138 Germany Toll-free
        +852 5803 3730 Hong Kong, China
        +852 5803 3731 Hong Kong, China
        +852 5808 6088 Hong Kong, China
        800 906 780 Hong Kong, China Toll-free
        +44 203 481 5237 United Kingdom
        +44 203 481 5240 United Kingdom
        +44 131 460 1196 United Kingdom
        +44 203 051 2874 United Kingdom
        0 800 031 5717 United Kingdom Toll-free
Find your local number: https://mozilla.zoom.us/u/a68iWilBp
```

## [Spring 2020 Virtual F2F](https://github.com/privacycg/meetings/issues/3)

Tess: Had hoped to have a face to face this spring, but COVID-19 makes that not possible
…: current [w3c guidance](https://w3c.github.io/Guide/meetings/continuity.html) on f2f meetings -> i.e. dont
…: we will have a virtual f2f meeting
…: please see [Virtual F2F scheduling issue](https://github.com/privacycg/meetings/issues/3)
