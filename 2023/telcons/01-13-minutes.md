# 12 January 2023 Privacy CG Meeting

* Chair: Martin Thomson
* Scribe: Cameron Boozarjomehri

## Agenda

* #154 Reloading frame after granted storage access to enable efficient site isolation
* Any other business

## Notes

* Storage Access API Issue 154
   * John Wilander: Issue on site isolation. Sites load many iframes from other sites especially ads. If you isolate each iframe to its own thread this can be problematic for resource constrained machines. Can We better manage these processes? The Login status APi could be something to look to to overcome these constraints. Currently Login Status api is not implemented so the idea became, if we block all 3rd party cookies then cross site iframes won’t cary sensitive data since all auth is typically 1st party. The switch is if a 3rd party is granted access via storage access and needs to be isolated for loading sensitive data. Less sites for all conditions, can you, at a later point, switch to another point, for an embedded cross-site iframe. You’d need to reload the iframe at that point. Could we allow that? How? Can we let devs control when that handoff occurs? The main point I read from Chris’s feedback was can we do this heuristically instead? Worst case scenario
      * You load a news site
      * It’s full of 3rd party iframes
      * We know the user cannot be logged in there?
         * In this case we could put all iframes on same process
      * But if a user logs in on another tab, then we can’t isolate as easily
   * Chris Fredrickson (Chrome): 1 suggestion I had was to use some new response header to indicate a document wants to load or access 3rd party cookie in the future. The doc level could opt in to sending a signal to the browser to give confirmation something should be loaded into a separate process. Switch from non-isolated to isolated
   * John: agreed, the response header seems good, if the response header becomes popular, then the browser engine will get to a point where they need less resources or separate processes and then we haven’t solved the problem
   * Nick Doty: I was wondering if there was developer ergonomic to the reloading. If we always reload then that answers the swapping question by just always reloading and gives access or doesn’t. Most devs wouldn’t need this api but it could be more straight forward
   * John: From an eng perspective and it’s unfortunate we didn’t write it like that originally and it’s attractive from many perspectives, we prefer it that way with a clean slate. But devs may be half done before needing to reload and now that login and reload causes them to lose their progress
   * Cameron: 1. Can I understand a bit more about the resource constraint. I feel like there might be a way to deprioritize some of the processing.  What sort of machines are we thinking about? 2. Is there no way that we can keep isolation and deprioritize those iframes.
   * John: A process is still a process, even with throttling it has overhead at the OS level. You have to have management with prewarming and have a pool. Mobile devices or cheap devices.  More than old smart phones.  Even for a great laptop, you want a lean browser. And early on when chrome hadn’t shipped site isolation it was on Google’s mind to limit processes.
   * Aram Zucker-Scharff: What occurs to me is an iframe with incentive to add a button to escalate access sound like high encourage for add fraud. We already have issues for iframes where ads have misleading buttons. On top of that, the potential that the button triggers an automatic reload to generate another impression would further this problem in the ad space. Maybe there needs to be a way to have top sites prevent this on specific iframes. Maybe this is already a thing, but I think that setting would allow the service that serves most iframes doesnt need that privilege because they could be consolidated by the made ad server. So the biggest problem is to avoid abuse. It depends on how it’s executed by the browser. We have stuff in place to prevent reloads to continue.
   * John: that’s interesting to hear. Publisher sites are trying to limit false positives on impressions
   * Aram: yes this could be a form of fraud or ad resale where iframes reload other ads to further revenue.
   * Chris: few questions on site isolation for chrome. I spoke with the site isolation team on android. Chrome is worried about too many processes on mobile (less than 2 gb of RAM), but that comes from the resource angle where as this proposal comes from storage access API is a requirement to allow process access, where as chrome has worked to avoid this do to resource constraints. Thbis page might be able to run in its own process but not each iframe
   * John: the point was more to just choose when to make choices of sharing between 1st party and iframes and do it heuristically, potentially based on observing past process loading and how users interact. And in general we want to keep 1st party separate from 3rd party, usually ad, iframes
   * Chris: Make sense the header makes more sensenow
* Site isolation is starting to get expensive
   * John: We in our work on site isolation learned combining partitioning and site isolation could be extra difficult
      * Say you need to split the threads when the site is both 1st party and 3rd party
      * This further complicates the site isolation problem
   * Martin: thanks this means there are maybe some quick hacks that hint to do this, or a policy change to have some scripts run in other areas
   * Aram: The other thing that might help is the outcomes for why ad iframes are loaded could be better achieved with lazy loading iframes. Rather than load all ads at once maybe we can use lazy loading at a browser level to help with memory process use
   * Cameron: +1 what Aram said
   * Nick: I am vaguely aware of spectre, is it trivial to run spectre and run overlapping code and get some memory some of the time
   * Martin: my understanding is the processes at the OS level can be isolated but it can be somewhat trivial to spectre
   * John: as a security thing spectre can become a case where the victim is helping the attacker
   * Martin: the cases where spectre is there is fairly high and might need some sort of shared plumbing to pass back and forth
   * Chris: +1 the concern. If we go down the partitioning route then we need to worry about colluding partitions especially when controlled by the same entity. https://www.usenix.org/conference/woot17/workshop-program/presentation/van-goethem Side channels can be reliably created to link data not just between processes in browser but also between browsers and we confirmed some of this work
   * John: in the spectre case we could hope for 1 core per site where it gets more constrained overtime
   * Martin: we moved the GPC spec into the org and make sure all the bits are there so everyone can contribute. I suspect the editors of that spec will be looking at those issues and raising things to update the charter and cover the changes. We might want to address the items that have been open for a while
   * John: what to move login status to FedCM and move PCM to the ad group
   * Martin: we can initiate those conversations when John is ready but may need to wait a bit on that.

Attendees (sign yourself in):
   1. Erik Anderson (Microsoft Edge)
   2. Kaustubha Govind (Google Chrome)
   3. John Wilander (Apple WebKit)
   4. Ben Savage (Meta)
   5. Martin Thomson (Mozilla)
   6. Cameron Boozarjomehri (Mozilla)
   7. Risako Hamano (Yahoo Japan)
   8. Nick Doty (CDT)
   9. Don Marti (CafeMedia)
   10. Aram Zucker-Scharff (The Washington Post)
   11. Chris Fredrickson (Google Chrome)
   12. Stefan Popoveniuc (???)
   13. Sam Goto (Google)
   14.  Darryl Green Jr. (The Washington Post)
