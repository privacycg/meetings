# Privacy CG Teleconference, 8 July 2021

* Chair: Theresa O’Connor
* Scribe: Andrew Knox

## Agenda

* [Private Click Measurement](https://github.com/privacycg/private-click-measurement)
    * [#57 - Why attribution reports cannot go to third-parties and to anything else than the registrable domain](https://github.com/privacycg/private-click-measurement/issues/57)
* [Proposal: Fenced Frames](https://github.com/privacycg/proposals/issues/25)

## Attendees (sign yourself in):	

1. Andrew Knox (Facebook)
2. Angela Ballard (CJ Affiliate)
3. Aram Zucker-Scharff (The Washington Post)
4. Arthur Edelstein (Mozilla; second half of meeting)
5. Ashkan Soltani (Self)
6. Arthur Edelstein (Mozilla)
7. Brad Rodriguez (Magnite)
8. Brandon Maslen (Microsoft)
9. Brendan Riordan-Butterworth (IAB Tech Lab / eyeo GmbH)
10. Brian Campbell (Ping)
11. Brian May (dstillery)
12. Chris Needham (BBC)
13. Christy Harris (Future of Privacy Forum)
14. Dan McArdle (Google Chrome)
15. Daniel Appelquist (Samsung / TAG)
16. Dominic Farolino (Google Chrome)
17. Eric Mwobobia (ARTICLE19) 
18. Eric Rescorla (Mozilla)
19. Erik Anderson (Microsoft)
20. Erik Taubeneck (Facebook)
21. Frederic Jacobs (Apple)
22. George Fletcher (Verizon Media)
23. Grant Nelson
24. Hitesh Lad (CJ Affiliate)
25. James Rosewell (51Degrees)
26. Jason Nutter (Microsoft)
27. Jeegar Shah (CJ Affiliate)
28. Joel Odom (Salesforce)
29. Johann Hofmann (Mozilla)
30. John Sabella (PubMatic)
31. John Wilander (Apple WebKit)
32. Jonasz Pamuła (RTB House)
33. Josh Karlin (Google Chrome)
34. Kate Cheney (Apple)
35. Kaustubha Govind (Google Chrome)
36. Kris Chapman (Salesforce)
37. Laszlo Gombos (Samsung)
38. Lionel Basdevant (Criteo)
39. Mallory Knodel (CDT)
40. Maud Nalpas (Google Chrome)
41. Michael Kleber (Google Chrome)
42. Nick Doty
43. Paul Bannister (CafeMedia)
44. Pedro Alvarado (Resonate)
45. Peter Lowe
46. Peter Saint-Andre (Mozilla)
47. Phil Eligio
48. Przemyslaw Iwanczak (RTB House)
49. Rick Byers (Google Chrome)
50. Robin Berjon (The New York Times)
51. Russ Hamilton (Google Chrome)
52. Russell Stringham (Adobe)
53. Sam Macbeth (DuckDuckGo)
54. Sean Harrison (Google)
55. Shigeki Ohtsu (Yahoo! JAPAN)
56. Steven Valdez (Google)
57. Theodore Olsauskas-Warren (Google)
58. Theresa O’Connor (Apple)
59. Valentino Volonghi (NextRoll)
60. Viraj Awati (Amazon)

## Private Click Measurement

### [#57 - Why attribution reports cannot go to third-parties and to anything else than the registrable domain](https://github.com/privacycg/private-click-measurement/issues/57)

Specific comment: [Why attribution reports cannot go to third-parties and to anything else than the registrable domain · Issue #57 · privacycg/private-click-measurement](https://github.com/privacycg/private-click-measurement/issues/57#issuecomment-860127313)

* John Wilander - long history on this issue, [referencing specific comment](https://github.com/privacycg/private-click-measurement/issues/57#issuecomment-860127313), but this may now be unblocked. Referenced at 2 points, at click time, or when sending attribution report. Can be 24 hours to 7 days apart. If the server can sent a personalized private key, they can track/personalize. Status quo: if they get the same key at both points, it’s probably not personalized and ok. This also applies to where the reports should be sent (if it is the same, probably not personalized).
* Eric Rescorla - how do you handle rollover of keys?
* John - open issue, we allow 2 keys for handover, not implemented yet (or you can hold back reports until the key is updated). Same issue exists for endpoints, but much harder to test -- you can’t just send the report to both places like you can for keys. Added IP address protection at WWDC, PCM uses it. Can be used in both requests to limit ability to correlate them -- opens up the new possibility. May allow a samesite wellknown location, and we won’t just do eTLD+1, we will do the real CNAME so you can delegate to 3rd party. It would be a “well known subdomain”. We allow the well known to respond with 2 endpoints to allow for rollover. 
* Aram Zucker-Scharff - this design assumes CNAME subdomain to third party vendor -- questionable design choice. Safer/more transparent to have the real name of the website receiving data. Requiring a CNAME system disadvantages small publishers for unclear benefit. Should not encourage this, “bad behavior by ad tech firms”.
* John - there is a tradeoff: how much the user understands where the agent is sending information. There is a legal risk about where it is sent, the redirect could be a “leak”. The other one is more transparent.
* Kris Chapman - I understand the user agent is saying “we protected you up to this point”, false sense of security, bad experience for users, doesn’t actually protect them. It should be right up front, my opinion is we shouldn’t use CNAME.
* Angela Ballard - what would prevent a publisher/brand from setting up a CNAME to a proxy? Could the agent even prevent this?
* John - IANAL, this is about data leaks or bad practices, “someone sent something 3 years ago, I don’t know what that is”, this makes the publisher more responsible because the agent definitely sent that to them
* Angela - we take an approach of “when we have data we do the right right”, but also clients need to do it right. Fits along the lines of how data privacy is being done anyway -- not as transparent, but doesn’t change much, as long as publishers and brands are following the regulations.
* John - CNAME solves rollover problem, you can change where it goes in the backend
* Aram - IANAL, this doesn’t make anything more or less responsible regardless of if it goes through a domain or not. CNAME unlikely matter in this context, it just passes through. Most publishers won’t set it up this way (maybe large publishers can do fancy things). People will just go through the motions and it won’t create more responsibility, just make it less clear what is happening. We have spent the last year educating people not to accept blind CNAME, you are letting an ad tech company do whatever they want in a way that allows them to represent the publisher. THis is already a big problem, the ad tech vendors do not have accountability. Publishers do not have a window into what happens. We are telling publishers the opposite of what you are intending, it makes them less responsible. This outcome is not intended, but it is likely to occur. Big publishers will have the expertise to do it right, but smaller pubs don’t have the time/people so they will just do it and nothing check on it or think about it. This would allow ad tech firms to act as if they were publishers (bad), and obscuring where data goes to the user (bad). We are not lawyers, but this doesn’t affect anyone’s actual responsibility. The CNAME doesn’t protect people, reinforces bad behavior.
* John - I share overall concerns, they are valid.
* James Rosewell - so much talk about lawyers - under european law, joint data controllers are responsible. If you don’t know what is happening, you have a problem, irrespective of how it is being done. Do we need to engage lawyers in these conversations? We have a responsibility to validate things that change how things work. In response to lawyers, need to be mindful of anti-competitive concerns. CMA are concerned about the definition of 1st and 3rd party as used to justify this and other proposals.
* John - if you can put any URL, you can put it to you own site and do CNAME. Is that enough capability?
* Aram - rolling over to a different reporting URL is easy.
* John - the problem is we would see different domains on the different requests, and drop the data
* Tess - you want a grace period, to make sure they match
* John - do we need an affordance similar to click fraud prevention
* Kris - from Salesforce POV we are always 3rd party. It’s better for consumers to know that Salesforce has the data so they can come directly to us, without having to go through a publisher or marketer. Consumers should know where their data is as close to the process as possible. This also assumes a single partner being used. In status quo the pixels are going to lots of companies, fraud, serving, etc. There are many parties that want this data, if we hide it behind a single endpoint it is problematic.
* John - might open a new issue with a proposal - currently leaning on allowing any url there, with a human readable element

## [Fenced Frames](https://github.com/privacycg/proposals/issues/25)

* [https://github.com/privacycg/proposals/issues/25](https://github.com/privacycg/proposals/issues/25)
* Josh Karlin - the goal is to prevent cross site tracking. We partition all the things, same as other browsers. When necessary, we poke the smallest holes possible. E.g. Ads, third party embeds, anti-abuse, antifraud. Some of those use cases call for data to be displayed cross site. Lift experiment, need ads data to go cross partition, buttons for “pay.com” show pay for logged in but sighup if not, also embedded docs. Many uses cases to support that require merging data from multiple pages, but we don’t want it to go into the same context -- it’s a vector for tracking. So we want to keep them separate. If they need to come together, limit frequency, similar to Storage Access API. The assumption is that embedder and embedded want to collude -- different than usual model. Both parties are adversarial, neither trusted. How do you do this but keep it isolated? IFrames can’t, because they allow opt in (e.g. POST, frame tree, other snooping). Fenced Frame says “can we treat the embedded frame as the top frame, but it just happens to be visually embedded”. Not in the same browser context group. The frame needs to know _something_ about the page it’s embedded on, maybe just top level domain, but don’t want to allow tracking. Want to keep it in it’s own partition. We don’t want to allow a.com to embed on every single page and track across the web. The fenced frame has its own frame tree and data partition. There is a continuum of restrictions we can place -- that’s the general pitch but we need to find the right place to land. This is useful for ads, because it allows embedding without too much leakage. There is some leakage, but we try to prevent it until the user clicks. When they click some information leaks, but only at that time. This is useful beyond ads, request storage access, isloggedin, can be safer from embedder, won’t allow an easy permanent join. Things to think about: restricting network within a fenced frame to mitigate side channel attacks (like a timing correlation attack) this type of attack made us think about network limiting at all. This might be beyond the browsers scope. Another option is we restrict access to two networks that agreed not to do bad things, scary policy to set up. Or maybe we have different modes, you can mix anything you want but never communicate anything out. Or only allow limited communication on click, and even then only to predeclared URL. Need to work through all of this, so far just sporadic use case exploration. Want to know if other browsers want to move forward with an idea like this.
* Jason Nutter - last time we can here with issues with storage access API and logout, when a user logs out of a given relying party, they are redirected to the IDP, the IDP clears cookies for that session, and then clears all other relying parties in a hidden iframe, this signals those parties that the user is ending the session. When third party cookies are blocked this is broken because it is inverted, it makes it hard to clear data and have a single logout experience across different machines etc. Fenced Frames could be a good use here, John had brought it up with storage access API, tracking issues there, Fenced Frame is a nice solution to that, loading relying parties into fenced frames and tell them that a user is ending a session. Would require storage access and network access.
* Josh - when we mix data from 2 partitions and then share it, there is a risk. The IDP 
* Jason - the oauth spec says you take the RPs, provide the session id, the RP knows the session ID, and figure out which authority the logout was for
* Josh - can you just send a fetch?
* Jason - no, we need to be able to clear client and server artifacts from RP, cookies, local storage indexdb, etc.
* Josh - is there a request header for clear site data you could use? Nevermind. I have to think about it, use case is documented.
* Tess (with TAG hat on) - had design reviews for subresource loading, sub iframe, portals, now fenced frames, another one, during one of those design reviews, we noted how confusing it is for authors to know why there are so many frames. For specification, there isn’t an effort to specify a common model that each of them is built on. When the fetch API happened, it wasn’t just modern XHR, it defined spec hooks where everything can specify everything specifically, in a common model. Have you been looking at other iframe-like thing and trying to create a common model. [+1 from Dan Appelquist]
* Josh - we do talk to portals team, the underlying infra would be shared, if we can find overlap etc that would be great. I’ll pitch people to write a spec. It’s a nice idea, what fetch did was good.
* Tess - [sign yourself in the doc]
* John - 5 things
    * 1 -  We brought up early the idea of the embedder setting up link decoration in the url for the FF to sneak in info about the user.
    * 2 - In terms of blocking networking, is IP protection enough?
    * 3 - for the owned partition, you might have sibling iframes with the fenced frame. Are all the fenced frames having their own partition, or can they share with iframes?
    * 4 - the spirit is there shouldn’t be communication between FF and other context, rules out Jason’s logout example
    * 5 - [issue 11](https://github.com/privacycg/storage-partitioning/issues/11) describes how clearing data could be used to track across websites
* Josh
    * 1 - a lot of use cases, the url . we don’t have enough data to join them because the browser controls it. If it is arbitrary/unpartitioned you have a mixed data problem. If you can give the same URL for the fenced frame regardless of credentials, you can have some confidence it is not a user param -- counterpoint: the embedders page could be user specific. You could have a .well-known file that announces which sites FF can link to. This is hard on developers because it has to be small. This is the hardest part of the URL leak problem. The size of the frame is also a leakage channel, we make  an enum so only a few bits get through.
    * 2 - it helps, you would have to do it on a raw timestamp. It’s a probability game, depends on how many users, if you pull apart many subdomains you migh defeat it
    * 3 - FF should not be able to talk to other frames. IF can talk to each other, FF can’t talk to anyone.
    * Didn’t quite understand Jason’s proposal
* Jason - the IDP knows all the RP the user is logged into, office teams etc, know session id for user, pass those 2 bits of info to the RP as strings
* Josh - Is IDP in FF or on it’s own?
* Jason - FF
* Josh-  In the url you need to pass 1p data
* Jason - both parties already know this
* Josh - and you are telling them to delete data they already have -- that scenario is tough
* Jason - this is an important scenario, it would be a privacy regression if you couldn’t do this. Really important in health care.
* Josh - you don’t have guarantees in this world, but it is obviously helpful
