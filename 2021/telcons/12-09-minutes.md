
# Privacy CG Teleconference
* Chair: Erik Anderson
* Scribe: Pete Snyder

# Agenda
* [Private Click Measurement](https://github.com/privacycg/private-click-measurement)
    * [Require noopener in links and window.open() #94](https://github.com/privacycg/private-click-measurement/issues/94)
    * [Supporting multiple conversions for one click, a.k.a. reporting windows #95](https://github.com/privacycg/private-click-measurement/issues/95)
* [Cookies Having Independent Partitioned State (CHIPS) proposal](https://github.com/privacycg/proposals/issues/30)
* Announcements
    * [First-Party Sets](https://github.com/privacycg/first-party-sets) editors update
    * January 13th meeting at APAC-friendly time-- 7 hours later (7pm EST/4pm PST)
* Any other business

## [Require noopener in links and window.open() #94](https://github.com/privacycg/private-click-measurement/issues/94)

JohnW: This is a request rooted in google’s private measurement API; they want to register the click through window.open.  Got me thinking about the “opener” relationship.  When you open a link, there is a link back to the window that “opened” you, and the opened page can talk back to the “opener” page. We want to change things so that there is no opener relationship if you’re using PCM. Should it be explicit or implicit? (i.e., using PCM implies noopener, or page author has to do the noopener annotation themselves or they lose PCM features).  John leans towards later / explicit. (corrected: John favors implicit)?

ErikA: Either way has pros and cons / foot shooting, and can cause confusion.

JohnW: Implicit (requiring annotation) seems better.

AramZ: Agree that noopener should be enforced, and implicit seems better since it’d make things more difficult for adopters (since authors would need to go back and add a bunch of annotations).

NickD: Noopener could break things though (e.g., pop ups that need to communicate back). Maybe better to have the non-explicit behavior be to disable PCM.

JohnW: Good points on both sides

 \
BrianM: Agree with Nick, number of people who’ve implemented PCM today is &lt;< people who might in the future.  Explicit markup might result in less errors.

AramZ: I can see both sides. I wonder how many folks are using PCM that would break with noopener right now. Are there measurements? \


JohnW: You == Me? We don’t have direct measurements, only # of bug reports and requests. We mostly hear from larger developers.  Its a real pickle. Will continue conversation in the issue.

 \
BrianM: long term stable state better than avoiding short term adjustment costs


## [Supporting multiple conversions for one click, a.k.a. reporting windows #95](https://github.com/privacycg/private-click-measurement/issues/95)

JohnW: Exciting stuff; we’re far along in the lifecycle of PCM. This issue is next hurdle. Takes inspiration for G’s Attribution API.  Should be two timelines for when sites can register clicks: short and long (as in ~7 days). Maybe we should add a third window: 0-2, 3-7 and 8-35 day-sized windows. Goal is to allow measurement of how useful advertising is for acquiring “valuable” customers. Use case is to distinguish between a “only want to try the trial” customer, vs a “i want to purchase the real deal” customer. We have two proposals to reduce bad behavior. First, reduce entropy in the “triggering” event; currently is 4 bits, why not 1 or 1.5 bits for the medium and long windows? Second deterrent: delay when the attribution event is sent, once the triggering even occurs.  This delaying is designed to make it difficult for the site to distinguish which users to fire the event on, which would allow for additional trackability (since whether the event is registered at all is a distinguishing feature).

BrianM: Makes sense to have less entropy for longer windows. Do we need to limit the size of the last window?  Could it be controlled by the caller? Also, theres a pattern of less information vs more context, or vise versa.

JoshuaS: If there is a shorter window, could it reduce the competitive advantage from the companies trying to do the advertising?

JohnW: Your question is around a shorter window, not a longer window? \


JoshuaS: Sorry I mean longer

JohnW: Why would that benefit certain actors?

JoshuaS: (scribe: I missed it, if someone can fill in I’d appreciate it)

JohnW: Please follow up on the issue, that might be a better way to address the concern

ValentinoV: Seems like the lower, shorter window gets more information (or at least benefits from having more information), vs the lower amount of information the further-out windows get. Maybe it’s not a big difference, 4 vs 1 (or 1.5) bits? Maybe other factors don’t matter that much (scribe: I might have missed some of this too)

JohnW: If a bad actor targets, only has, 3 customers, then they can tie attribution reports back to those customers; there are enough bits in the signal to identify that # of participants.  However, with longer windows, the site can bucket users into different windows as a way of distinguishing between users further (i.e., getting additional implicit bits to identify with). So we need to reduce the # of bits, as the number of windows, and the length of those windows, increases. 

ValentinoV: I understand the abuse potential, but it harms utility. I can’t label which campaign caused an event, no? \
 \
JohnW: Maybe misunderstanding.  On the attribution side, you still get 8 bits.  You only get, destination side, 1 (or 1.5) bits in the far out windows. More conversation welcome on the issue.

ErikT: Wouldn’t the source and destination side need to coordinate to carry out these attacks (or how else would the destination be able to understand the assigned buckets)

JohnW: Even w/o coordination, imagine if you will: site gets a new user, site starts tracking the “journey” of the user (including time line of things the new user does, relative to start date). Site might know “if the user is still here in 2 weeks, they’re a high value”, site might only use PCM for these “high value” users. By only applying PCM to some users, PCM implicitly is getting more bits to distinguish between users. \
 \
… Site could get 10m users, but doesn’t know where they came from (friends, organic, paid advertising).  If the site was measuring across the board, site would always call the PCM API. However, as the site learns more about the user, site can only call API for the &lt;< number of remaining users, meaning there are fewer users that need to be distinguished between, and so fewer bits go further.  \
 \
BrianM: Anything > 2 weeks in a browser sometimes gets lost because of other causes


## [Cookies Having Independent Partitioned State (CHIPS) proposal](https://github.com/privacycg/proposals/issues/30)

DylanC: (scribe: there are slides we can link to, im just going to note things that aren’t in the slides, and summarize slide contents)

… Topic is unpartitioned cookies

… explainer slides about how cookies work in browsers w/o DOM storage partitioning

… google wants to preserve some kinds of cookie uses

… sites can add a new attribute, and cookies can be annotated with both the top level site and the cookie setting site

… to reduce the additional mem/disk space required by partitioning, lets decrease limits on cookie amount and size

… Firefox partitions cookies in strict mode, Safari blocks them

… CHIPS proposal would make partitioning opt-in, since 1) 3p sites have the expectation of cookies today, and 2) creates a transition path to an all-partitioned world \
… CHIPS folks also propose that sites can tell browser that they want to use partitioning features though a Sec- header

… Could integrate with FPS as follows: when the user navigates to a site thats (in 1p context) a member of a FPS, the relevant top level partitioning site is the FPS owner (scribe: I think i got that right…?) \
… fin

EliG: What can be done to make CHIPS useable w/o additional network requests? Maybe there could be other ways of opting into partitioned state (JS API) that could prevent implementation headaches. Will it be applied to more state? We currently use localStorage and unpartitioned 3p state to transmit consent information across contexts.

DylanC: CHIPS are not HTTP only, the CHIPS functionality can also control cookies through JS. Chrome is planning on partitioning JS state.  Referenced Chrome storage partitioning write up on GitHub.

EliG: Is this roadmap about improving the use of CHIPS, or additional opt in mechanisms

Erik: Maybe this is more a Storage Access API question?

EliG: I am trying to understand the direction things are going in.  Currently proposed options seem hacky

DylanC: If sites are in the same FPS, they can use SameParty cookies too

EliG: My org isn’t in our customer’s FPS; we don’t want to cause additional network requests coordinating among the FPS.

DylanC: Issues welcome

LeeG: I like CHIPS; sometimes we’re the top level site, sometimes we’re embedded. I opened an issue but I lost track of it. We don’t own the cookies tagged to our domain; CDNs and other parties assign cookies to our storage area and we can’t always control the names / attributes of them. Now we’d need to conditionally name our cookies, which is more complicated than it seems. It's more difficult than just setting attributes.

DylanC: Here's why we thought the __Host- prefix was a good idea: Host prefix requirement is a convention and standardized. Sec attribute is important

LeeG: We like the Sec attribute.  The issue is just the naming requirements are difficult in practice

DylanC: Will follow up on issue

LeeG: 10 cookies (proposed) might not be enough, since other layers in the stack might exhaust that limit in ways we don’t control. More might be necessary in practice.

DylanC: 10 limit comes from HTTP Archive data, where &lt;= 10 cookies seemed to handle most most most cases.  Not a fixed limit though, Chrome could change

JohnW: Storage Access API isn’t the user opting into unpartitioned storage, its the developer opting into partitioned storage.  Otherwise user gets prompted. 

DylanC: We think a cookie attr is better because there are non-JS cases where cookies are important, where JS controlled prompting isn’t available.  Thanks for the clarification.

BrianM: Have you considered things other than FPS for cross-site cookie use. E.g., federated login

DylanC: Don’t use CHIPS for federated login; use the proposed federated login targeting APIs

ErikA: there might be some overlap in terms of using CHIPS to storing access tokens after using FedCM though

BrianM: would be good of having ways of registering benign cross site cookie use other than FPS

DylanC: Sites might want to allow other sites to share state thats not FPS (e.g., enterprise apps)

TanviV: i) If a site doesn’t opt into partitioning, are 3p cookies on the page blocked?  \
 \
DylanC: Thats the goal, in the future

TanviV: If thats the case, what about browsers that don’t support FPS? Would cause incompatible behavior right?

DylanC: Would fall back on top-level site partitioning.  Would result in different behavior between FPS and non FPS behaviors

TanviV: for a site developer, blocking 3p cookies instead of partitioning might keep things simple; for others they’d prefer partitioned storage.  With FPS, now there is a third state of partitioned within same first party, which some browsers will honor and others will not.  Added complexity and makes things more difficult for maintaining cross browser compatibility

KaustubhaG: I’ve been thinking on the same topic that Brian mentioned, we’ll follow up with more suggestions



## Announcements

ErikA: 1) HarneetS is a coeditor on FPS doc, 2) next F2F will be at a time more friendly for Asia-timezone members. 

# Attendees
1. Allan Spiegel
2. Aram Zucker-Scharff (The Washington Post)
3. Brian Lefler (Google Chrome)
4. Brian May (dstillery)
5. cfredric
6. Chris Needham (BBC)
7. Christian Biesinger (Google Chrome)
8. Christine Runnegar (PING co-chair)
9. Christy Harris (Future of Privacy Forum)
10. Chris Wood (Cloudflare)
11. dan sinclair (Google Chrome)
12. Daniel Laliberte
13. Don Marti (CafeMedia)
14. Dongoh Park (Google)
15. Dylan Cutler (Google Chrome)
16. Eli Grey (Transcend)
17. Eric Mwobobia(ARTICLE19) 
18. Eric Trouton
19. Ericka Wright
20. Erik Anderson (Microsoft)
21. Erik Taubeneck (Meta)
22. George Fletcher (Yahoo Inc)
23. Grant Nelson (TripleLift)
24. Harneet Sidhana
25. Jack Frankland
26. James Rosewell
27. Jason Nutter (Microsoft)
28. Jeff Mixter (OCLC)
29. Jeffrey Yasskin (Google Chrome)
30. Joel Odom (Salesforce)
31. Joey Salazar
32. John Wilander (Apple WebKit)
33. Joshua Ssengonzi(Article 19)
34. Kaustubha Govind (Google Chrome)
35. Kris Chapman (Salesforce)
36. Lee Graber (Tableau/Salesforce)
37. Mark Xue ( Apple )
38. Mike O’Neill {Baycloud)
39. Mike Taylor (Google)
40. Nick Doty (Center for Democracy & Technology)
41. Peter Snyder (Brave Software, PING Co-Chair)
42. Przemyslaw Iwanczak
43. Riley Wills (Ellucian Inc)
44. Robert Sanderson (Yale University)
45. Russell Stringham (Adobe)
46. Sam Goto (Google)
47. Sam Weiler (W3C/MIT)
48. Scott Yates (JournalList, of trust.txt, (also chair of Credibility WG))
49. Sean Harrison (Google)
50. Shawn Cohen
51. Shivan Sahib
52. Simeon Warner (Cornell)
53. Steve Englehardt (DuckDuckGo)
54. Tanvi Vyas (Mozilla)
55. Tara Whalen (Cloudflare)
56. Theodore Olsauskas-Warren (Google)
57. Theresa O’Connor (Apple)
58. Tim Cappalli
59. Tom Crane (Wellcome/Digirati)
60. Valentino Volonghi (NextRoll)
61. Wendell Baker (Yahoo)
62. Yi Gu (Google Chrome)
63. Jeff Mixter (OCLC)
64. Eric Trouton
