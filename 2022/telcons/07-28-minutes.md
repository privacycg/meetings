# 28 July 2022 Privacy CG Meeting
* Chair: Erik
* Scribe: Cameron Boozarjomehri

# Agenda
* [CHIPS](https://github.com/privacycg/CHIPS)
    * [CHIPS and the Path attribute #47](https://github.com/privacycg/CHIPS/issues/47)
    * [10 cookies per cookie jar limit #48](https://github.com/privacycg/CHIPS/issues/48)
* Any other business

## Notes

### [CHIPS and the Path attribute #47](https://github.com/privacycg/CHIPS/issues/47)
* Dylan Cutler: When Chrome first proposed CHIPS they wanted to ensure all cookies also use the “host” prefix. Encourage better security attributes for cookies but gotten feedback that it's problematic in place of 3rd party cookies. Also got rid of the “no domain” requirement because limits adoption over 3rd party cookies. Lead to opening up if Path attribute may be a suitable replacement, should we keep it or take it away?
* John Wilander: Apple WebKit, need to hear where CHIPS stands and the connection to 1st party sets (added opt in to partitioning)
* Dylan: Opt-In partitioning still done via cookie attribute (what are the semantics and additional requirements)
* John: but these are not cross-site
* Dylan: partitioned can also be cross-site in terms of how they are set, some terms still need to be standardized. Partitioned cookies need this attribute to validate cookies so we can define the semantics of if they are valid.
* John: wanted to check nothing left over from 1st party cookies discussed
* Kaustubha: Wanted 3rd parties servicing first parties to have similar access. From the spec we want to separate out First Party Sets, on CHIPS we will discuss 1st party in WICG, CHIPS will use First Party Set as top-level key. We published an update yesterday (issue #92 - [https://github.com/WICG/first-party-sets/issues/92](https://github.com/WICG/first-party-sets/issues/92)) on latest info for 1st party sets. Please leave comments on the issue.
* John:: wanted to check privacy/security risks since all attributes will cause some degree of developer pain
* Ben VanderSloot: Mozilla thinks it makes sense to not have the Path requirement, similarly may wanna get rid of the “secure” attribute as well
* Erik Anderson: we could see the perceived privacy improvements and getting sites over to HTTPS, hard to tell if offering viable way for sites to keep working or cause larger harms to developer pain over privacy gains.
* Dylan: So for path, any other value than / would limit the context would make the context more restrictive. Don’t foresee too much breakage, don’t see huge benefit, opposite is remove “no domain” req disallowing certain URL paths from sharing cookies. Using secure 3rd party features to improve online security has a benefit for users.
* Ben: I agree, makes more sense than path attribute, common use case is these auth scenarios, and would prefer HTTPS everywhere
* Mike O’Neill: Difficulty with giving consent if path attribute unavailable, for different languages/legal requirements, want to have regional contextuality. Want to have fine grained cookies for language (or country)  variance.


### [10 cookies per cookie jar limit #48](https://github.com/privacycg/CHIPS/issues/48)
* Dylan Cutler: Limit # of cookies available to 3rd parties to less than 180 on chrome. Partitioning 3rd party state means increased memory footprint for browser. Given we want to impose stricter limit to what can be set in particular partition, that was the reasoning to including the limit in the proposal. Derived from HTTP archive data (HTTP req/response) and 10 cookie limit = 99% of cases on normal web but not the “dep web”. Some feedback that the 10 limit may be too strict, want to know what others think is reasonable.
* Kaustubha: really wanted to make decisions based on query data in a manner similar to what we are seeing. But now with partition everyone gets something different because each gets their own cookie jar. Want to make it clear the caveat with HTTP archive data is that it is crawler driven and doesn’t account for user action or private/enterprise domains. Not perfect but best we could do.
* Erik Anderson: any mechanism to see when sites hit the limit (is there a limit we expect to hit frequently, however arbitrary, how can sites provide context).
* Kaustubha: hoping to hear that feedback and any lowering that limit would be backwards incompatible, hoping starting with tighter limit, can always increase it. Specific feedback provided by Akami (CDN) considering adding to the public suffix list. Currently all share same limit.
* John Wilander: maybe with same site cookies may need 2-3 cookies. Good for performance and memory 
* Kaustubha: issue we are seeing is need to migrate system, significant effort people using multiple cookies. More about prioritizing systems.
* Ben: several points 1) going off crawled/archived data would likely undercount cookies (didn’t encounter deep web). Not going through full behavior path means you are missing a lot of cookies. Mozilla has nothing ourselves but browser telemetry would be more compelling. 2) setting limit by size (in bytes) not # of cookies would help avoid perverse incentives to have a few bloated cookies.
* Kaustubha: We considered the size limit but didn’t find a binding size
* Dylan: usually can’t go over 4kb per cookie
* Ben: doesn’t matter if it's 10 4kb cookies or many more smaller cookies that negatively impact the cap
* Dylan: given the size + number limit for cookies. Should we make the limit based on size only an make it less than like 40kb total
* Ben: yes, better by byte count than cookie number
* Aram Zucker-Schrarff: Devs aren’t gonna be aware of the memory limit versus count limit and need clear communication for if someone hits that limit
* John: This particular issue has had many problems historically (like truncating cookie headers) and no mechanism to reliably tell servers what cookies get dropped. Work in ITF to evict right cookies first but we have nothing reliable to tell sites we are not taking more cookies.
* Aram: I agree, I think this is a situation for even high priority for more messaging
* John: should it be part of HTTP or a server round trip?
* Aram: having the API on page is where the issue may be (Especially in this use case). Issue is per page not server request.


### Any other business
* TPAC:
    * Erik: several registration packages. Got a PrivacyCG slot that monday. Sits between privacyCG calls so may just cancel  our september calls. Opinions on canceling? We can discuss in privacyCG channel/slack
* Hi from your new chair
    * Martin is number 1, greatest ever. We are blessed by his presence. +1

## Attendees
1. Erik Anderson (Microsoft Edge)
2. Cameron Boozarjomehri (Mozilla)
3. David Dabbs (Epsilon)
4. Chris Needham (BBC)
5. Dylan Cutler (Google Chrome)
6. Tim Huang (Mozilla)
7. Tanvi Vyas
8. Martin Thomson (Mozilla)
9. Joel Odom (Salesforce)
10. Mike O’Neill (Baycloud)
11. Kaustubha Govind (Google Chrome)
12. Benjamin VanderSloot (Mozilla)
13. Wendy Seltzer (W3C)
14. Michael Kleber (Google Chrome)
15. Emily Lauber (Microsoft)
16. Harneet Sidhana (Microsoft Edge)
17. Daniel LaLiberte (Google Chrome)
18. John Wilander (Apple WebKit)
19. Matt Mara (CJ)
20. Tom Jones
21. Aram Zucker-Scharff (The Washington Post)
22. Brian Campbell
23. Don Marti
24. Jen Schutte (P&G)
25. Russell Stringham (Adobe)
26. Grant Nelson (TripleLift)
27. Alexandre Nderagakura (IAB Europe)
28. Nick Doty (cdt)
29. polpojan
30. Kris Chapman (Salesforce)
31. Don Marti (CafeMedia)
