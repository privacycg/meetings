# 2023-10-26 Privacy CG
* Chair: Martin
* Scribe: Matthew


# [Agenda](https://github.com/privacycg/meetings/blob/main/2023/telcons/10-26-agenda.md)
* [GPC Explainer](https://github.com/privacycg/gpc-spec/issues/56)
* Adoption feedback on
    * [Opener Storage Partitioning](https://github.com/privacycg/proposals/issues/43)
    * [Third-party Cookie Access Heuristics explainer](https://github.com/privacycg/proposals/issues/42)
    * And maybe [Extending Storage Access API (SAA) to non-cookie storage](https://github.com/privacycg/proposals/issues/41)
* Any other business

# Notes
Scribe: Matthew X. Economou (RDCT)

* Global Privacy Control (GPC) Explainer:
    * corresponding issue on GitHub: [https://github.com/privacycg/gpc-spec/issues/56](https://github.com/privacycg/gpc-spec/issues/56)
    * mostly about legal issues—added additional details about UX w/r/t implementing defaults in compliance with relevant law, how user agents and web sites are responding to GPC
    * issue w/r/t request header structure may need additional work, cf. [https://github.com/privacycg/gpc-spec/issues/6](https://github.com/privacycg/gpc-spec/issues/6)
    * issue w/r/t support for other privacy laws, cf. [https://github.com/privacycg/gpc-spec/issues/51](https://github.com/privacycg/gpc-spec/issues/51); existing spec narrowly focused on California and Colorado opt-out, another header might be suitable in long term? a second rights opt-out proposal is welcome
    * next steps: switch doc to Suggestions mode, request/review additional comments, convert to Markdown, submit pull request
* [Opener Storage Partitioning](https://github.com/privacycg/proposals/issues/43)
    * implementer interest? adopt here?
    * Ari has an action to ask for standards positions/gather signals for this.
* [Third-party Cookie Access Heuristics explainer](https://github.com/privacycg/proposals/issues/42)
    * implementer interest? adopt here?
    * both Gecko and Blink are interested
    * is formalizing these heuristics a good idea?
    * do they constitute proprietary product differentiation for browsers? e.g., this is how Safari distinguished itself from other browsers in the past. Gecko doesn't consider this a differentiating feature.
    * is this temporary? Gecko is interested in discussing this if a temporary measure
    * understandable timelines? how will these be turned off when deprecated? TBD, publications planned
    * what commonly supported vs. browser-unique heuristics exist?
* [Extending Storage Access API (SAA) to non-cookie storage](https://github.com/privacycg/proposals/issues/41)
    * implementer interest? adopt here?
    * local session storage and indexdb code done and trial next week
    * interest from both Mozilla and Safari
    * needs formal spec, within next two weeks?
    * any objections to adding PR to main storage access spec?
    * storage access editors want to finish existing work and keep this separate
    * also need to move explainer from personal repo to existing SAA repo
    * Ari will publish an IDL doc

## Attendees
* Martin Thomson (mozzarella)
* Ari Chivukula (Google Chrome)
* Justin Brookman (Consumer Reports)
* Matthew X. Economou (RDCT)
* Chris Fredrickson (Google Chrome)
* Kaustubha Govind (Google Chrome)
* Benjamin VanderSloot (Mozilla)
* Heather Flanagan (Spherical Cow Consulting)
* Brian May (dstillery)
* Johann Hofmann (Google Chrome)
* Sebastian Zimmeck (Wesleyan University, GPC Group)
* Erik Anderson (Microsoft Edge)
* Aaron Selya (Google Chrome)
* Sam Weiler (W3C)
* Dylan Cutler (Google Chrome)
* Tim Cappalli (Microsoft Identity)
