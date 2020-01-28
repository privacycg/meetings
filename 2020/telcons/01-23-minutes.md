# Privacy CG telcon - 23 January 2020

* Chair: Tess
* Scribe: Pete Snyder

## Attendees (sign yourself in)

* Erik Anderson (co-chair, Microsoft)
* Theresa O’Connor (co-chair, Apple)
* Tanvi Vyas (co-chair, Mozilla)
* Pete Snyder (Brave)
* Irene Knapp (no affiliation)
* Wendy Seltzer (W3C)
* Konrad Dzwinel (DuckDuckGo)
* Jason Dorweiler (DuckDuckGo)
* Dave Harbage (DuckDuckGo)
* Sam Weiler (W3C)
* Nick Doty (UC Berkeley)
* Matt Finkel (The Tor Project)
* Wendell Baker (Verizon Media)
* Kushal Dave (Scroll)
* Steven Englehardt (Mozilla)
* Peter Saint-Andre (Mozilla)
* Ehsan Akhgari (Mozilla)
* Arthur Edelstein (Mozilla)
* Mark Xue (Apple)
* Jacek Oleksy (Opera)
* Martin Meyer (Akamai)
* Christy Harris (Future of Privacy Forum)
* Dick Hardt (SignIn.Org)
* David Dabbs (Conversant)
* Nicolas Arciniega (Microsoft)
* John Wilander (Apple)
* Kristen Chapman (Salesforce Marketing Cloud)
* Harneet Sidhana (Microsoft)
* Christine Runnegar (PING co-chair)

## Slack details

* https://w3cping.slack.com
* channel - `#privacycg`

## Agenda

* Introductions
* Welcome from the chairs
* How we work
    * Public and private communication
    * Meetings
    * Proposals
    * Deliverables
* Current Proposals
    * [Private Click Measurement](https://wicg.github.io/ad-click-attribution/index.html) ([#1](https://github.com/privacycg/proposals/issues/1))
    * [Storage Access API](https://github.com/whatwg/html/issues/3338) ([#2](https://github.com/privacycg/proposals/issues/2))
    * [JS Isolation via Origin Labels and Membranes](https://docs.google.com/document/d/1GFWONU2lq9ukQoj6dIGudOO4P3op7a1xt75Gb_jAA1c/edit#heading=h.8blcqbqrr76o) ([#3](https://github.com/privacycg/proposals/issues/3))
* Any other business

## Introductions

(round of introductions by all attendees)

## Welcome from the chairs

- Tess: PING is use for privacy reviews, but can't introduce new specs.  This group is meant as a complement to PING.  This group's output may have implications across the rest of the web platform (e.g. fingerprinting).  WICG is a incubating group where new proposals can get legs and then move to a working group for full standardization.  WICG targets "early stage" work, where failure is okay (failing isn't a failure).  This group will be similar, where privacy ideas can be discussed across browsers and other community members.  If and when some of these are ready, they'll be moved to WG too, parallel to WICG.  

## How we work

- Tess: This group will have (i) proposals and (ii) deliverables.  

### Public and private communication

- Tess: This group will aim to be very transparent.  We'll work in public wherever possible (e.g. GitHub).  This is the default.  When necessary we can use private communications though (e.g. privacy mailing list for W3C members).

- Tess (cont): Our work will be async, to make it more inclusive (across timezones, languages, availability, etc).  We will also have sync meetings (face to face meetings, phone calls), but decision will be async to maximize inclusion.  All decisions made in sync meetings are therefor tentitive until async commitments / discussion.

### Meetings

- Tess: We want to focus on including multiple voices, including the existing privacy interest group (PING).

- PING - 1st and 3rd thursday.
- Privacy CG - 2nd and 4th thursday.
- Same dial in details; same slack instance - different channel and different cryptpad.

- Tess (cont): PCG will use the same communication methods as PING: Zoom for calls, cryptopad for minutes

### Proposals

Tess: Our proposals will be discussed on our [Github account](https://github.com/privacycg/proposals).  We hope that some of these proposals will firm up, and once they do, proposals will become deliverables.  In order for proposals to become deliverables, they must have support from multiple browser implementors.  This "multiple implemetor" requirement is because the web requires interop between different browsers.  For privacy improvements to succeed in making the web _over all_ more private (not just a single vendor) we need to make sure there is buy in from browsers.

### Deliverables

- Tess: Once multiple browsers have supported a proposal, the proposal becomes a deliverable.  Deliverables have two parts (i) explainer and (ii) spec.  The explainer describes the problem being solved; the spec is the solution to the problem.

- Nick Doty:  How implementation driven will the decision process be?  How to adjudicate between different proposals, which both have multiple implementations?

- Tess: The charter details working mode and decision process, that's the authority.  This process is based on the WHATWG (a standards body that works on foundational standards like DOM, HTML).  It relies on having an editor for each spec, both for writing and revising specs.  If there are disagreements though, the charter has process for escalation.  The proposal process is designed to have a low entry cost (i.e. issues on github).  Deliverable editors are appointed by PCG chairs.  Expecation is the person championing the proposal will be the editor.  

- Tess (cont): this is not a group just for browser vendors; its a group for the larger set of people who want to influence browsers.

- Pete: process for objecting to proposals?

- Tess: yeah, it's in the charter. Anyone can object to a decision that gets made. Charter details how that would be handled. Goal is for specs to be moved to working groups when ready (e.g. proposal -> deliverable -> working group). So deliverables from PCG might also take the form of pull requests (PRs) on specs owned by other groups. If the objection fails to prevet / convince PCG from moving deliverable -> working group, the recourse is to try and convince the working group.

- Dick Hardt: Are issues of mobile apps in scope?

- Tanvi: unsure: we might lack expertise and leverage, but if we can we will.

- Tess: There are many technologies that make up "the web"; we should also consider the interface between browsers and these other parts of the web, seems in scope.  BUT we should also make sure to stay in scope of W3C, which targets Web (though the line is fuzzy).

- wseltzer: We should think of the web broadly: things in browsers, data interchange, mobile apps, etc.  We should think of these items too.

- Irene Knapp: Mobile privacy is a web issue in multiple ways: (i) advertising, (ii) analytics, (iii) cookie joining.

## Current Proposals

### [Private Click Measurement](https://wicg.github.io/ad-click-attribution/index.html) ([#1](https://github.com/privacycg/proposals/issues/1))

- Tess: John Wilander is the editor this spec, which is currently in WICG.

- John Wilander: Spec is ment to provide attribution for clicks across websites, in a post tracking web. Apple authored proposal to allow the collection of aggregate stats w/o identifing the individual(s) doing the clicking.

### [Storage Access API](https://github.com/whatwg/html/issues/3338) ([#2](https://github.com/privacycg/proposals/issues/2))

- Tess: This is currently an issue filed on the HTML spec.

- John Wilander: Addresses some of the effects of blocking 3p cookies / storage.  Gives third-party iframes the ability to ask for permanant, cross first-party storage.  Safari's implementation has an explicit prompt.

### [JS Isolation via Origin Labels and Membranes](https://docs.google.com/document/d/1GFWONU2lq9ukQoj6dIGudOO4P3op7a1xt75Gb_jAA1c/edit#heading=h.8blcqbqrr76o) ([#3](https://github.com/privacycg/proposals/issues/3))

- Pete: Proposal here 3 audiences - 1) browser implementors 2) extension authors, and 3) parties that contain 3rd party script. Situations where you'd like to include third party script on my page without giving it access to everything on the page. Isolation parallel to iframes - on the page but not given all access.  Proposal designed to allow the containment without rewriting the script being contained (which is great so script doesn't need to be rewtritten).  Done in a behavioral way; access control policies through javascript itself.  Nothign new introduced to language, uses exsiting javascript.

- Tess: Its great that there are already 3 proposals.  Others who have ideas or proposals, please do so; just open an issue on github.  Future calls might allow more time for disucssion of proposals.

## Missed Introductions
Anyone who didn't get to introduce themselves?

## Any other business

- Tess: Slack group is managed by PING.

- Tanvi: there will be an email going out to answer how to be added to slack.  All PCG members are on this mailing list

- Pete: is PCG okay as an abbreviation?

- Tess: "privacycg" is the W3C shortname for the group.

- Tess: I prefer "Privacy CG" or "Privacy Community Group", but you can use what you prefer.

## Queue
