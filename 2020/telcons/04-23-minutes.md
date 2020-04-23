# Privacy CG telcon - 23 April 2020
* Chair: Tess
* Scribe: Pete 

## Attendees
* Erik Anderson (co-chair, Microsoft)
* Tanvi Vyas (co-chair, Mozilla)
* Theresa O'Connor (co-chair, Apple)
* Martin Meyer (Akamai)
* Pete Snyder (Brave Software)
* Pranjal Jumde (Brave)
* Max Gendler (NY Times)
* Wendy Seltzer (W3C)
* Jacek Oleksy (Opera)
* Don Marti (Consumer Reports, GitHub: dmarti)
* James Hartig (Admiral)
* Sam Macbeth (Cliqz)
* Steven Valdez (Google)
* Brad Lassey (Google)
* Tara Whalen (Google)
* Ben Savage (Facebook)
* Jack Frankland
* Shivan Sahib (Salesforce)
* Sam Tingleff (IAB Tech Lab)
* Peter Saint-Andre (Mozilla) 
* John Wilander (Apple WebKit)
* Carlos Bracho (Axel Springer)
* Thomas Bergemann (Axel Springer)
* Anuvrat Singh (Amazon)
* Ketan Deshpande (Amazon)
* Thomas Steiner (Google)
* Marijn Kruisselbrink (Google)
* Michael Kleber (Google)
* David Senecal (Akamai)
* Kris Chapman (Salesforce)
* Kushal Dave (Scroll)
* Christine Runnegar (PING co-chair)
* Johann Hofmann (Mozilla)
* Chris Pedigo (DCN)
* Steven Englehardt (Mozilla)
* Sam Weiler (W3C/MIT)
* Nicolas Arciniega (Microsoft)
* Allan Spiegel (Adobe) github: qualitybydesign
* Jeffrey Yasskin (Google)
* Paul Jensen (Google)
* Kaustubha Govind (Google, GitHub handle: krgovind)
* Igor Alferov (Fiducia DLT)
* Marshall Vale (Google, GitHub username: mvale)
* David Benjamin (Google, GitHub username davidben)
* Bill Densmore, Information Trust Exchange Governing Assn.
* Wendell Baker (Verizon Media)
* Joey Salazar (ARTICLE 19)
* Devin Rousso (Apple)
* Shivani Sharma (Google, GitHub handle: shivanigithub )
* Vibha Sethi (Verizon Media)
* annevk (Mozilla)
* Cory Aitchison (Adobe)
* Sebastian Zimmeck (Wesleyan University)

## Zoom

If you're having trouble with the Zoom link, please try this link instead: https://zoom.us/wc/join/769153986?pwd=

## Agenda
* Introductions
* Overview for new participants
    * General group info
    * How we work on GitHub
* Charter
    * [2020-04-15 charter updates](https://github.com/privacycg/admin/issues/13)
    * [Client-Side Storage Partitioning is a Work Item](https://github.com/privacycg/admin/issues/14)
* Proposals
    * [Registry of Businesses and Domain Name Ownership](https://github.com/privacycg/proposals/issues/11)
* [May 2020 Virtual F2F](https://github.com/privacycg/meetings/tree/master/2020/05-virtual)
* Any other business

## Introductions

[many introductions, mainly from google folks, bc google joined the CG.  Also new reps from Adobe and Verizon.]

## Overview for new participants

https://www.w3.org/community/privacycg/participants

* Tess: provides overview for PrivacyCG procedures.  Group started in Jan, w/ > 150 participants
* …: Most work is done on Github (proposals, specs).
* …: org is https://github.com/privacycg
* …: If you'd like to be added to the org, please add your GH handle next to your name above in attendees section
* …: there are three main repos where work occurs
* …: [proposals repo](https://github.com/privacycg/proposals), file an issue to start disucssion on a new topic
* …: [meetings repo](https://github.com/privacycg/meetings), used to organize meetings, virutal and otherwise
* …: [admin repo](https://github.com/privacycg/admin), used for anouncements, procedural questions, concerns
* …: mailing lists: public-privacycg@w3.org for discussion
* …: there is also a mailing list for talking with the chairs for issues not comfortable discussing on GH as described in [SECURITY.md](https://github.com/privacycg/.github/blob/master/SECURITY.md) (note: email alias group-privacycg-chairs@w3.org)

## Charter

* Tess: Charter has changed twice since the last call (this is uncommon).
* …: [first charter update](https://github.com/privacycg/admin/issues/13), formalizes the role of a proposal champion, which is similar procedure to TC39
* …: second, the rules for how proposals become work items (i.e. specs).  More specifically, allows proposal champions and chairs to work together when guiding proposals forward
* …: also, the definition of an "implementor" was removed, since it was complicated and not useful
* …: If folks have questions, ask here or on the above issue 13
* Tess: [second charter update](https://github.com/privacycg/admin/issues/14), which adopts client side storage partitioning as a PrivacyCG work item

## [Registry of Businesses and Domain Name Ownership](https://github.com/privacycg/proposals/issues/11)

* Jack Frankland: This is the result of a "do not sell" flag from previous PrivacyCG calls.
* …: Argues for the need for authorities that bizes can register with to designate they're a trusted owner of user data
* …: that registration would be tied to a domain name, and include information about what legal requirements they followed (e.g. GDPR CPAA, etc)
* …: and possibly what 3p the organization contracted with.
* …: This signal could be used by the browser to determine what features / storage / similar sites (and embedded parties) have access to.
* …: (e.g. un-registered parties might not get access to storage)
* …: Additionally, the browser could use these signals for auditing purposes, and to determine which organizations have access to which stored values
* John Wilander: This seems related to first-party sets.  Google and first party sets spec authors are here, would like to hear their htoughts
* …: Q1: what to do about misuse of these lists (can people register for each others' domains)
* …: Q2: whats the update cadence? 
* …: Q3: have you considered also registering the purpose of each domain name with the registration
* Jack Frankland: First, re list misuse, makes it more important to limit to trusted authorities (e.g. ICO has a registery for who is a data controller).  User agent could trust / key off this/
* …: To prevent misuse, would need to be a mechanism to prevent fradulent registrations or folks using data for non-advertised reasons
* …: ICO requires annual updates, something similar could be done here.
* …: re purpse, registration could include the legislation the domain follows, and/but could also include other info
* Ben Savage: I like the idea of business saying what legislation they'll follow, and that this could be split off by domain (e.g. CDN does X, but advertising domains / URLs does Y)
* …: proposal mentions things like GDPR as privacy standards
* …: these will change quickly, might be better to have industry agreed to "privacy primitives" indepdent of existing legislation
* …: similar, these industry standards might also clarify what certain "terms of art" mean (e.g. account deletion means X under Y stnadard)
* Kaustubha Govind: I am one of the authors of first party sets (others are on the call)
* …: 1) Proposal assumes that browser trusts the .well_known advertised information
* …: First party sets proposal has changed to require that the first party set assertion be signed
* …: 2) As JohnW said, mergers and similar can make things complicated
* …: the current proposal has and explores a header based way of sites opting into a set
* …: Additionally, first-party sets proposal anticipates all parties in the same set having shared 1p storage
* …: Also, the UX mock in the proposal is similar to what Google has in mind
* Brad Lassey: This proposal has some overlap with first party sets
* …: Also considering using EV certs to assert first-party set membership (though EV cert history is complicated and tricky)
* …: [Issue 12](https://github.com/krgovind/first-party-sets/issues/12) on the first-party set explainer has more details
* Sam Tingleff: IAB framework has some overlap
* …: I dig the proposal, there is similarity with concerns on transparency and consent
* …: incentives are the difficult part
* …: First party sets might be one incentive to get people to register, but getting participation is a challenge
* …: Ben mentioned privacy standards, IAB did something similar.  Brazil and Canada (maybe others) seem likely to have their own propsoals
* …: we should have a global privacy standard, tech lab is trying to get folks into a global policy body to set privacy baselines
* Peter Saint-Andre: to echo previous points about EV certs, there are well known, historically exploded landmines in the area
* …: second, a lot of functionality is getting hung off this idea (privacy policies, storage access api, first party sets)
* …: would be better to narrow the scope
* Kris Chapman: re incentives
* …: users should move away from reasoning about domains, and move to thinking about orgs
* …: if this is a way to get transparency to users about how data is being shared
* …: thats a big incentive for adopters / orgaizations
* Wendell Baker: I dig it
* Pete: What started off as a proposal to _reduce_ amount of information being shared is moving in a direction of allowing people to share _more_ information across domains.  Need to be cautious of sensitive information moving around.
* Jack: I didn't realize the overlap with first-party sets
* …: second, the goal here is to advance user privacy, though allowing sites to share user data (with transparency) is an incentive to get adopters
* Tess: please continue disucssion on github in the relevant issue (linked above)

## [May 2020 Virtual F2F](https://github.com/privacycg/meetings/tree/master/2020/05-virtual)
13–14 May from 15:00 to 19:00 UTC.

* Tanvi: there will be a virtual face to face coming up (date above).
* …: There will be a total of 8 hours (w/ breaks).  Will be one track.
* …: currently soliciting agenda items; please tag on github
* …: would be good to have presentations too; if you have something you'd like to present please let the PrivacyCG chairs know
* Tess: Im most interested in disucssing partitioning

## Any other business

### Announcement ad-hoc meeting on Bounce Tracking right after this

* JohnW: Will we be using the same cryptpad (what you're reading now) and zoom
* Tess: Yes
* Tanvi: We prefer video on for adhoc meetings when possible, to engage better
