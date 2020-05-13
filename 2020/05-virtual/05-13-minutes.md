# Privacy Community Group F2F, 13 May

Chairs: Erik, Tanvi, Tess

## Agenda

* Welcome
* Privacy and Firefox
* Privacy and Edge
* Privacy and Chrome
* Privacy and Brave
* Privacy and Safari
* Privacy and Samsung Internet
* Privacy at the W3C
    * Privacy Interest Group
    * Improving Web Advertising Business Group

## Attendees TBD

## Introductions

Scribe: Peter Snyder

* Erik: minutes go here (in cryptpad)
* …: other details are [in Github](https://github.com/privacycg/meetings/tree/master/2020/05-virtual)
* …: conversation will happen in Slack; send message to the PrivacyCG chairs if you need access
* …: presentations are 10-15 min, and 10-15 min of questions, 25 min total
* …: This is PrivacyCG's first face2face (and virtual f2f).
* …: [166 folks from 60 orgs in PrivacyCG](https://www.w3.org/community/privacycg/participants), including all standards-engaged browser vendors, and involvement from other web stakeholders and invovled parties.
* …: (round of introductions, and some moderate cryptpad debugging)
* …: PrivacyCG is working as per [charter](https://privacycg.github.io/charter.html) taking up [proposals](https://github.com/privacycg/proposals) listed in the [repo](https://github.com/privacycg).  The goal is to use this time to find alighment on moving new specs forward on the web to improve privacy.
* …: There hasn't to date been a dedicated forum for discussing new privacy-focused browser features. 
* …: So immediate goal is to find where there is general agreement between implementors.
* …: Specs incubated in PrivacyCG will eventually be pushed to other working groups.
* …: The time today will be discussions about what browsers have done and are planning on doing.
* …: Tomorrow will be discussion about specific specs planned or currently being implemented.
* …: Went w/ two four-hour slots b/c of virtual f2f.  If we don't complete the agenda or need further discussion, we can have break out sessions, have future calls, and other wise figure out good venues for future discussion.
* Tess: GitHub is where most conversation happens in PrivacyCG.  Its where documents live and are disucssed (through issues).
* …: there is a PrivacyCG org on GitHub that folks should be members of, so that you can add labels, etc.
* …: If you're not currently a member, please message someone from PrivacyCG to be added.
* Wendy S: W3C mostly uses IRC; some groups experimenting with Slack.

## Privacy and Mozilla Firefox

(scribe: Jeffrey Yasskin)

### Presentation

Steven Englehardt presenting. [Mozilla slides](https://docs.google.com/presentation/d/1If6LBvKK0qS3bRRsfHvRqXsy0KlMHLVgQa_hrU42Vt0/edit?usp=sharing)

(Missed scribing of intro slides.)

[slide 7] Our goal: Make the web platform private by default. 
1. No cross-site communication methods by default.
2. Any cross-site communication governed by the Storage Access API, or another application-specific API (e.g., Private Click Measurement).
3. Support stateful applications that users want (first-party and third-party).
4. Monetization options that don’t rely on tracking

[slide 8] Defining privacy-violating uses of our browser. Security/Anti-tracking policy

Implement blocking - questions we have and what we’d like to solve in this group.

Stateful protection

Embedded content around the web, we block cookies from being sent in those embedded cases.
Policy review to add/remove people from the blocklist.  Also have entity list that don't block third party entity that is related to first party.  Take list of trackers and apply Enhanced Trackign Protection to it - blocks storage API and partitions some of the caches and network level connections by top-level ETLD+!.  Not all third parties - just the ones on the blocklist.

Near-term mitigations for site breakage - could cause breakage, users won't understand.  So implemented heuristics that automatically grant access (remove restrictions) upon certain interactions.  If user clicks login with social provider button, that creates a popup, common, then automatically access storage.  Users can also override our protections.  Near-term, allows us to get the protection out.

Longer term want to shift to safer and user controlled apis - Storage Access API prompt user etc.  risks of doing this, risks of saying okay.  Implemented in Firefox, workign on standardizing here.  And other stateful things we want to support, ex login API.  

Perioidically clear cookies from origins that are detected as trackers.  Inspired by Safari's implementation.  If cookies blocked in third party context, try to get user to visit you as first party and bounce through you as navigating through sites.  We removed cookies and site data from trackers once daily.  unless first party interaction.  Hasn't shipped yet, experimenting in Nightly.

Thinking about something that applies to all third parties - ex: paritioning.  All third parties under the website have storage access but the access they received is specific to the current first party (ex: double-keyed).  Different cookie jar than when interacted as first party.

Have First Party Isolation in the Tor browser - know there are web compatability issues like with cookei blocking.  working on how we can make it more web compatabile so we can ship it sooner.  There are some open questions when it comes to parititioning - are there are APIS that we prefer to block rather than partition and block them all the time in the third party cookies.  Ex: safari and cookies.  what about indexeddb, localstorage, how should we handle.  still thinkinga bout.

Questions around - Partiioned third party to non-partitioned third party.  Shoudl we swap?

protections against fingerprinting: use device properties as a fingerprint. harder to protect against than stateful tracking.  In Firefox 72 contenet blocking approach to anti-fingerprinting.  Parties that are known to disconnect as participating in fingerpritning.  Actually shared flagged fingerprinting scripts with Disconnect and they have do a very public review and then release a list of fingerprinters.  If other folks have expertise, would love to get those reports form folks here as well.

People have reached out to disconnect and said we've stopped the types of things we used to do - to try and get off the list.

Potentional direction to epxlore - anti-fingerprinting through API normalization.  Very difficult to ship.  Trying to make all Firefox users look the same regardless of th device they are on.  Requires doing a lot of non-standard things, and breaking lots of the web.  Are there ways to make this API level anti-fingerpritning approaches more compatabile so we can ship.

What's next - on the slides - out of time.  Thanks!

Open Questions: 
* APIs to block rather than partition?
* Should a storage access grant swap partitioned to non-partitioned storage?

(Yasskin switch to scribing now)


### Q&A

Sebastian: Interesting Work. How do you distinguish first parties from third parties? E.g. CDNs, you look at the domain that looks like afirst party, but it's different from the domain the first party is actually using? How do you make the distinction?

Steven: Depends on the type of protection. If we're doing first-party isolation, it's based onthe origin. No exceptions for being part of the "same organization". It's just the data available to the browser. When we _do_ have a list of companies, for Enhanced Protection and tracker-based blocking, we do use the idea that parties are part of the same organization. But we have trouble imagining how that scales to the whole web, in a way that can't be abused. Proabably have to do it at the origin/hostname level.

John Wilander: Thank you for the presentation. We hear a lot about profitability. "More profitable if we can target people and track customer journeys." How do we measure up to that force?

John: And about fingerprinting, we talk mostly about hardware fingerprinting, or browser-specific fingerprinting. How about user-specific and behavioral fingerprinting. Accessibility tools, etc. Have you or Tor done work there?

Steven: On profitability: Can't be ignored, but we should approach this by putting user privacy first. Shouldn't compromise on user privacy. Web platform has historically been not-private by default, and that's led to a lot of problems. Should rethink how to build profitable systems on top of a more private platform. Shouldn't put onus on users. Idea that "we should keep things open by default, and people who care can take steps to protect themselves", is a bad way to frame it. It means only experts can protect themselves.

Steven: On fingerprinting: I see your point. We're thinking about device fingerprinting, and there's a lot more out there. We don't even know how to solve device fingerprinting at scale, so we should start there. Tor has been worrying about what gets exposed when people install extensions. Awesome research papers that show extensions that add things to the DOM make users trivially identifiable. Need to do more work.

Nick Doty: Thank you for working on a lot. Want to talk about when partitioning makes sense, and when blocking makes sense. Want to scal to the whole web, instead of relying on lists. Seems like partitioning gives you all the things that blocking would give you. As long as local storage is partitioned, it doesn't give you a global identifier. 

Steven: Two aspects: Breakage: If we want to deploy this soon, what puts us down codepaths that work with current integrations? e.g. Safari has had a restrictive policy for a while, and what's web compatible for them might be different from Firefox, where third-party cookies have historically been allowed.
Partitioned storage allows a malicious actor to store an identifier in their third-party storage, and over time join sites. It's an even bigger risk when the malicious site is visited as a first party, and search engines and social networks that act as a gateway to the web might be able to correlate a lot of traffic.

David Senecal: I work for a web security vendor, providing anti-fraud, bug detection. We're bound to follow certain regulations like GDPR. Forces us to collect certain data and store it, and dispose of it. I see a problem that the privacy features start to impact the quality of fraud detection and bug detection. Makes the web more private but maybe less secure. Restricting data collection makes it hard to detect htat an account has been stolen. How can browser vendors and security vendors work together so that the browser vendor can recognize that the security vendor is using data responsibly, so the security vendor can still do their job?

Erik: That's a very general problem statement, and one worth more discussion in this group, but we're cutting into the break time, so please continue that discussion over Slack and in the [proposals repository](https://github.com/privacycg/proposals).

## How to join the Privacy CG

1. Get a (free) W3C account at https://www.w3.org/accounts/request.
2. Log into https://www.w3.org/community/privacycg/join with your W3C account.
3. Click "join".
4. (Sometimes) Wait for your organization to approve your request.
