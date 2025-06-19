# 12 June 2025 Privacy CG teleconference
* Chair: Tess  
* Scribe: Jeffrey  

# Agenda
* [Storage Access API for non-cookie Storage](https://github.com/privacycg/saa-non-cookie-storage)  
  * [Declaring constructors as methods is strange \#31](https://github.com/privacycg/saa-non-cookie-storage/issues/31)  
* [Storage Access API](https://github.com/privacycg/storage-access)  
  * [How does storage access interact with Dedicated, Shared and Service Workers? \#157](https://github.com/privacycg/storage-access/issues/157)  
* Any other business

# Notes
[Storage Access API for non-cookie Storage](https://github.com/privacycg/saa-non-cookie-storage): [Declaring constructors as methods is strange \#31](https://github.com/privacycg/saa-non-cookie-storage/issues/31)

* Ari: For BroadcastChannel and SharedWorker, there's a way to create them off the handle. They were named after the constructor with an uppercase initial letter, which implies the wrong things. Suggesting to rename them createBroadcastChannel, etc.   
* Tess: Any controversy?  
* Anusha: Anne suggested maybe we should combine some of the storage constructors as well. So there's a larger overhaul that we wanted to discuss.  
* Johann: Can't find the discussion  
* Anusha: [Issue \#41](https://github.com/privacycg/saa-non-cookie-storage/issues/41): "storage access members have esoteric design"  
* Ari: Right now, there's a 1:1 mapping between the name and the input dictionary. createSharedWorker function matches the name in the dictionary. But why not name it the name of the thing instead of the function. But that's bikeshedding, and we want a reason. A bunch of these are included by default, and maybe you'd want to block just the synchronous ones. But don't want to pick around the edges of naming.  
* Johann: Having a sync discussion is a good idea, but Anne and Andrew Sutherland should be in the room. Will re-tag the controversial issue. Maybe we can ask them to join the next CG call.  
* Tess: Cool.  
* Ari: Unlike the constructor thing, the question of what to put in the dictionary is lower priority, but a discussion is fine.  
* Tess: We'll plan to have that discussion next call.

[Storage Access API](https://github.com/privacycg/storage-access): [How does storage access interact with Dedicated, Shared and Service Workers? \#157](https://github.com/privacycg/storage-access/issues/157)

* Johann: Also not sure if the right people are in this room. Broader question from 2 years ago. Lots of back-and-forth overall. Think Anne a year ago suggested (and lots of people support this, despite some discussion around details) that dedicated workers inherit storage access, but shared and service don't. Controversial whether dedicated workers inherit storage access. Shared and Storage would need a separate API, if anyone needs it. Haven't seen demand. Where we have seen demand is a way for dedicated workers to get storage access. Discussion about whether that makes sense. Chris Fredrickson tagged this as agenda+ to make the case that inheritance is a good thing. We've also been thinking about this in the context of storage access headers. Ben? Or others?  
* Ben: Inheriting seems useful for dedicated workers. Might even make sense for all workers. Working on the PR for doing some worker updates for cookie layering. Inheriting the context's properties makes sense. Firefox does this without issues. Seems reasonable, but should have Chris around to dive in.  
* Johann: I'll also queue a report about how cookie layering is going. We should align on how cookie layering interacts with storage access soon. There's also more to elaborate on in the future, around some possible new APIs.

Any other business

* Jeffrey: The TAG will be bringing something around cross-site communication to the Privacy WG. Wanted to give a heads-up here. Discussion in [https://github.com/w3ctag/meetings/blob/gh-pages/2025/telcons/06-02-minutes.md\#design-reviews906-extending-storage-access-api-saa-to-non-cookie-storage---zcorpan-torgo-lolaodelola-1](https://github.com/w3ctag/meetings/blob/gh-pages/2025/telcons/06-02-minutes.md#design-reviews906-extending-storage-access-api-saa-to-non-cookie-storage---zcorpan-torgo-lolaodelola-1) and [https://github.com/w3ctag/meetings/blob/gh-pages/2025/telcons/06-09-minutes.md\#design-reviews906-extending-storage-access-api-saa-to-non-cookie-storage---zcorpan-torgo-lolaodelola](https://github.com/w3ctag/meetings/blob/gh-pages/2025/telcons/06-09-minutes.md#design-reviews906-extending-storage-access-api-saa-to-non-cookie-storage---zcorpan-torgo-lolaodelola).


# Attendees (sign yourself in)

* Theresa O’Connor (Apple, chair)  
* Jeffrey Yasskin (Google Chrome)  
* Brian May (unaffiliated)  
* Ari Chivukula (Google Chrome)  
* Johann Hofmann (Google Chrome)  
* Achim Schlosser (Bertelsmann)  
* Chris Needham (BBC)  
* Ehsan Toreini (Samsung)  
* Sandor Major (Google Chrome)  
* Benjamin VanderSloot (Mozilla)  
* Emma Zuehlcke (Mozilla)  
* Anusha Muley (Google Chrome) 
