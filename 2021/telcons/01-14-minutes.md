# Privacy CG Teleconference, 14 January
*   Chair: Tess
*   Scribe: Kaustubha Govind

# Agenda
*   Private Click Measurement
    *   [#57 Why attribution reports cannot go to third-parties and to anything else than the registrable domain](https://github.com/privacycg/private-click-measurement/issues/57)
    *   [#58 Require both source and destination to be HTTPS / secure context](https://github.com/privacycg/private-click-measurement/issues/58)
    *   [#59 Well-known url endpoint naming](https://github.com/privacycg/private-click-measurement/issues/59)
*   The Storage Access API
    *   [Proposal for an ad hoc meeting](https://github.com/privacycg/meetings/issues/11)
*   Any other business
    *   Scheduling our next virtual F2F


# Private Click Measurement

## #57 Why attribution reports cannot go to third-parties and to anything else than the registrable domain

John: This started as a PING slack conversation right after a PCG call. As currently specified, the report goes to a first-party on the click, and we discussed that it should also go to a well-known location on the advertiser side. The 1p should state that it wants the report to go to a specific 3p, or the fact that the ad is specified by that 3p. I started writing an explanation about why these reports can’t go to 3ps to preserve privacy properties. We discussed a possibility for the browser to reach out to a .well-known asking for a single location to send the report to within 24-48hrs of the event occurring. The publisher and advertiser can specify the location. We may have to put this as an optional thing in the spec. We’ll need to explain to the user why their ad click info is going to a 3p. Mozilla is planning to present this information. (Invites call participants to comment on the issue)

Don Marti (CafeMedia): Possibility limiting the patterns to which conversions should be sent, so that the destination could not leak information about the user.

Johann (Gecko/Firefox): I made a couple of suggestions. For example, .well-known location and sent within a certain time period. Not sure if we need to limit to one end-point, perhaps there can be several. It wasn’t clearly decided on the thread. I’m making these suggestions on the assumption that this is beneficial to the ecosystem, but I’m not 100% sure. If you’re a publisher delegating ads to a 3p; or if you’re an adtech provider; please chime in if you think this is beneficial for your setup.

JohnW: With regards to multiple vs. one, we need to revisit the thread, but my recollection is that when it’s time to send the report, we need to pick …. The same report would go to all 3ps. Not a dynamic thing. Because if it’s based on the time of day, or what the user did, we may be opening this up for tracking abuse.

Kris (Salesforce): We are a vendor for a lot of marketers and publishers. I don’t think it should be a single endpoint. There is a cap today already (this data is shared by tracking pixels today) on how many pixels send the conversion. E.g. Google’s limit is five. Doesn’t have to be dynamic, but publishers do have a limited number of groups they are working with, so shouldn't be a problem for them to list them. We’ve been ingesting data from ad servers and we frequently have the brand itself send a report from their ad servers. Our experience is that it has been a nightmare for small marketers and pubs. They don’t have resources to debug with the vendors like google or facebook. Concerned that this could impact small pubs.

BenSavage (Facebook): Want to reply to Johann’s q. We run a 3rd party Ad-Network (Audience Network) for the 3p ecosystem. Not a big part of our business, but we think it’s important for the ecosystem. It’s not meaningful for advertisers to work with a large number of small publishers, so it’s hard for small publishers to attract advertiser spend. They need ad networks to get access to ad dollars. In terms of our ability to run this ad network, all we need is accurate reporting. This is important for value creation. I’m pretty flexible in terms of whatever solution we come up with. If we want to support small pubs, they need to work with ad networks to attract ad spend. Things that help: Accurate reporting. It’s hard to convince 10s of thousands of people to configure things. It would be more convenient if the report could be sent directly. Data could be anonymous. Perhaps 1p could forward information to 3p. As long as we can validate - e.g. blind signatures to validate that these are accurate private click reports - even forwarded reports are okay. The only risk is that they might omit reports, and not forward everything. Perhaps there is also some kind of cryptographic way to prevent this.

Johann (Firefox): I understand what you’re saying. If 99% of reports are going to the 3p, we might as well expose that in the browser. 

Don Marti: Want to echo the point that Kris and Ben made about small pubs’ limited capacity to configure/operate reporting endpoint on their own site. There are proposals floating around to formalize the ability for the 1p to [declare their relationships to a third party.](https://github.com/privacycg/proposals/issues/22)

tmeasurement api. I want to respond to something John said about ...3p… If 6+6 bits, the publisher can choose the # of bits to sent to the reporting endpoint. Want to echo what kris said that picking one endpoint causes lock-in and prevents innovation. In our design, we made sure that more than 1 party can receive the report at a time.

Brian May (Dstillery): I’m wondering if it’s possible to define the endpoint when the campaign is defined. In terms of the user’s perception of their data going to 3ps. At this point, users don’t understand what 3ps their data is going to anyway. Perhaps using the https icon to display information about what advertisers the pub works with, etc. …

AramZS (WaPo): Want to support this idea of a .well-known list of additional analytics providers / receivers/ Generally pubs would like to work with more than one system. Many of them are not technically capable of fwding their reports. Worry about lock-in, etc. This will only make these problems worse. We’ve replicated the problems we already have, but it’s in the back-end instead of the front-end. Having a set of pre-approved 2-5 providers and letting the pubs choose at runtime (based on performance impact, etc.) is in line with what exists today. If we can do this in a privacy-preserving way.

JohnW (Webkit): Forwarding data is what we talked about. The shopping site can fwd the data. Could happen as a redirect (although not fond of the idea) from the .well-known location. Could even happen dynamically on the server-side. Not sure we need to have additional json structure and figure things out on the browser side. I think the servers can figure it out. The second point is about the back-end … Sending data to multiple 3ps has come up before, ad tech don’t want data to leak among them. Sending data to all 3ps would inform all of them about a campaign that all of the companies would then learn about. Could we set up reporting the reporting endpoint when campaign is defined: We discussed this on the issue. This is a privacy concern because the 1p cookies are available and can cause the endpoint to be personalized. This is why it needs to be a static thing. 

Brian May: Should be defined statically when campaign is defined.

John: My understanding is that campaign are defined dynamically, but please open an issue if I’m misunderstanding.

JohnW: From browser perspective, we need to do what is best for users first. If businesses have to deal with data storing/sharing, it’s on them to disclose and align with user expectations.


## #58 Require both source and destination to be HTTPS / secure context

Johann: This came up in the JSON structure conversation. We thought there might be ways to simply the structure if https. In Firefox, we only ship APIs for secure contexts. My recommendation is to encode that in the spec, and require endpoints to be HTTPS. I’d like to hear if there are reasons for this to be infeasible.

John Delaney: i agree that requiring secure contexts is important. Instead of https, it may be better to use potentially trustworthy origin in order to allow devs to test using localhosts.

JohnW: The spec already requires the report to be sent over https. It just picks up the domain and picks https. We’ve found it difficult to force the adoption of TLS by gating features to secure contexts. We are supportive of TLS, but we have not really jumped on that train. This also falls into that bucket, but if others browsers want to do this, we’d be open to it. 

Johann: We don’t have to encode into spec, but Firefox would likely gate to secure contexts.

JohnD (Chrome): Chrome’s event-level conversion measurement api does require publisher and adtech endpoints to be https.

Tess: I’m curious if there are smaller pubs who have an opinion on whether this is going to be an undue burden.

Aram (WaPo): not a small pub, but pretty everyone we’ve talked to realizes that https is becoming a requirement. Probably not an undue burden.

PaulBannister (Cafemedia): represent ~3000 medium-small pubs. We already require pubs to use https. Can’t represent smallest pubs, but again they might not be set up for this API anyway. 

Wendell Baker: https isn’t going to be a problem. Business opportunity for service provider. The tech should allow for delegation.


## #59 Well-known url endpoint naming

JohnW: This is very technical. Hoping to make a decision. Where should these .well-known locations sit? Should we share w/ Google’s proposal Conversion Measurement API? Should we have separate ones for measurement, attribution, report?

Tess: With the overall goal of minimizing differences over time, to the extent that it’s possible, it might be a good idea for both specs to have a common endpoint. No opinion on naming.

JohnW: Might require the reporting endpoint to also name the feature (conversion measurement, or PCM) so the endpoint knows which API is reporting.

JohnDelaney: I agree with john’s point that we need some way to differentiate. In the JSON, there are already some differences between the APIs, but we could have a more explicit. I don’t have an opinion on shared vs. not, but shared is probably a good idea. Valuable to separate the conversion on the advertiser site from the report. Having a common name like “measurement” in the path sounds good to me. Will follow-up on issue.


## Ad Hoc Issue, will file on repo

Ben Savage: This is a problem that we call the “Etsy problem”. We know that a lot of things won’t change, but what things won’t change? There will continue to be small merchants. E.g. those who do not have a website, or a unique eTLD+1. A whole bunch of them use platforms like Etsy to sell their wares. What etsy has done is for their merchants to configure a different facebook pixel id. One issue with the PCM is that it implicitly assumes that a eTLD+1 maps to a single business. Today, we would get campaign id and etsy.com, but we wouldn’t be able to tell which business it came from. It seems like an undue burden to expect these merchants to register an eTLD+1. Would be great to find a solution to let them use platforms instead of having to register a website.

JohnW: Similar to the Public Suffix List. Not saying Etsy should go put themselves on the PSL, but interesting idea.


# The Storage Access API


## Proposal for an ad hoc meeting

Ad hoc meeting scheduled for: Jan 21, 2021, 10am Pacific/1pm Eastern

Anyone interested can attend.


# Any other business

## Scheduling our next virtual F2F

We’ve had two productive F2Fs so far. Chairs are leaning towards having two this year, one possibly in March. Will file issue in Meetings repo to discuss scheduling. Please chime in if you have concerns in terms of scheduling.

# Attendees:
1.  (Google)
2. Abhinav Sinha(PubMatic)
3. Allain Spiegel
4. Andrew Knox (Facebook)
5. Anne (he/him; Mozilla)
6. Aram Zucker-Scharff (The Washington Post)
7. Arthur Edelstein (Mozilla)
8. Ashkan Soltani (Null)
9. Ben Savage (Facebook)
10. Bill Densmore (ITEGA)
11. Brian May (dstillery)
12. Cezary Cerekwicki (Opera)
13. Chris Needham (BBC)
14. Chris Wilson (Google)
15. Christine Runnegar (PING co-chair) (part)
16. Christy Harris (Future of Privacy Forum)
17. Devin Rousso (Apple)
18. Don Marti (CafeMedia)
19. Eric Mwobobia (ARTICLE19) 
20. Ericka Wright
21. Erik Anderson (Microsoft)
22. Forrest Akin (CJ Affiliate)
23. Garrett McGrath
24. Harneet Sidhana
25. Heinz Baumann (Quantcast)
26. Hitesh Lad (CJ Affiliate) 
27. Jack Frankland
28. Jade Kessler
29. James Hartig
30. James Rosewell (51Degrees)
31. Jason Nutter (Microsoft)
32. Jeddy Chen
33. Jeffrey Yasskin
34. Joe Genereux
35. Joel Meyer
36. Joel Odom (Salesforce)
37. Joey Robert
38. Johann Hofmann (Mozilla)
39. John Delaney (Google)
40. John Wilander (Apple WebKit)
41. Kate Cheney (WebKit)
42. Kaustubha Govind (Google Chrome)
43. Kelda Anderson (Microsoft)
44. Kris Chapman (Salesforce)
45. Liz Hurley
46. Lola Odelola (Samsung Internet)
47. Marshall Vale (Google)
48. Marçal Serrate (Hybrid Theory)
49. Max Gendler (NYTimes)
50. Melanie Richards (Microsoft)
51. Michael Kleber (Google Chrome)
52. Mike O’Neill (Baycloud)
53. Paul Bannister (CafeMedia)
54. Peter Snyder (Brave)
55. PI
56. raj
57. Robert Stratton(neustar)
58. Russell Stringham (Adobe)
59. Ryan Avecilla (Neustar)
60. Sam Weiler (W3C/MIT)
61. Sean Harrison (Google)
62. Steven Englehardt (Mozilla)
63. Tanvi Vyas (Mozilla)
64. Theresa O’Connor (Apple)
65. Thomas
66. Tim Cappalli (Microsoft - Identity)
67. Tony Resendiz (CJ Affiliate)
68. Valentino Volonghi (NextRoll)
69. Viraj
70. Vittorio Bertocci (Auth0)
71. Wendell Baker (Verizon Media)
72. Wendy Seltzer
73. Josh Karlin (Google)
74. Henry Lau (Privolta)
