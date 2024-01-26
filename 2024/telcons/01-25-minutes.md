# 2024-01-25 Privacy CG

* Chair: Erik
* Scribe: Matthew

# [Agenda](https://github.com/privacycg/meetings/blob/main/2024/telcons/01-25-agenda.md)
* [Storage Access Headers #45](https://github.com/privacycg/proposals/issues/45)
* [Extending Storage Access API (SAA) to non-cookie storage #41](https://github.com/privacycg/proposals/issues/41)
    * [Explainer: Extending Storage Access API (SAA) to omit unpartitioned cookies](https://arichiv.github.io/saa-non-cookie-storage/omit-unpartitioned-cookies.html)
    * [Explainer: Extending Storage Access API (SAA) to Shared Workers](https://arichiv.github.io/saa-non-cookie-storage/shared-workers.html)
* Any other business

# Notes

## [Storage Access Headers #45](https://github.com/privacycg/proposals/issues/45)
* Ben: Mozilla is supportive of working on this.
* Erik will work with other chairs to bring this into the repo.
* Chris: We’ll discuss issues in future calls.


## [Extending Storage Access API (SAA) to non-cookie storage #41](https://github.com/privacycg/proposals/issues/41) ()

Ari: Original proposal discussed at TPAC.  Two extensions based on dev feedback.  Additional feedback desired.

Goal is to discuss the two proposed extensions to [https://arichiv.github.io/saa-non-cookie-storage/](https://arichiv.github.io/saa-non-cookie-storage/) below:


### [Explainer: Extending Storage Access API (SAA) to omit unpartitioned cookies](https://arichiv.github.io/saa-non-cookie-storage/omit-unpartitioned-cookies.html)

Ari:
* Change the API to allow someone to omit unpartitioned cookies.
* With a parameter, one can request other kinds of storage, but one was always required to take on unpartitioned cookies in addition.
* This change makes it so that if one does not explicitly ask for cookies with other storage types, cookies won't be available.
* Storage access function will be renamed after a deprecation notice since the old name is confusing.

Nick:
* More confusing matrix of states?
* Partitioned cookies but unpartitioned local storage?
* Why would dev want that?
* Would that lead to problems, like privacy leaks or dev confusion?

Ari:
* If requesting local storage access, that doesn't override existing access.
* Handle gives access to unpartitioned local storage.
* Might be confusing to have unpartitioned cookies mixed in automatically when not expecting it/not needed.

Johann:
* Someone was using storage for consent signals but want to avoid sharing that client-side state with servers.

Brian:
* Potential for confusion but not exposing data when not explicitly requested is good practice.


### [Explainer: Extending Storage Access API (SAA) to Shared Workers](https://arichiv.github.io/saa-non-cookie-storage/shared-workers.html)

Ari:
* Original extension didn't add shared workers.
* The general request storage access API doesn't give you lax or strict cookies, only none.
* But shared workers potentially have access to lax/strict samesite cookies (ergo, an info leak, security concern).
* Part 1: Mod existing shared worker constructor.
* At shared worker init, pass option for samesite cookies (all or none).
* All gives access to everything in the current context, incl. lax/strict.
* Only makes sense in first-party context where a shared worker is to be available in both first and third party context from the same origin.
* Doesn't change semantics of shared workers in third party contexts where they don't use that handle.

Ben:
* Looks reasonable while protecting security on first read.

Johann:
* Are you hoping to get these adopted separately?

Ari:
* Separate explainers to propose and think through them independently.
* Adoption goal is to add to repo a formal specification that has monkey patches to existing storage access API spec, then working on getting all three things adopted in a single spec before the APIs exist ahead of an origin trial.

Johann:
* Seems key for the cookies extension.

Erik:
* Should coordinate with the editors to determine right place for documents.

Johann:
* Non-cookie storage not in privacy CG yet.

Ben:
* Supports adoption of non-cookie storage extensions to storage access API as a work item.

Ari:
* Wants a draft spec ready by end of February.

Nick:
* Add as issue on non-cookie extension/storage access API?
* Should the storage access API have been graduated a while ago?
* What's going into it before it graduates?

Erik:
* To be determined.

Johann:
* Was in WebKit and Firefox, when Chrome came along there were security and privacy reviews and a refactor.
* Looking into these issues now
* Happy to report back more frequently or adopting cookie layering (this refactoring project) later
* There are also some unresolved issues with Storage Access but hopefully nothing major

Erik:
* Need status update from editors on future call

Ari:
* If graduated by mid-Feb, would offer all three explainers together as a single spec, patching the storage access API spec.
* Do not want to block the larger storage access API work.


## Attendees
* Brian May (dstillery)
* Erik Anderson (Microsoft Edge)
* Nick Doty (CDT)
* Chris Wilson (Google)
* Matthew X. Economou (RDCT)
* Ari Chivukula (Google Chrome)
* Chris Fredrickson (Google Chrome)
* Sam LeDoux (Google Chrome)
* Paul Zuehlcke (Mozilla)
* Ben VanderSloot (Mozilla)
* Tim Huang (Mozilla)
* Ben Wiser (Google Android)
* Johann Hofmann (Google Chrome)
* George Fletcher (Capital One)
* Wendy Seltzer (Tucows)
