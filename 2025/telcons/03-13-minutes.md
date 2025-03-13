# 13 March 2025 Privacy CG teleconference

* Chair: Erik  
* Scribe: Erik

# Agenda

* Review agenda+ items (dependent on sufficient quorum per topic)  
  * [Storage Access Headers](https://github.com/privacycg/storage-access-headers)  
    * [Network roundtrips doubled for API requests  \#6](https://github.com/privacycg/storage-access-headers/issues/6)  
    * [Missing worker integration \#26](https://github.com/privacycg/storage-access-headers/issues/26)  
  * [CHIPS](https://github.com/privacycg/CHIPS)  
    * [Clear-Site-Data integration is incorrect \#93](https://github.com/privacycg/CHIPS/issues/93)  
    * [Localhost port numbers \#92](https://github.com/privacycg/CHIPS/issues/92)  
  * [Navigation-based Tracking Mitigations](https://github.com/privacycg/nav-tracking-mitigations)  
    * [Moving bounce tracking mitigations to the fetch standard \#101](https://github.com/privacycg/nav-tracking-mitigations/issues/101)  
* Any other business

# Notes

[Network roundtrips doubled for API requests  \#6](https://github.com/privacycg/storage-access-headers/issues/6)  
* Discussed in the previous meeting.
 
[Missing worker integration \#26](https://github.com/privacycg/storage-access-headers/issues/26)

* Would like to have feedback from Anne, perhaps in a future meeting and/or in the issue.

[Clear-Site-Data integration is incorrect \#93](https://github.com/privacycg/CHIPS/issues/93)

* Aaron: haven’t gotten to updating the spec yet. If anyone has specific language they’re looking for on that particular addition, please bring it up here. If no issue, I’ll draft something hopefully within the next two weeks or so.

[Localhost port numbers \#92](https://github.com/privacycg/CHIPS/issues/92)

* Was discussed last time ([https://github.com/privacycg/meetings/blob/main/2025/telcons/01-23-minutes.md](https://github.com/privacycg/meetings/blob/main/2025/telcons/01-23-minutes.md)) a bit. Still open?  
* Aaron: Had some offline discussion, but still open.  
* Erik: Any specific browser vendors you are looking for more feedback from?  
* Ben: Mozilla wants to understand if partitioning by port is something that needs to be done, I recall Apple had implementation-specific concerns.  
* Johann: Did we find specific owner(s) to drive this forward? As we do CHIPS, vendors blocking 3p cookies, etc. this is going to be a bigger deal. Painful to resolve/align later, so would be good to resolve soon. Perhaps we should have a goal of resolving within the next couple of months.  
* Dylan: Does Firefox include port in the partitioning key?  
* Ben: it does, but it predates me. My understanding is that the choice of scheme/eTLD+1/port was a relaxation of origin. Port was included because separation of standard port and non-standard port seems valuable, e.g. local servers with different levels of permission. But I’m backfilling logic on a choice made before I was here.  
* Johann: I also wasn’t involved at the time. Will have to do some archaeology. Reached same conclusion last time; need to have someone drive it forward and talk to Ben/Anne/others to get to a compromise to present.  
* Ben: I think there are two questions: (1) is this a problem? and (2) where do we end up if we all end up in the same place? Probably “yes” and “ehhhh.”  
* Johann: Agree it’s a problem because it’s a fundamental difference and don’t want to leave unsolved. Dylan can help follow-up.  
* Dylan: Sure, sounds good.

[Moving bounce tracking mitigations to the fetch standard \#101](https://github.com/privacycg/nav-tracking-mitigations/issues/101)

* Erik: Not sure if things are specified enough, would love to hear opinions.  
* Andrew: I filed this not as an immediate thing, but as a medium-term thing, would it make sense to move to another location and where that would be. Wanted to solicit thoughts: is this a sensible thing to do and on what timeline? Not immediately.  
* Erik: two questions: what is the destination group for the work and what is the timing?  
* Andrew: more things to spec, what to do with 3p cookies enabled and disabled.  
* Erik: interop question: how much is shared in terms of shipping implementations?  
* Ben: Firefox implements the same thing. If we get the stateless thing solved as well, we have two implementers with implementations that are not to all users. That might block getting the monkey patches into Fetch. I do agree that the destination for the monkey patches is Fetch. Depending on Fetch editors’ appetite, the question is if the core algos go into Fetch or into Privacy WG.  
* Erik: Chairs will help chat with other groups/editors to help get clarity.  
* Ben: bounce tracking record depends on a new concept called an “extended navigation.” Genericizing out of the spec might be a useful thing as part of getting it into Fetch. That extended navigation is also used in storage access heuristics.

Any other business

* None.


# Attendees

* Erik Anderson (Microsoft Edge)  
* Ari Chivukula (Google Chrome)  
* Benjamin VanderSloot (Mozilla)  
* Emma Zuehlcke (Mozilla)  
* Chris Fredrickson (Google Chrome)  
* Andrew Liu (Google Chrome)  
* Don Marti (Raptive)  
* George Fletcher (Independent)  
* Aaron Selya (Google)  
* Johann Hofmann (Google Chrome)  
* Jeffrey Yasskin (Google Chrome)  
* Dylan Cutler (Google Chrome)  
* Nick Doty (CDT)  
