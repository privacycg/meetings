# 26 May 2022 Privacy CG Meeting

* Chair: Tess
* Scribe: Heather Flanagan (1st half), Pete Snyder (2nd half)

## Agenda

* [First-Party Sets](https://github.com/privacycg/first-party-sets)
* [Move FPS to different CG/WG](https://github.com/privacycg/first-party-sets/issues/88)
* [Top-Frame lifetime, partitioned storage for embedded frames](https://github.com/privacycg/proposals/issues/18)
* Any other business

## Attendees (sign yourself in): 

1. Allan Spiegel (Adobe)
2. Andrew Aikens (TripleLift)
3. Aram Zucker-Scharff (The Washington Post)
4. Benjamin VanderSloot (Mozilla)
5. Bill Densmore (ITEGA.org)
6. Brain Campbell (Ping Identity) 
7. Brian Lefler (Google Chrome)
8. Chris Wilson (Google)
9. Christian Biesinger (Google LLC)
10. Daniel LaLiberte
11. Dominic F
12. Don Marti (CafeMedia)
13. Dongoh Park (Google)
14. Dylan Cutler (Google Chrome)
15. Eli Grey (Transcend)
16. Ericka Wright (Intuit, but representing self)
17. Erik Anderson (Microsoft Edge, co-chair)
18. Eric Mwobobia (ARTICLE19)
19. Guillermo Movia
20. Harneet Sidhana (Microsoft Edge)
21. Heather Flanagan (Spherical Cow Consulting)
22. Heinz Baumann
23. Helen Cho (Google Chrome)
24. Jason Nutter (Microsoft)
25. Jeffrey Yasskin (Google Chrome)
26. Jeremey Ney
27. Johann Hofmann (Google Chrome)
28. Kaustubha Govind (Google Chrome)
29. Martin Thomson (Mozilla)
30. Michael Kleber (Google Chrome)
31. Nicolas Pena Moreno
32. Pete Snyder (Brave, PING co-chair)
33. Sam Goto (Google, Chrome)
34. Stefan Popoveniuc
35. Tanvi Vyas (Mozilla, co-chair)
36. Theresa O’Connor (Apple, co-chair)
37. Tim Cappalli (Microsoft Identity)
38. Vittorio Bertocci (Auth0|OKTA)
39. Yi Gu (Google Chrome)
40. Brian May (dstillery)
41. Daniel Gale (Simplifi)


## Move FPS to a different venue

* Tess: a moment to talk about incubation and what this and other groups re for. Incubation is a fairly recent innovation in the W3C standardization process. It’s something we do early, and we use it when we don’t know if an idea has legs. Not everything makes it out of incubation; not all ideas are best ideas. The PrivacyCG is not the first group set up to do incubation. The WICG led the way for incubation. The CSS working group incubates its own features, however. One of the things we wanted to do in the PrivacyCG was to have a more focused group; we’re trying to do privacy stuff, not everything. The goal is to focus the group's attention on ideas we think have a good shot at being standardized (but you never really know in incubation). One thing we wanted to avoid is work simply stalling. We should be able to say when we think it’s time to spin things down and send a project on its way to standardization.
* Pete: We’ve been discussing FPS for at least a year or so. Seems like PrivacyCG is not the right place to continue the discussion. It’s either a specific proposal for giving passive access to cross-site cookies (which seems to be the dominant use case, and several orgs think this is privacy harming) or it’s not a prescriptive proposal and is just an abstract way to describe relationship between entities and has nothing to do with privacy. In either case, PrivacyCG wouldn’t be the right forum. While I don’t have a suggestion for another home, finding it a new home isn’t the responsibility of this group.
* Kaustubha: In terms of objectively answering the possibilities, the intention of FPS was to deal with compatibility that most browsers are facing with 3pc deprecation. 3pc cookies have existed for so long, we have no idea how many ways they’re being used by now. Several browsers started with something like this, though Apple deprecated their idea, and Mozilla would like to move away from what they’re doing. Every browser is doing something to address this problem, even if temporary, and they are all different.  We want to deal with compatibility to keep non-tracking use cases functioning. When we looked at how we define tracking, we looked at the DNT specification in the W3C and started from those principles. So we want to break info flows across sites not part of the same organization. In the interim, there are security and migration issues, and will need architecting by web apps to move to a new world. We don’t want to hold tracking protections back as people figure out what to do. We need to converge on whether multi-domain websites should or should not exist.
* JohannH: responding to something Pete said, that majority of browsers are not interested in FPS as a means of accessing cookies. Yes, FPS has received negative feedback from browser vendors, but that’s different from whether the issues FPS is trying to solve are relevant to this group. And yes, the issues are very relevant. Firefox has been fighting these problems for years with various features and  heuristics, so we see these issues as very relevant. And there are other entities in this group trying to move forward with more privacy-preserving APIs and are concerned with the same issues. Can understand why Pete is not interested in it, but wouldn’t say this group as a whole is not interested.
* Aram: this issue is probably a request to stop development on FPS. Can’t imagine another group picking this up at this time. If we don’t find FPS suitable for a privacy-forward specification, maybe it makes sense as a group to write something up about that more specifically. Not as familiar with the other browser use cases. If we think that FPS as it is at this moment unsatisfactory for the requirements, can we find the pieces that satisfy our use cases, break them out, and resolve them another way? If not, we should just be clear we don’t want to work on this.
* Martin: Aram is right, this is a request to say “stop” and I kind of support that. Johan’s port of the existence of breakages is an important one. Privacy policies we’re trying to enact are going to be challenging to ship with the existing web because of how orgs have relied on cross-site info flows. But we’re seeing some very good work going on to establish purpose-limited APIs, and hopefully those will move beyond incubation quickly. The idea we might build something that enables a new kind of info flow without any user intervention is something Mozilla is opposed to; we’ve held that position publicly for nearly 2 years. Not concerned about the mechanisms, want to know what it makes possible and whether those possibilities are things we want to see on the web. And I don’t want to see the possibilities FPS allows on the web. Would like to continue the conversation on the breakage Johan mentioned, but the same-party cookies is a line too far. 
* Don: One of the most constructive parts of this proposal is the independent enforcement entity, which is some kind of future governance body capable of evaluating FPS and approving sets that are valid and able to reject sets that are not. Right now, it seems like there’s uncertainty about the level of resources or governance, or how that entity is going to be able to do all the many tasks assigned to it by this proposal. Would be more comfortable with FPS in general if we knew about how the IEE works, its level of resources, guarantee of independence, and the skills represented there.
* Kaustubha: purpose-built solutions is exactly what Google wants to do, but these are not drop in replacement for how cross-site cookies are used today. It will require that sites rearchitect their systems. Are we going to hold off tracking prevention until those changes are made, or are we going to provide some mechanism to bridge that gap. One concern is that we’re just enabling the problem to continue for longer than it should. Also, the entity list isn’t maintained by any browser vendor.
* Chris: the WICG will actually allow incubation for anything as long as there are multiple independent entities that are interested. On ad hoc solution to the removal of 3pc, as long as they are hidden entity lists, that’s not a full solution. When we’ve had hacking solutions in the past, they’ve either been ossified into a solution or pushed into a standard. We’re not convinced this is the right solution.
* Aram: in terms of drop-in solutions, it would be better to release a permanent working solution rather than a temporary solution that covers up the fact we don’t know how to fix the problem. If we build something temporary, people will just build it in and we’ll need another temporary solution to get past that in the future. The IEE would be useful to break off into its own thing; trying to make it part of FPS seems a bad fit. If we’re preserving FPS to have an IEE, let’s just have an IEE
* Pete: process question - what to do in a situation where ⅖ of the vendors in a conversations say “we don’t think so”. What is the process to deal with this? 
* Tess: John Wilander emailed the list to express Apple’s opinion on FPS. James Rosewell also emailed his thoughts on this question to the chair and that will be pasted below for the record. To answer the process question, in the life of this group, we’ve tried to focus call time on things that have more consensus than this does. But we haven’t removed a work item before. For something to become a work item, there has to be consensus between the champions of the proposal and the chairs that it’s worth pursuing. Removing work items doesn’t have that restriction; the chairs can remove items if they decide it’s the right thing to do (see the charter). In terms of next steps for FPS, the chairs need to think about this. 
*  
* James Rosewell's email: 

        _“In February 2022 Google agreed [1] with the UK Competition and Markets Authority (CMA) to align their browser development globally to GDPR. FPS does not align to GDPR. To assist Google in achieving alignment, 51Degrees proposed GDPR Validated Sets [2] which has the following benefits over FPS. 1) data sharing is aligned to data controller and processors and not domain names (GDPR has no concept of first and third) [3]; 2) existing decentralized authorities for verifying legal identity are used; and 3) consumer consent is aligned to common data use agreements rather than specific data controllers and processors. 51Degrees would prefer Google to incorporate these concepts from GDPR Validated Sets into their work. If Google have an alternative method of aligning to GDPR and the commitments with the CMA we would like to understand it. If Google do not withdraw FPS we support FPS remaining within Privacy CG at this time. We also observe that due to the timing of the meeting many voices from Europe will not be represented. Before settling the matter of moving FPS another meeting at a Europe friendly time should be conducted.”_


        _ _


        _Citations for the minutes._


        _[1] https://assets.publishing.service.gov.uk/media/62052c6a8fa8f510a204374a/100222_Appendix_1A_Google_s_final_commitments.pdf_


        _“Applicable Data Protection Legislation” means all applicable data protection and privacy legislation in force in the UK, including the Data Protection Act 2018, the UK General Data Protection Regulation (and regulations made thereunder) and the Privacy and Electronic Communications (EC Directive) Regulations 2003;_


        _[2] https://github.com/privacycg/first-party-sets/pull/86_


        _[3] https://ico.org.uk/media/about-the-ico/documents/2619797/cma-ico-public-statement-20210518.pdf_


        _Box B: what is the difference between first-party and third-party data? Data is sometimes categorised according to the relationship between the party collecting and processing it and the individual or circumstance it relates to: • First-party data: data that is collected by a business through direct interaction with an individual providing or generating the data. For example, data collected by an online retailer regarding purchases made by consumers on its site. • Third-party data: data collected by a business not in direct interaction with the individual providing or generating the data, for example, through business partners. Digital firms that do not have a direct relationship with users frequently rely on third-party data. The boundaries between first and third-party data according to the above definition are not always clear, particularly when large companies own a variety of businesses, some of which have a relationship with the user and some of which do not. Both first-party and third-party data as defined above can include personal and nonpersonal data. Whether information is personal data depends on whether it relates to an identified or identifiable individual. There is no explicit reference to the distinction between first-party and third-party data in data protection law. 9 The descriptions of ‘first party’ and ‘third party’ are also used (though with a different meaning) in the context of cookies and similar technologies,10 which collectively form the key means by which information (including personal data) is collected and disseminated in online advertising. A cookie is generally identified as being first-party if the domain of the cookie matches the domain of the page visited and as being third-party in instances where the domain of the cookie does not match the domain of the website. This is not a rigid distinction. Some functions typically delivered through third party cookies can be done via first party cookies, even if a third party’s code and associated service is still involved. The rules on the use of cookies and similar technologies are specified in Regulation 6 of the Privacy and Electronic Communications Regulations 2003 (as amended) (‘PECR’), and oversight of these rules is one of the ICO’s regulatory functions. PECR provides more specific rules than the UK GDPR in a number of areas such as cookie use. It is also important to note that PECR’s provisions in this area apply whether or not personal data is processed._

## Top-frame lifetime

* Pete: appreciate folks here who have spent the time to work on FPS and discuss it here..
* Pete: Braves current storage policy - new third party storage configuration - preserver third party state partitioned under first party, but cleared as soon as you move away from the site.  Seems to be web compatible for us.  It came out of a conversation with this group.  Chrome team suggested it. 
* Martin Thomson: p1: How confident is Brave that this policy is effective and captures 3p use cases. P2: does it improve privacy
* Pete: For the first, intentionally no.  It is our intent is to break certain information flows as a privacy feature for our users.  So this is by design.  There are some use cases… we will keep alive first party state for 30s after the last instance disappears so that we can preserve federated login scenarios where there is bouncing between first parties.  If others wanted to implement this as a global policy, they might be interested in these tweaks. About privacy: I think that it does improve privacy.  Accumulated fingerprinting surface, looking at people over time you can build up some state about fingerprints.  So this is defense in depth.  Different accounts for the first party, the third party might otherwise be able to link first-party accounts.  As a practical matter it might help.
* Aram: I've worked on use cases that would be supported by this concept of being able to share state between frames but not shared with the top frame. It's useful for advertising. It's done by non-privacy respecting mechanisms, but this seems to respect privacy better. Several useful outcomes that we'd use for direct-sold ads. 
* Ben: Agree there's some privacy gain here. First, swiss cheese model to prevent longitudinal tracking. The 3p storage under an individual site being used as 1p storage is something we've seen to some extent. Has a risk of breakage. There's a knob here in the degree of ephemeralness. How many holes should the swiss cheese have? How long does storage last? Some setting would probably work for us, depending on user experience.
* Eli: For Brave: 30s time limit, is this configurable? Have you considered the model using completed subsequent navigation, to help people with poor or high-latency internet. \
FPS: As described, FPS is negative for privacy, but a limited proposal with more restrictions could be useful to users without privacy negatives, but with UX negatives. Tiny quota, &lt;1kb. But ask users if they want the site to store this data. Makes data human-readable and explains what you're doing with it.
* (Pete Snyder answering above storage question not on mic)
    * Its configurable by Brave; we can push down changes if we need to (we haven’t) but we don't expose the knob to users
    * We did consider that, we went with #-seconds bc it was easier to implement and ship to solve a problem we had in the field, but i think this (# navs) could be a good idea too!
* Erik: Worry about developer ergonomics and embedding scenarios. In identity space, an be desirable to say that if the user's logged into an IDP, and I load a control, should be able to iframe the IDP which has its partitioned cookies and can pass back an auth token. Source control is embedded, and don't want to postMessage all the way up and down. If we can postMessage, how much privacy improvement is it? For sites that aren't aware, … but should be a way for sites to get storage for the same origin across embedding contexts. \
Another scenario: app developer would like to support embedded apps having multiple identities loaded concurrently. If we have a partitioning scheme that can optionally do that, could be a privacy or capability improvement. Let sites choose what partition their data ends up in.
* Martin: Lots of ideas there. Clarifying with Aram, I thought this was a retention limit where this is a reduction in capability ont he site's side. 
* Aram: Ability of iframe B embedded in window A, ability to speak with another iframe from B elsewhere in the page. When they disappear and reappear, they want to retain state. 
* Martin: That capability exists with or without Pete's change. I heard a claim that there's an advantage in that state disappearing at some point?
* Aram: I thought this was preserving some level of retaining the state even if the B iframes disappear briefly.
* Martin: As I understand this, once the top-level origin, there's a short timer, and then all the partitioned 3p state disappears. For privacy, you break 3p continuity. But once you have cooperation with the 1p that might not provide a material privacy benefit. But maybe with the current state of the web there's some real privacy benefit.
* Erik: Pete's initial comment defines a set of behaviors.
* Aram: In these cases, it's not necessarily useful to retain the state past page navigation. In my use cases, it's desirable to discard that state on navigation as long as it's retained within a single page .
* Erik: Interested in the opinion on ephemeral storage. Brave + Safari doing this. Chrome or Edge are not bought-in so aggressively? Mozilla?
* Pete: Whether folks find the idea of imposed or optional ephemerality useful. Could imagine a hook in requestStorageAccess() to opt into this. Given that fingerprinting attacks get more sophisticated over time, there's some practical benefit to scrubbing 3p state that's unlikely to be useful or user-serving. 

### Any other business

* Adopt CHiPS as a work item? - https://github.com/privacycg/proposals/issues/30
    * Tess: I think this is a fine idea. Martin, what do you think of incubating this here, or the HTTP WG? We seem to have sufficient expertise
    * Martin: “Incubation” is what makes it difficult; this group isn’t well suited to finalize something like this. HTTP WG does have the expertise of a “real” standards body. I think CHiPS belongs here for incubation and then could be moved to IETF. Might be a rubber stamping exercise but still best to pass it there after adopting it here
    * Dylan: not much to add, glad to hear there is support for PrivacyCG adopting as a work item.
    * Tess: Everything is temporary in the world of incubation
    * Kaustubha: Are other browsers interested in being editors?
    * Tess: Editing is A+ and good and ethical and moral
    * Martin: We should create an internet draft as part of this group
    * Tess: If folks are interested in editing, please reach out
    * (Martin: YOU COULD HAVE AN RFC TOO!)
