# 2024-01-11 Privacy CG

* Chair: Tess
* Scribe: Pete Snyder

## [Agenda](https://github.com/privacycg/meetings/blob/main/2023/telcons/12-14-agenda.md)

* [GPC](https://github.com/privacycg/gpc-spec)
    * [Use HTTP structured field values #6](https://github.com/privacycg/gpc-spec/issues/6)
    * [Are implementers expected to always have the header active or not? #60](https://github.com/privacycg/gpc-spec/issues/60)
    * [Should the navigator property always have the property active or not? #62](https://github.com/privacycg/gpc-spec/issues/62)
* Any other business
    * Slack instance
    * Meeting cadence

## Notes

### [Use HTTP structured field values #6](https://github.com/privacycg/gpc-spec/issues/6)

* Pete: There has been a request to use structured headers instead of existing syntax
    * Editors have decided against
    * Currently already a lot of implementation so don’t want to confuse
    * Also, designed not to be extensible — it’s meant to be a very binary signal
* Jeffrey: Well, there is a general preference not to do new headers that aren’t structured
    * Don’t just want to rubber stamp because it’s already shipped
    * A desire to possibly apply to new rights going forward — structured header could make easier
* Tess: 
    * Re Existing processors, if this was changed to structured headers, would existing consumers break?
    * One possibility is that they break, but its also possible that existing implementations would handle a structured header correctly
* BenV
    * Agree with Jeffrey, that just bc something has shipped doesn’t mean it shouldn’t be changed
* Justin
    * No strong feelings around structured vs not structured.
    * GPC is widely deployed. THere has been enforcement actions (1) around the header, in CA
    * Other states are expected to also support, so very important to consider when making changes
* Jeffrey
    * Spec currently [requires](https://privacycg.github.io/gpc-spec/#the-sec-gpc-header-field-for-http-requests) ?1 to be equiv as absent
* Tess
    * Possible future versions / alt spellings could also allow the existing behavior / name too
* Nick
    * Structured headers spec might also cause the current value “1” to count as absent [From Jeffrey: [https://www.rfc-editor.org/rfc/rfc8941.html#name-booleans](https://www.rfc-editor.org/rfc/rfc8941.html#name-booleans) has "Note that in Dictionary (Section 3.2) and Parameter (Section 3.1.2) values, Boolean true is indicated by omitting the value."]
    * Being deployed doesn’t prevent changes, but it matters
    * Current behavior is very common in headers
* Tess
    * Re Justin: just bc states require the law to be respected does not mean in practice software is widely deploy
    * Re extensibility: we’ve had similar efforts like DNT, we might want to change things going forward
* Ben
    * In structured headers, current value would be “integer 1”
    * We could maintain the current name but with the expectation that it’d be removed in the long term.
    * Firefox would change its current implementation if needed 
    * [https://well-known.dev/?q=gpc_support%3Atrue#results](https://well-known.dev/?q=gpc_support%3Atrue#results) has deployment information.
    * 
* Sebastian
    * Re deployment, we can measure deployment by looking at .well-known deployment, inc large sites like amazon
    * Re extensibility, the spec is intentionally undefined on what the value means, the regulator decides what the signal means. Extensibility could come through regulatory interpretation instead of the vale sent
* Tess
    * The existing value (“1”) is a valid value (integer 1). But existing software has conformance / compatibility classes. Same approach could be taken here; servers could be instructed to be more flexible and understand ?1 and “1” as equiv
* Pete
    * I favor a dumb “1” response to make it clear that the group (and this feature) thinks fine grained approaches to this problem are not good (and in particular the approaches similar tools have taken)
* Aram
    * Some of the values of headers would need to be flipped, in some folks opinions, to comply with the expected behavior of SEC headers
* Tess
    * That concern is Sec-GPC vs GPC, not the value
* Jeffrey
    * The meaning of bool true in structured headers is not the same as integer true in structured headers
* Ben
    * Proposal is to have the new structured header be spelled differently but interpreted the same
* Aram
    * I'm not totally clear on the specifics of structured headers; implementers and consumers of the values might also be unclear. Thats high risk given the legal impacts
    * Do other implementers consider structured/not-structured to be a blocker?
* Tess
    * As a chair and not an Apple rep, i observe that other folks from other browsers would like to see this change

### [Are implementers expected to always have the header active or not? #60](https://github.com/privacycg/gpc-spec/issues/60)

* Aram
    * Question is whether the header should always be present, regardless of the the user’s preference (or whether the user has made a preference)
    * Initially thought was “omit the header if the user hadn’t made a preference”
    * Does the group / PrivacyCG have a preference here, on whether the Sec-GPC header should always be sent, regardless of user preference
* Ari
    * I assume the header shouldn’t be sent (instead of Sec-GPC: 0)
* Nick
    * I agree with Ari. Lets close this and move on to issue 62, which is a similar question but dealing with the Web API property
* Aram
    * No objection to not sending the header; thats my preference
* Brian May
    * I’d expect that no header => no preference (and no preference => no header)

### [Should the navigator property always have the property active or not? #62](https://github.com/privacycg/gpc-spec/issues/62)

* Aram
    * This question is similar to the previous issue, but dealing with the navigator JS property, instead of the HTTP header
    * Two concerns: first, should the property be set is the user hasn’t expressed a preference, and second, is there a general practice of keeping the presence of the navigator property in sync with the presence of the HTTP header
* Ari
    * Seems surprising if the property would or wouldn’t exist, depending on the user setting
* Ben
    * Agree with Ari; having the property be either `true` or `undefined` is odd
    * As an implementer, adding a `false` value is equally as fingerprintable as an `undefined` value
* Nick
    * I agree, no fingerprinting concerns from browser POV. Could be diff for an extension
    * Don’t think it matters in the spec 
    * Seems unlikely that this would trip up site authors in practice
* Tess
    * Re feature detection, generally sites check for a property to know if a browser supports a feature
    * If there is an extension that wants to avoid detection, then the extension could violate the spec.
    * Implementers generally want to make it clear to site that the browser supports the feature
    * This doesn’t seem a like a problem being solved by ever omitting the property
* Aram
    * No point in doing feature detection for GPC, this is unlike doing feature detection for CSS feature support
    * Feature detection is probably also not useful bc sites could determine support based on browser version and the major browser version will always be available and will indicate this. 
    * From a developer POV, `false` vs `undefined` doesn’t matter, only `true` matters. So, doesn’ really matter as a developer
* Justin
    * +1 for what Nick and Aram said (that its not an important decision if `false` and `undefined` mean the same thing).
    * Spec should just be explicit about the above
* Ben
    * The above matches my understanding, and I think the spec already says this
    * Having a false value being set is useful for detecting opt-out rates

### Slack changes

[w3ccommunity slack](https://w3ccommunity.slack.com/) #privacycg #privacycg-github ([invite](https://www.w3.org/slack-w3ccommunity-invite))

* Tess
    * All PrivacyCG discussions have been moved to the W3C community slack (and away from the W3C Ping Slack instance). The latter still exists, but don’t use it, it archive
    * Good to have less slack instances, and will make it easier to have conversions with folks in other parts of the W3C
* Nick
    * If you already, previously had a W3C community slack account, you HAVE NOT automatically been added to the new PrivacyCG channels, you need to join those channels on their own

### Meeting cadence

* Tess
    * We canceled a lot of meetings last year for lack of agenda items. Having two calls a month might be more than what's needed. Maybe we’d be fine w/ a monthly call?
    * We also have two time slots, with every 4th call being more convenient for Asia/Pacific time attendees. We could also change this
    * Please think about whether our meeting frequency or call times should change
    * In Feb we’ll try just having one call and see how it goes
    * Which calls will be at what times is still TBD
* Brian
    * I'm in a lot of groups, so if we move to one call a month, it might be hard for me to go back to two months
* Ari
    * I prefer more frequent, shorter calls, even if it means sometimes canceling a call

## Attendees

* Erik Anderson (Microsoft Edge)
* Aaron Selya (Google Chrome)
* Theresa O’Connor (Apple)
* Pete Snyder (Brave)
* Chris Needham (BBC)
* Brian May (dstillery)
* Chris Fredrickson (Google Chrome)
* Justin Brookman (Consumer Reports)
* Paul Zühlcke (Mozilla)
* Nick Doty (CDT)
* Sam LeDoux (Google Chrome)
* Benjamin VanderSloot (Mozilla)
* Eyal Abadi (P&G)
* Aram Zucker-Scharff (The Washington Post)
*  (Wesleyan University, GPC team)
*  (Google Chrome)
* Matthew X. Economou (Research Data and Communication Technologies)
* Jeffrey Yasskin (Google Chrome)
* Philippe Le Hegaret (W3C)
