# 14 July 2022 Privacy CG Meeting
* Chair: Erik
* Scribe: Cameron Boozarjomehri

# Agenda
* [Storage Access API](https://github.com/privacycg/storage-access)
    * [Forward Declared API Addition #100](https://github.com/privacycg/storage-access/pull/100) pull request
* [CHIPS](https://github.com/privacycg/CHIPS)
    * [Should CHIPS be hostname-bound? (the no-Domain requirement) #43](https://github.com/privacycg/CHIPS/issues/43)
* Any other business
    * Charter updates: [PR #38](https://github.com/privacycg/privacycg.github.io/pull/38), [PR #39](https://github.com/privacycg/privacycg.github.io/pull/39)
## Notes
### [Forward Declared API Addition #100](https://github.com/privacycg/storage-access/pull/100) pull request
* Ben Vandersloot: a bit of background, storage access api there are some constraints we’d like to solve with it but it’s not perfect. It’s been preferred off in Firefox and we are open to discussion
* Requires JS call from within 3rd party iframe. If no iframe then API won’t work. Won’t work with same 3rd party and top party. Use Case where A.com needs to check for B.com. User can navigate to B.com and then come back brining the required cookies. Want to support this use case but the redirect approach won’t support iframe. Want to split out api request call to one on the ID provider side and the 2nd to happen in the top level site to work within the iframe as opposed to via redirect
* Requires user activation, contains door hanger, and when you go back to the original page will resolve ID issue
* Jason Nutter: Had a chance to try this out and opened 1st floor issue to request these enhancements. Provides better UX and lets us prompt and better set expectations. From design standpoint some design quirks for iframe and this solution is a little less finky. Would be better if browsers returns useful errors from API (would get Undef as error in catch statements). Potential enhancement to get bidirectional storage access when embedded party is top frame unlocks front channel logout. Instead of reliant party embedding idp, idp embeds for a reliant party. Originally had lots of hidden iframes but not can show prompts because both parties are in the top frame. Eager to see other browsers follow suit
* Ben: comment on PR 100 once linked
* Nick Doty: somewhere to see this explanation with examples fr test it?
* Ben: I can get some instructions together or can rebuild and do quick demo
* Erik: high level prompt is shown on other site
* Ben: click login with X, get pop up or new tab, click to do login action returns doorhangar/ auto tag, user navigates back and a background handshake passes the credential
* Brian Lefter: how does this work with Fx ephemeral granting, is this okay for Microsoft/other use cases?
* Ben: ephemeral grants are just diff grant in same process, request accept/deny, if that fails then go to other prompt
* Brian: are you requesting forward access that could expire before you get to use it?
* Ben: that is accounted for and seems to work
* Johann: I’m happy to see movement here, how do you see this lining up with FedCM (solves similar problems like storage access API). 
* Ben: Storage access did start out as iframe, but once we get that permission flag set for storage key, sets it for all requests of that type. Even without iframe could still be useful. Not all cases meet that need for iframe. Useful beyond FedCM, want to show off first. As FedCM is shipped will have less need for this
* Johann: FedCM was a better interface for all these but could be future deprecated for other use cases
* Ben: not end all be all, but this works better for variety of opportunities for FedID CG. It’s complicate but hopefully fedCm gets rid of some use cases long-term

### [Should CHIPS be hostname-bound? (the no-Domain requirement) #43](https://github.com/privacycg/CHIPS/issues/43)
* Dylan Cutler: When we designed partitioned attribute wanted to separate cookie from partition attribute. Make cookies more origin bound and make sure cross-site cookies set in cross-site context and not set by malicious sub domains. Involves re-architecturing that adds latency. Cookies being hostnamed bound help facilitate removal of 3rd party cookies. Wanted to get feedback.
* Ben VanderSloot: I agree it would be nice but not priority. If causing problems leading to deployment of cookies best to not let happen together.
* Erik Anderson: +1 from MS/Edge


### Charter Updates
* Hober (Tess): Pul request 39, replace Tanvi with Martin as Co-chair. Make sure charter reflect previous decision on login access is changed, added CHIPS as work item, dropped 3rd party sets. Charter has changed, nothing to discuss.
* Tanvi: thanks for this opportunity, was great to work with you all and hope to work with you in future just not as chair. Great learning experience for me “Keep on rocking the free web!”
* All: we will miss you :’(
* Tess: now that Martin is Co-chair, it’s sub optimal since he is APAC and so he can not attend very often. Maybe we should change cadence to alternate time so he can attend more frequently. How can we be more inclusive to APAC participants?
* Erik: I think we can do every 3rd call as APAC friendly
* Cameron: could we make it every other call?
* Erik: we could, more challenges with EU folks for APAC friendly time. Something to consider
* Tess: don’t have time that is APAC/EU friendly. Every 3rd call would map nicely to the 3 chairs (he would chair on his time)
* Tanvi: I filled in 16 missing participants. Please sign yourselves in as I usually duid that and I won’t be there to sign in in the future.

## Attendees
1. Aram Zucker-Scharff (The Washington Post)
2. Ben Kelly (Google)
3. Benjamin VanderSloot(Mozilla)
4. Brian Lefler (Google Chrome)
5. Cameron Boozarjomehri (Mozilla)
6. Chris Needham (BBC)
7. Chris Wood (Cloudflare)
8. Cfredric
9. Daniel LaLiberte
10. Dylan Cutler
11. Emily Lauber (Microsoft Identity)
12. Ericka Wright
13. Erik Anderson (Microsoft Edge)
14. Erik Taubeneck (Meta)	
15. Frantz
16. Gabby Schnaid
17. George Fletcher
18. Harneet Sidhana
19. James Hartig (Admiral)
20. Jason Nutter (Microsoft)
21. Joel Odom (Salesforce) - North America
22. Johann Hofmann (Google Chrome)
23. Jeremy
24. Kaustubha Govind (Google Chrome)
25. Kelda Anderson
26. Lee Graber (Tableau/Salesforce)
27. Lubna Dajani
28. Mike O’Neill (Baycloud Systems)
29. Nick Doty (Center for Democracy & Technology)
30. Paul Zühlcke (Mozilla)
31. Polikseni Pojani (CJ Affiliate)
32. Pragya Seth
33. Providence Baraka (Article 19)
34. Same Weiler
35. Tanvi Vyas (Mozilla)
36. Theresa O’Connor (Apple, co-chair)
37. Tim Huang(Mozilla)
38. Vittorio Bertocci (Auth0|Okta)
39. Wendell baker (Yahoo)
40. Wendy Seltzer (W3C) 
41. Yi Gu (Google Chrome)
