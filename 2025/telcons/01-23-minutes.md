# Privacy Community Group Teleconference, 2025-01-23
Chair: Tess  
Scribe: Ben

## Agenda
* [CHIPS](https://github.com/privacycg/CHIPS)  
  * [Localhost port numbers \#92](https://github.com/privacycg/CHIPS/issues/92)  
  * [Clear-Site-Data integration is incorrect \#93](https://github.com/privacycg/CHIPS/issues/93)  
* [Storage Access Headers](https://github.com/privacycg/storage-access-headers)  
  * [Network roundtrips doubled for API requests \#6](https://github.com/privacycg/storage-access-headers/issues/6)  
  * [Should \`retry\` use a different name/keyword? \#23](https://github.com/privacycg/storage-access-headers/issues/23)

## Notes:
* [CHIPS \#92](https://github.com/privacycg/CHIPS/issues/92)  
  * Aaron: Firefox and Chrome handle ports differently for cookie partitioning  
    * Current spec is Site \+ bit indicating that it was set as top level site  
    * Why not include port: sync with Storage. Localhost as an exception is a bad idea.   
    * Why do it: more partitioning is good for privacy. Port can meaningfully divide services. Origin Bound Cookies haven’t been launched yet  
    * Qs: Why does firefox do this? Feedback?  
  * Ben: Partitioning, port separates admin processes from non on Linux  
    * Is not different for localhost or Storages  
  * Tim: Historical background: We used the origin definition for partitioning originally  
  * Aram: Putting aside localhost. Match the consideration of subdomain.   
  * Johann: Agree with that on the open web, localhost has special considerations. Should we just special case localhost here? Clarify: Does partitioning now include subdomain  
  * Tim: No, just base domain  
  * Ben: Makes more sense not to special case localhost  
  * Tess: Clarify: Firefox’s behavior is the same on localhost and open web. Chrome wants to align on localhost.  
  * Johann: Prefer to drop port entirely, but don’t want to add it on open web. What does Safari have for Storage Partitioning?  
    * I think it is Origin\! That is painful  
  * Ben: Deployment may be hard, for either  
  * Johann: Ben, rearticulate your position.  
  * Ben: \*reiterates\* But we should unify behaviors though  
  * Brian: Sympathy for web developers  
  * Johann: Idk  
  * Aram: local development sharing between ports can just modify /etc/hosts. Let’s not special case it. Let’s just make sure it is well documented.   
  * Brian: Don’t disagree with you Aram\! Just the first time is painful  
* [CHIPS \#93](https://github.com/privacycg/CHIPS/issues/93)  
  * Aaron: Dylan and I are working to alter the spec to take this into account.  
  * Johann: Let’s just do that  
* [SAH \#6](https://github.com/privacycg/storage-access-headers/issues/6)  
  * Alex (Sandor): Explains SAH. SAP has a SPA that sends 5 sequential requests that use this feature. We propose the following to solve their concern and make the storage access activation “Sticky”  
    * PR: [https://github.com/privacycg/storage-access-headers/pull/22/files](https://github.com/privacycg/storage-access-headers/pull/22/files)  
    * The idea is to make these request “sticky” for specified subsequent subresources. This does not include wildcards  
  * Tess: Clarify the flow diagram.   
  * Chris: reuse-for is tied to the origin of the server providing this.  
  * Ben: Thank you for tying up my complaints about request overhead  
  * Tess: I’ll ping people internally (to WebKit)  
* [SAH \#23](https://github.com/privacycg/storage-access-headers/issues/23)  
  * Chris: Looking to close the loop from a standards-position thread.  
  * Ben: Was looking into whether this was a reload or retry. The name is fine and the spec clarified it\!
* AOB  
  * Johann: I’ll put an agenda+ on an issue to get Anne to look at something


## Attendees:
1. Benjamin VanderSloot (Mozilla)  
2. Brian May (unaffiliated)  
3. Tara Whalen (W3C)  
4. Chris Fredrickson (Google Chrome)  
5. Sandor «Alex» Major (Chrome)  
6. Tim Huang (Mozilla)  
7. Ari Chivukula (Google Chrome)  
8. Emma Zühlcke (Mozilla)  
9. Theresa O’Connor (Apple, TAG (for now), co-chair)  
10. Johann Hofmann (Google Chrome)  
11. Christine Runnegar (Privacy WG co-chair)  
12. Wendy Seltzer  
13. Aaron Selya (Google Chrome)  
14. Aram Zucker-Scharff (The Washington Post)  
15. Heather Flanagan (Spherical Cow Consulting)  
16. Chris Needham (BBC)  
17. Aaron Selya (Google chrome)
