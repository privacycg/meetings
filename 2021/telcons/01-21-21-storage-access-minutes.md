# Privacy CG Ad hoc on Storage Access API, 21 January
* Host: Johann Hofmann
* Chair: Tanvi Vyas
* Scribe: Wendell Baker

# Agenda:
* [40 mins] Work through interop Issues.
* privacycg/storage-access#25

   * Firefox is planning to align with Safari behavior, any concerns from Edge?
   * privacycg/storage-access#36

      * Browsers seem to align on always rejecting without user gesture in cross-origin iframes, but differ in top-level context behavior (Safari resolves, Edge rejects).
      * https://github.com/privacycg/storage-access/issues/5 

         * There's a new suggestion for a resolution from Firefox folks, would be great to get feedback on this.
         * privacycg/storage-access#4

            * This probably needs a few words in the spec but might otherwise be punted to storage-partitioning.
            * privacycg/storage-access#2

               * Firefox and Edge align on this as well as Safari Technology Preview, can we resolve this issue?
               * [20 mins] Standardization.
               * Are the spec editors (John and Tess) available to start resolving (inline) spec issues? What do they need help with?
               * Can we identify issues that are blockers for standardization and assign someone to move them forward? Potential blockers: inline spec issues
               * privacycg/storage-access#40
               * privacycg/storage-access#50
               * privacycg/storage-access#31
               * privacycg/storage-access#12 and privacycg/storage-access#10 (from Edge folks)
               * privacycg/storage-access#37
               * Where do we want to move the spec as it matures?

# Overview
* [Johann Hofmann]] Why we’re having this meeting:  Firefox is an implementor of the Storage Access API. Firefox would like to move the spec out of PRIVACYCG.  It is not clear where to start to get the ball rolling to move forward with (getting the spec out of the CG). Here are the agenda items that are relevant. (shown below)  We’ll spend the time to work on the interop(erability) issues. The meeting will go over the issues one by one; we don’t have to complete discussion on any one, but a breadth-first approach will be followed.  The second part of the meeting will be about resourcing towards developing the standard.
* [John Wilander] About six months ago, the Google team said they wanted something else.  Is there any other update on their concepts?
* [Johann] (implicitly, no update).  Is someone from Google available?
* [Theodore O-W.] No update. No one from that team here.
* [John W] asks that the agenda for the next CG meeting contain a topic for an update from Google on this (whatever their alternative proposal was).  Followup

# Work through interop Issues.
* privacycg/storage-access#25 - Firefox is planning to align with Safari behavior, any concerns from Edge?
   * [Johann H] Issue #1 (below).   The Storage Access API (SAA) would remove the UX interlock around the activity on the page.  There were some concerns that this was insufficient.  Safari and Firefox differ here. 
   * [John W] Some history with WebKit.  WebKit is asynchronous.  There must be a jump from the network context to the content context to process the UX.  So that consumes the user gesture in all cases.  However, developers had some questions about the flow.  Having been granted cookies the flow frequently moves to login the user elsewhere.  The contradiction comes in ensuring that the user gesture is preserved across the separate micro-tasks so that the end user is not prompted twice.
   * [Brandon Maslen] Overall there should be consistency in how UX events are consumed. This seems fine though.
   * [Johann H] There were some concerns, but it seems possible to get alignment here.
   * [John W[ It would be nice to resolve the issues with a decision so they don’t remain open or wind up with differing decisions.  If there is a link to a bug (ticket) for Edge.  
   * [Brandon M] there will be a PR that clears the issue in Edge.
   * [Anne van Kesteren] If [the UA] look at consuming the gesture and then unconsume it when permission is granted.  It is unclear if all User Agents do this the same way.  There is a flag on the document which says that there is user interaction. This is a promise.  There is a potential for race conditions on denying and allowing in a promise scenario.  The view is that the new model in the DOM will handle this.
   * [Johann H] This should be captured in the ticket, specifying how the promise is resolved to avoid these race conditions with the UX task. Followup.

* privacycg/storage-access#36 - Browsers seem to align on always rejecting without user gesture in cross-origin iframes, but differ in top-level context behavior (Safari resolves, Edge rejects).
   * [Johann H] moving on to the next issue.  Firefox will resolve the promise if there is user interaction.  The question resolves around the sense of always in the specification of whether user interaction is required.  Firefox will always require a user gesture. Edge differs.  Edge rejects without a user gesture in a 1st party context. Safari has a different policy differing between 1st party and 3rd party.  There are two cases 1st Party & 3rd Party.  The specification should be clear.  Also fact-check the behavior of Edge, Safari and Firefox for the scenario.
   * [John W] Verifying the Safari code in real time, the code checks for the user gesture.  The description of Safari is correct.  The policy can be changed depending on the specification around always and never.
   * [Johann H] This can be resolved (documented) in the issue.
   * [John W] Some rebasing of the test suite will be necessary to change this.
   * [Johann H] fine.
   * [John W] The Safari team has been thinking about giving a reason (an explanation) for any rejection that occurs in the UX.
   * [Johann H] There was some discussion of that in an issue somewhere.  The reason for not giving a reason was that this could be more confusing that not giving any reason at all.
   * [John W] There are two sides here.  Telling the requesting site that a rejection occurred and a different side is telling the receiving user why the rejection occurred.
   * [Johann H] The error scenario needs more documentation.

* privacycg/storage-access#5 There's a new suggestion for a resolution from Firefox folks, would be great to get feedback on this.
   * [Johann  H] Moving to Issue #5, which wasn’t in the agenda.
   * [Johann H] Firefox has a concept of storage going away after a period of time. The issue is not clear enough on what data is being deleted.  There is a view that it is not desirable to specify the time of expiry.  There are two suggestions here. [something]  The second suggestion is count user interaction across iframe boundaries towards the freshening of the expiry time bound.  Because it is hard to figure out precisely when the last time a user interaction occurred, there will potentially be a large number of calls to the API.
   * [John W] This is an old issue and is inconclusive.  Safari has an eighteen year old policy.  The site must be visited in 1st Party context first.  The state machine starts with the 1st Party so that the user can perform recognition and then the grant of storage access occurs after that.  The new policy in Safari is with per-page storage access.  So granted storage access occurs temporally near the user interaction.  There are long-lived web pages such as mail, PWA or other applications so there might be a long time between user interaction and the test.  Are there other scenarios?  Al siblings and subdomains of a page are entailed here in the interaction computation.
   * [Johann H] So the grant request is for a single domain (e.g. a.com)  The Firefox development team was concerned about the number of calls to the access API over long spans of time.  The Firefox view is that persistent 3rd parties are a small fraction of the population.
   * [John W] The WebKit maintains a counter to track the last time storage access was granted.  In the Firefox this would happen frequently with a Calendar widget or similar sorts of long-running sub-pages.  
   * [John W] So there needs to be some sort of counter-based rate limiting of the storage access requests into the API.  
   * [Johann H] Firefox has something like this.
   * [John W] There can be a specification of the counter and rate limiting that needs to be performed to manage the UX.
   * [Rob van Eijk Background Zoom Comment ] Acuityscheduling.com is a service that people can implement as an iframe. persistency would be welcome.
   * [Steven Englehardt] This issue is important with long-running pages (related to issue 2).  Firefox testing showed this was frequently the case. But Safari does not have this behavior.  [John W wil answer later]
   * [Brandon M] With new grants.  If there are unrelated interactions there is a possibility to inadvertently extend the access grant.  What about ensuring that there are no inadvertent gaps in storage access?
   * [Johann H] yes.  The idea is that the Storage Access API can be called frequently but this does not become a blockage for the UX or for performance.  That’s the theory.
   * [Rob van Eijk] In an example with a calendar service as an iframe, there is a need for consistency in the storage access.  These widgets are visited sporadically and the storage access needs to be maintained over long periods of time, e.g. 6 weeks or 3 months..
   * [Johann H] There might be a re-prompt, but some work can handle the consitency.
   * [John W] responding from earlier .  Yes, there may be a bug or mis-implementation of the specification.  There can be an interaction with ITP where the granted storage access is deleted when ITP deletes state.  The goal is to have per-page storage access.   But the Storage Access is per site. The access to actual state CRUD is per page, that is where the confusion lies.
   * [Johann H] There is a separate counter that manages the site-level and the page level.  So the prompting does not occur again when the same site is contacted on a different page.  So there is a two-level query here with getting a grant for the site and getting a grant for the state CRUD on a page.

* privacycg/storage-access#4 - This probably needs a few words in the spec but might otherwise be punted to storage-partitioning.
* SKIP for now.  Please mark item agenda+ if you want it scheduled in a future teleconference.
* privacycg/storage-access#2 - Firefox and Edge align on this as well as Safari Technology Preview, can we resolve this issue?
* SKIP for now - some discussion about this covered in issue 5 discussion.  Please mark item agenda+ if you want it scheduled in a future teleconference.


# Standardization
* Are the spec editors (John and Tess) available to start resolving (inline) spec issues? What do they need help with?
   * [Johann H] Question to JohnW and Tess O’C.  Are you ready to begin working on this?
   * [Tess O’Connor] Yes. We are enthusiastic and will manage the time appropriately.  It needs to be in a PR on github.  The preferred mode for editing specifications is to have editors from different organizations.
   * [John W] The co-editorship should be shared, perhaps with someone at Mozilla.
   * [Johann H] Will follow up after the meeting.
   * [Tess O’C] What venue should carry this work where the specification is developed?
   * [Johann H] Maybe an HTML WG.
   * [Tess O’C] Agreed.
   * [John W] Similar, with the history being that the work came from an HTML WG. WHATWG HTML

* Can we identify issues that are blockers for standardization and assign someone to move them forward? Potential blockers:
   * [Johan H] Invitation to the Edge delegation to relate the nested iframe condition.  Is there an overview we could get?
   * [Brandon M] The 1st child can call the API.  The 2nd and below can call Storage Access API but always gets a reject.  WebKit differs.  There should be a dynamic enablement/disablement of the Storage Access API in iframe hierarchies.
   * [John W] the iframe is sandboxed so the iframe must have a policy token to allow it to call the API. Cross-Site Iframes or Nested Cross-Site Iframes might differ in how the Permissions API would allow the Storage Access API to be called.  This could change.
   * [Johann H] Firefox has Permissions Policy implemented so that can be summarized in the issue.
   * [John W] document how the Permissions Poilcy relates to Storage Access API behaviors.


# Attendees (sign yourself in):
   1. Anne van Kesteren (he/him; Mozilla)
   2. Brandon Maslen (Microsoft)
   3. Brendan Butteworth
   4. Brian May
   5. Christine Runnegar (PING co-chair)
   6. Don Marti (CafeMedia)
   7. Erik Anderson (Microsoft)
   8. Erhard
   9. Harshad Mane
   10. Jack Frankland
   11. James Hartig (Admiral)
   12. Jason Nutter (Microsoft)
   13. Johann Hofmann (Mozilla)
   14. John Wilander (Apple WebKit)
   15. Jonathan Kingston (DuckDuckGo)
   16. Kelda Anderson (Microsoft)
   17. Lola Odelola (Samsung Internet)
   18. Mark
   19. Melanie Richards (Microsoft)
   20. Mike O’Neill
   21. Paul Zühlcke (Mozilla)
   22. Rob van Eijk
   23. Rotem Dar
   24. Sam Weiler (W3C)
   25. Sean Harrison (Google)
   26. Steven Englehardt (Mozilla)
   27. Tanvi Vyas (Mozilla, Co-Chair)
   28. Theodore Olsauskas-Warren (Google)
   29. Theresa O’Connor (Apple)
   30. Tim Huang (Mozilla)
   31. Vittorio Bertocci (Auth0)
   32. Wendell Baker (Verizon Media)
   33. Wendy Seltzer (W3C)
