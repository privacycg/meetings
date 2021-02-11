Privacy CG Ad hoc on Storage Access API, 11 February
* Chair: Tanvi Vyas
* Scribe: Michael Kleber





Private Click Measurement - John Wilander

Overview of [Apple's Private Click Measurement Announcement](https://webkit.org/blog/11529/introducing-private-click-measurement-pcm/)
* John Wilander:
   * We published a blog post a week ago, about Private Click Measurement being on by default in Safari in iOS and Mac Safari.
   * Now has 8-bits on click side and 4-bits on conversion side, based on conversions here and in GitHub
   * Stress that this is a proposed standard, not a standard.  We want this to become a standard, looking for another independent implementation.
   * First we discuss Web-to-web clicks.
   * Per TAG feedback, we've tried to harmonize with the Google conversion measurement API proposal, and also that we should pick terminology that isn't specifically about ads — can be used to measure clicks not related to ads also.  So "attributionsourceid" and "attributeon", instead of talking about "ad click" and "conversion".
   * Report format has changed to a JSON object, with names jointly decided with Google folks, again without ad reference — e.g. "attribution_trigger_data" rather than "conversion value".
   * We've also added app-to-web click measurement: Click inside an app that triggers navigation to a web browser can be measured also
   * "Future Enhancements" section is a bunch of links to the open issues in the GitHub repo:
      * Fraud prevention using blinded signatures, #27
      * Modern JS API (#31) where we've heard feedback that this may be in conflict with lots of sites wanting pixel-based rather than 3p-JS-on-their-site implementation — maybe there will be a same-site tracking pixel approach that lets the API support measurement without running JS
      * Attribution reports going to advertisers also, #53, as we've talked about here previously.  The data in the report should make sense to both parties (not a runtime-generated ID that's opaque to one of the parties), this is important if multiple people get reports for double-checking others' work
      * Allowing in nested iframes
* Brian May: Q about the section that talks about "Misuse" — "If PCM is being misused for tracking purposes or being used in conjunction with unrelated means of tracking users, events, or devices, we may block the offending party from using PCM and potential future measurement features."  Is this incompatible with UID2 from Trade Desk?
   * Wilander: We don't comment on specific other companies
   * Brian: How about deterministic IDs in general?
   * Wilander: This is for click measurement.  It is not supposed to be used with tracking — if that's what's going on, we don't need PCM at all.  We're doing the work to prevent tracking; if we learn that people are using this as extra on top of their tracking, we may tell them they can't use this — this is meant to preserve user privacy.
* Andrew Knox: Happy to see the Fraud Prevention attention.  If click-stuffing is possible, the ecosystem could have a really bad time.  I'd like to encourage you to release the fraud prevention mechanism as early as possible, so that the broader crypto and security community have a chance to weigh in on it.  We'd like a lot of eyes on it to be sure it's robust
   * Wilander: I could believe we'll iterate on it over time, start with a version that addresses 80% of the use cases, then iterate.  Conversations about tying the token to the data is interesting too, we're paying attention.  We will try to iterate.  In the "On by default" section, we talk about why we're turning this on before the fraud prevention is available — it's because all the other parts are stable, and we want to allow the industry to try it out, file bugs, etc. as soon as possible.
   * Andrew: Totally agree with the reasons for releasing without fraud prevention first, but fraud will change when other things become unavailable
   * Wilander: This is purely additive, so I don't think this is the moment when fraudsters will jump on PCM.  But we understand this is a risk.
* Paul Bannister (Cafe Media): Thank you for this.  You've talked about view-through.  Open-web display ads rarely perform well for click-through; there is a big need for view-through.
   * Wilander: Ben Savage (Facebook) brought something up at Web-Adv BG that is the first thing I've heard about view-through that seems plausible to me without creating a huge tracking vector.  Will talk about it later in this meeting.
* Kris Chapman: Regarding whether you are tracking — What about non-ads-related use cases, like realtime site personalization (e.g. want help with a specific product)?  When there is a use case where delaying the measurement would be a problem, are you interested in developing something in PCM to support that use case?  Or are we stuck with information in click URL to trigger real-time availability of site personalization?
   * Wilander: User clicking on something that could be an ad; context in which they're clicking could affect how the destination site renders, to personalize that experience, so if the destination site knows something about the source then it could affect what the site chooses to show.  We have not thought about that at all.  There are a lot of things that happen at rendering time.  Ben Savage has talked about optimizing ad clicks, similarly.  PCM is all about the reporting phase, later on, not about in-the-moment changes to rendering.
   * Kris: Makes sense.  The clicks that happen might not be going to the landing page.  The fact that a click happened might be more like a view-through — a site might want to be personalized later based on the fact that a particular click happened, but not 24-48hrs later.  I'll file an issue as well, but it's not advertising based, and we'd like to figure out site personalization in these use cases.
   * Wilander: It does tie into the whole term "personalization" — means you know something about the person, or technically the device, that informs how you render your site.  I think this would constitute cross-site tracking — knowing about what the user did on another site
   * Kris: Yes, it is, but it's not nefarious cross-site tracking.  Cross-site doesn't mean two different companies also.  We're trying to show you the thing that you're interested in — not consumer specific.
   * Wilander: I do see what you mean.  One is obviously related to First Party Sets, because if it's really the same party and just domains differ, then at least Google has a proposal, you know about that and all the issues raised around it.  The other issue is "when does something become personalized?"  Maybe there is a discussion to be had about what we mean by these terms.  Please file the issue
   * Kris: Would love to have a discussion about levels of personalization, will file issue.

PCM [The "Etsy Issue" #60](https://github.com/privacycg/private-click-measurement/issues/60)
* Andrew Knox (FB, in Ben's absence):
   * Some websites act as multiple businesses — etsy, indiegogo, kickstarter, salesforce as Kris just mentioned.  This is the opposite of the First Party Set problem: one domain acting as many businesses.  Web standards like looking at domains, but that doesn't reflect some realities.
   * Like the FPS question, this is something that you could completely work around by changing your DNS registration strategy — etsy could have a domain for each sub-business that runs on their top-level domain.  So it's kind of like the Public Suffix List, which is what lets browsers know that .co.uk isn't the same level as .co.com would be.  We could let etsy specify that they are a public suffix, but that would be pretty heavy-handed.  Related to questions of how users understand what domains they are on / what the rules are.
* Wilander: We've been discussing in the GitHub issue, conversation about potential pitfalls.  There are risks of bucketizing users, need to resolve.  Definitely worthwhile, it is a popular business model both for sites and their users.  If we can figure out a way that doesn't create a tracking vector, I'm all for it.
* Andrew: This is about the bit-limiting property — if bit limits are your main form of defense, that is problematic.  Some level of noise can help mitigate this.  Doing bit limiting is difficult, as this example shows.  I think this is part of what has motivated the interest in a Differential Privacy or noise-based responses.  If you can move away from bit limiting, this isn't a threat in the same way.

PCM [Ads / No-ads measurement #64](https://github.com/privacycg/private-click-measurement/issues/64)
* Andrew: (Sorry I'm not quite prepared for this one) — The fundamental thing about this is measuring things that are not ads and certainly are not clicks.  Example: Smart TVs (there is no click), experiments (where sometimes you see no ad at all, on purpose).  In the experiment case, it's very unlikely that there are many experiments at the same time going to the same domain.  Ben was proposing much stricter bit limits, because (unlike regular ad campaigns, where there are a lot) there are only 1 or 2 randomizations here.  So this proposal has extreme bit limits.
* Ben: Sorry I'm late, I'm here now.
* Wilander: Thank you for filing.  This is the first case where I see something with view-through that might actually work, that's very exciting.  Very limited because it's 1 bit, but it's something, it's view-through.  Of course we need to be sure it can't become a tracking vector.
* Ben: When one advertiser is running a whole variety of ad campaigns on Facebook, we apply that since ads/no-ads choice to all of the single advertiser's ad campaigns — this is how we can get to statistical significance.  In practice, measuring more than say 4 groups is basically useless, so we only need very low entropy.
* Wilander: Comment from the IWABG call: I misunderstood this at first, so to be clear: This is about explicitly deciding to not show ads to some people, and yes show ads to other people.  So in the browser there are three states: (1) I deliberately chose not to show this person an ad, (2) I did show them an ad, (3) no info.  This is what makes it very clearly different from just "send the report if I showed this person an ad."



Update on First Party Sets and SameParty cookie attribute in Chrome - Kaustubha Govind
* Chrome is planning to run a limited capability and duration Origin Trial between Chrome 89-91. Information on what “Origin Trial” means: https://web.dev/origin-trials/
* Chromium.org page with information: https://www.chromium.org/updates/first-party-sets
* Scope of implementation for the experiment: https://docs.google.com/document/d/1Lbvn3Wt664AhWA-UytjGEi7UcRMhrR4trUWEi2ieUkE/
* Intent-to-experiment: https://groups.google.com/u/1/a/chromium.org/g/blink-dev/c/XkWbQKrBzMg
* Kaustubha Govind:
   * We are running a limited Origin Trial on First Party Sets, starting in March running for about 18 weeks.  I have a link above to what that means and how to join and try it out.  Code is in M89, code in beta now.  You can also test things locally, via a flag, if there's a set of domains and you want to try it out with your own sets of domains in your own machine.
   * In the explainer, we discussed a way that sites could assert their FPS-ness via a .well-known url, but we don't have that now.  So far, we're shipping a static list, kind of like entities.json.  Submit a bug into Chromium's bug tracker to get your set of domains listed.
   * We're looking for feedback, that's what an Origin Trial is for
   * We have a policy for sets of domains now : limited to 5 registerable domains, for now.  This is intended to start simple.
   * Origin Trial only lasts for probably three chrome stable releases = 18 weeks.  Might extend, but this is just an early trial.
   * Single domain can only be included in a single FPS.
   * Policy also includes domains needing shared ownership and shared control.  (The disconnect.me list reflects that too.)
   * Policy: We're also asking for the location of a common privacy policy.  Is that a good part of the FPS requirements?  If there's some reason single-privacy-policy doesn't work for you, please let us know.
   * This feature isn't giving any additional access to third-party cookies — no extra access or privileges during the Origin Trial.  This is just a trial run.  Also no UI in place yet, and that will be necessary before any additional privileges are granted.
   * Link above includes a section on how to join the Origin Trial, and there's also a mailing list if you have any questions.
   * Wilander: Thanks for the update.  I'm happy that you're starting with a limit that's small enough to fit on a screen.  This will be super-interesting if this limit works for the companies that reach out to you.  We're also very interested in knowing the purpose of each domain — e.g. point out "accounts.google.com" as the SSO domain for the FPS — it would help us build the feature we're particularly interested in, making the path smooth for a login experience (how UI behaves, how long storage access lasts, isLoggedIn, etc.).  If you could ask the companies in the OT for that information, then it would be really valuable to us what you hear from OT participants.
   * Kaustubha: I know there's an open issue for this.  Is SSO the only domain use that you want to call out?  Any others?  Do you think only one purpose per domain, or there might be multiple uses?
   * Wilander: We did talk about a whole classification at some point, including login/advertising/content, there were others but not hundreds.  Might be useful to other browsers also.  I think this came up at the XXXXXX <scribe missed this> seminar.  This is relevant to privileges of third-party scripts too — if we knew classifications of domains, then could limit where scripts can load from, or could give some scripts lower power.
   * Kaustubha: My worry in asking for these domain-use-case strings is that it's hard to imagine how we could enforce or verify the assertions
   * Wilander: We'd want something like "you can only choose one domain as your login domain".  It would be really useful as a way to leverag the set — this one domain has elevated rights to ask for permission.
   * Another question from Kris, moved to #slack due to lack of time.



Update on Global Privacy Control - Ashkan Soltani
   * Update/Press release on 01/28/21: https://globalprivacycontrol.org/press-release/20210128
   * CA Attorney General response that GPC ‘satisfies the legal requirement’ as a valid opt-out mechanism https://twitter.com/AGBecerra/status/1354850758236102656
   * Ash S: We gave an update for Global Privacy Day, 40M users using it, bunch of sites that are live or expect to be live in the coming quarter in honoring the header.  Consent Management Platforms adding support also, so that sites can customize their behavior.  Also got a supportive tweet from the Calif AG, who recognized this as a valid mechanism under CCPA.  Other folks have reached out to talk about how to support GPC, and we're hoping other browser vendors will support it as at least an opt-out in California, other states are looking at something similar.  Happy to answer questions.
   * Henry: Across other browser makers, is this something you're considering?  Does consideration fall within this group, or is it adopted by other browser makers?
   * Steve E (Mozilla): We're supportive of the effort, but our primary concern is exposing something to the user that has unclear benefits — we don't want to mislead users into thinking they have certain protections when it's unclear that they will
   * Ash S: Is the AG's rulemaking so far not enough?  Companies that support it have declared it on their website?
   * Ash S: Current AG is transitioning out, new one coming in.  New initiative requires AG and Data Protection Auth to issue guidance specifically on this question.  My guess is that the staff would use their notice-and-correction mechanism over the whole CCPA to tell companies about this.  Since they recognized it in this tweet, they do recognize this as enforceable under CCPA, for companies that have agreed to sign on
   * Sebastian Zimmeck: One of the lessons learned was that there needs to be an enforcement mechanism, this is something regulators understand.
   * Steve: In the CG, we have this one issue that's just a proposal, we haven't heard from any other browser vendors, it's not in a great position right now, curious to hear where you plan to take it.
   * Kris: Numbers question — the 40M active users on it — is that downloads, or active users enabling it?
   * Ash: That's the reported MAUs of all the vendors and extensions that support it.  Includes e.g. Brave where it's on by default.  In the Calif. law, it says that it can be on by default in a privacy-preserving tool.  That might be different in other jurisdictions, and if so then we'll need to address that.  But in CA it's explicitly allowed.  So if you use Brave and go to NYTimes, you will be automatically opted out of the sale of your personal information under CCPA.



Any other business
   * Next meeting in 2 weeks.  Reminder that if you want to dig further into a specific topic, we can put together an ad hoc meeting.  You can [file an issue here](https://github.com/privacycg/meetings/issues/new) to kick that off.



Attendees (sign yourself in):
   1. Allan Spiegel
   2. Andrew Knox (Facebook)
   3. Anne van Kesteren (Mozilla)
   4. Anthony Coccia (CJ Affiliate/Epsilon)
   5. Anthony Molinaro
   6. Aram Zucker-Scharff (The Washington Post)
   7. Arnaud Blanchard (Criteo)
   8. Arthur Edelstein (Mozilla)
   9. Ashkan Soltani (Georgetown / GPC)
   10. Ben Savage (Facebook)
   11. Bill Densmore (ITEGA)
   12. Bill Maslyn
   13. Brad Rodriguez
   14. Brendan Riordan-Butterworth (IAB Tech Lab / eyeo GmbH) 
   15. Brian May (dstillery)
   16. Cezary Cerekwicki (Opera)
   17. Chelsea Gong
   18. Chris Needham (BBC)
   19. Chris Wood
   20. Christian Dürschmied (Bundesverband Digitale Wirtschaft)
   21. Christine Runnegar (PING co-chair)
   22. Christy Harris (Future of Privacy Forum)
   23. Daniel R
   24. Dave Harbage
   25. Don Marti (CafeMedia)
   26. Eli Grey (Transcend)
   27. Eric Mwobobia (ARTICLE19)
   28. Ericka Wright
   29. Erik Anderson (Microsoft)
   30. Farah
   31. Henry Lau (Privolta)
   32. Hitesh Lad
   33. Jack Frankland
   34. Jade Kesler (Google Chrome)
   35. James Hartig (Admiral)
   36. James Rosewell (51Degrees)
   37. Jeegar Shah
   38. Joe Genereux (Brave)
   39. Joel Odom (Salesforce)
   40. Johann Hofmann (Mozilla)
   41. John Wilander (Apple WebKit)
   42. Jonathan Kingston (DuckDuckGo)
   43. Josh Karlin (Google)
   44. Kate Cheney (WebKit)
   45. Kaustubha Govind (Google Chrome)
   46. Kelda Anderson (Microsoft)
   47. Kris Chapman (Salesforce)
   48. Kushal Dave (Scroll)
   49. Les Cochrane
   50. Lisa Levasseur (Me2B Alliance)
   51. Liz Hurley (CJ Affiliate/Epsilon)
   52. Lola Odelola (Samsung Internet)
   53. Marcal Serrate
   54. Mark X
   55. Marshall Vale (Google)
   56. Martin Alvarez
   57. Marçal Serrate
   58. Melanie Richards (Microsoft)
   59. Michael Kleber (Google Chrome)
   60. Mike O’Neill (Baycloud)
   61. Nicholas Chrysanthos
   62. Paul Bannister (CafeMedia)
   63. Paul Marcilhacy (Criteo)
   64. Peter Saint-Andre (Mozilla)
   65. Pranjal Jumde (Brave)
   66. Raj
   67. Rob van Eijk (Future of Privacy Forum)
   68. Rowan Merewood (Google Chrome)
   69. Russ Hamilton (Google)
   70. Russell Stringham (Adobe)
   71. Sam Goto (Google Chrome)
   72. Sam Macbeth (DuckDuckGo)
   73. Sam Weiler (W3C)
   74. Sean Harrison (Google)
   75. Sebastian Zimmeck (Wesleyan University)
   76. Steven Englehardt (Mozilla)
   77. Steven Valdez (Google Chrome)
   78. Stuart Watson (Camelot)
   79. Tanvi Vyas (Mozilla)
   80. Theodore Olsauskas-Warren (Google)
   81. Tim Cappalli (Microsoft Identity)
   82. Tony Resendiz
   83. Valentino Volonghi
   84. Viraj Awati
   85. Vittorio Bertocci (Auth0)
   86. Wendell Baker (Verizon Media)
   87. Wendy Seltzer (W3C)
   88. William Bock
   89. Yuan Chen
