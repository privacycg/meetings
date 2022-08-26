# 25 August 2022 Privacy CG Meeting

- Chair: Erik and Martin (who was late)
- Scribes: Kaustubha Govind, Nick Doty

## Agenda

- Notice: TPAC is coming. Registration costs increase at the end of the month, so if you plan to come, now is the time to register and book travel. Chairs are looking for agenda items. We are dedicating half an hour of our precious time at TPAC to a meeting with the FedCM CG, so prepare accordingly.
- CHIPS: "[Add explicit behavior requirement for "Partitioned" attribute on First Party cookies](https://github.com/privacycg/CHIPS/issues/51)" - Dylan ([deck](https://docs.google.com/presentation/d/1PFQ8Tas80sGGuRcBoj_WL7Z8AuU032x9Wgcv_-lG3sU/edit?usp=sharing))
- Storage Access: "[Consider requestStorageAccessFor Method](https://github.com/privacycg/storage-access/issues/107)" - Johann/Matt

## Notes

### [CHIPS: "Add explicit behavior requirement for "Partitioned" attribute on First Party cookies"](https://github.com/privacycg/CHIPS/issues/51)

- Dylan: Presenting "same-site CHIPS" to be defined soon. Recap of CHIPS: Implementation of partitioned cookies, double-keyed on host-key of cookie (per usual), and also partition-key which is the top-level site. Allows 3p embeds to use cookies without tracking across top-level sites. "Same-site CHIPS" are cookies whose partition key is the same site as the host-key. Do we want to disallow partitioned cookies in these contexts? SameSite attribute already exists which. Clear-site-data has different behavior for unpartitioned vs partitioned cookies, Partitioned cookies where defined for 3p contexts.
- Dylan: Pros and cons of disallowing same-site CHIPS. Pro and Cons ([see slide](https://docs.google.com/presentation/d/1PFQ8Tas80sGGuRcBoj_WL7Z8AuU032x9Wgcv_-lG3sU/edit#slide=id.g146d96d7bf3_0_204))
- Dylan: There is a divergence in keying between partitioned cookies and storage. Storage has an ancestor-chain bit, which cookies don't.
- Dylan: Including cross-site ancestor bit for cookies partition key would mean that a cookie set in top-level context would not be accessible in a same-site embed with a cross-site ancestor. [Pros and cons slide.](https://docs.google.com/presentation/d/1PFQ8Tas80sGGuRcBoj_WL7Z8AuU032x9Wgcv_-lG3sU/edit#slide=id.g146d96d7bf3_0_234)
- Dylan: open questions
  - Should we allow same-site CHIPS at all?
  - If yes, should we use the ancestor chain bit in the CookiePartitionKey?
  - If yes, how do we support top-level sites setting cookies for same-site embeds with cross-site ancestors post-3PCD?
- Lee: Main takeaway is that you didn't feel like there were security issues. It's about privacy/tracking. None of the pros/cons are about how it might hurt. It was all about usability, and compatibility. Is that a reasonable statement?
  - Dylan: Yeah, less of a security issue, than privacy.
  - Lee: My ask is that many people don't know if we're embedded in 1p/3p contexts. If we don't break existing scenarios; that would help. Since we are not breaking privacy.
- Nick: Can we clarify implications for disallowing same-site CHIPS. Didn't seem like we need to disallow; but not sure what disallow means. Dropping cookies seems bad for developers opt-ing to more private behavior. What does disallow mean?
  - Dylan: One option would be to ignore Partitioned attribute; but that would mean that the cookie doesn't become available in cross-site ancestor, same-site contexts.
  - Nick: Developer could become confused.
- Lee: Wondering if it might be helpful to show a table with cookie key; with iframe as one parameter; and what cookie you would get. Was confused by your last comment that the cookie would be blocked if there isn't a partition key. What cookies do I get in the iframe? A embeds iframe A; one cookie is associated with domain A, another cookie has the Partitioned cookie with domain A. Which cookie do I get in the iframe A?
  - Dylan:If we allow Partitioned cookie in these contexts; then…
  - Lee: Both is what I'm looking for. Is there a difference between allowing and ignoring?
  - Johann: Depends on how you said SameSite attribute. If you have set it to Lax|Strict, because of the way SameSite works; the intermediate cross-site iframe would prevent inner same-site iframe. If SameSite=None, you would get both cookies. However, the nice thing about partitioned (apart from developer convenience and code portability), it will stay contained to your site, not other top-level sites. It's a bit of extra protection you can give yourself.
  - Lee: So you're saying if I set partitioned cookie in the top-level context; would I not be able to access that cookie via Storage Access API in another context?
    - Johann: Don't use SAA as a security mechanism. If you set SameSite=None, you can be prone to CSRF. Partitioned cookies might protect you, but maybe there are other mechanisms.
  - Kaustubha: to answer the one question Lee had about the behavior today you get on the inside. You were asking about what happens when you block 3p cookies or have tracking protections on. That's not defined in any spec today and every browser does something different today. Hoping to discuss that at TPAC. In Chrome is samesite=none without partitioned attribute we'd block, with partitioned we'd allow it.
  - Martin: We need to put this down in a document before TPAC. If that doesn't happen the chairs might be giving agenda time to someone else.
    - [Document](https://docs.google.com/document/d/1vvRW5HVlHBBB2Sg0M3dO5ZawcBnzh1AgGe3RGm4wRVM/edit)
  - Johann: We also have a breakout about the "future of cookies" about how cookies should be spec'ed. This could be in-scope to that discussion also if we don't have time in PrivacyCG.

### Storage Access: "[Consider requestStorageAccessFor Method](https://github.com/privacycg/storage-access/issues/107)"

(scribe switch to npdoty)

- Matt: (method name TBD). Proposal is to abandon SameParty cookies which relied on FPS. Subsets is also relevant, but not part of this today. Chrome could adopt Storage Access API, with this as an extension.
- Matt: requestStorageAccess could be called by an embedded party after a user interaction with the embedded iframe – browser can do additional checks or prompt the user in that case. FPS-supporting browsers could automatically grant without a prompt. But the embedded party would need to signal to the embedding party that it needs to be refreshed to take advantage of the accessed storage.
  - requestStorageAccessForSite, top-level site can request access on behalf of embedded sites that it knows will need access.
  - Could apply for non-iframe cross-site resources
  - Safari and Firefox have some existing quirks mode APIs for this
  - Avoid abuse: third-party scripts that are embedded could spam with prompts. First-party sets could be one way to avoid that, automatically rejecting if not part of first-party sets.
  - What other trust signals could be possible besides first-party sets? Browser choice a positive, but standardization would also be helpful for developers.
  - Forward-Declared proposal involves redirecting to the third-party site and back which is focused on login use cases.
- Jason Nutter: if the two origins are not in the same set, what would happen?
  - Matt: don't want to depend on FPS. but current proposal is allow for sites in the same set, and otherwise reject.
  - Jason: Microsoft would be interested as a large company with many domains, so would like auto-grant if in FPS, but also to have a prompt when not in a first-party set.
- Kaustubha: just about how to make sure this isn't abused. FPS is just one signal of trust. How do we make sure it's not spammy?
- Lee: would prefer not to see prompts, but if asking the user, is it a question of trust by the first-party or the third-party? The user knows the first-party website.
  - Matt: not sure about the UX of the prompt. Script could be running in the first-party context for itself.
  - Lee: some customers embed us but they don't want to tell users that they're using our product or show our name.
  - Johann: browsers have tried to avoid that scenario in order to make prompts meaningful. Part of FPS is to make the entity more recognizable to the user. [?] For Firefox often tried to avoid prompts in low-risk scenarios. Safari only shows the prompt if the user has clearly interacted with the third party in a top-level context.
  - Lee: seemed like an opportunity for us to guarantee that our name would show up.
  - Johann: auto-granting vs. auto-denying when trying to decrease prompts.
  - Martin: why does your case need first-party context? Lee: would prefer to just use CHIPS, but don't want our name to pop up. Martin: want to have accountability for who is passing information between two different top-level origins.
  - Lee: for federated identity, most IdPs don't want to be put inside your iframe. Have a potential proposal for how to get access tokens from an iframe.
    - Martin: good problem for fedcm.
- Jason: compared to forward-declared, achieves the same outcome with many things that we like. Probably don't need both, and might be confusing to have both.
  - Matt: rsa-for is more flexible in not being redirected to the other site
- Martin: overlap and good movement on finding common ground.
  - Would like to see a proposal that we talk about at TPAC.

## Attendees (sign yourself in):

1. Erik Anderson (Microsoft Edge)
2. Brandon Maslen (Microsoft Edge)
3. Lee Graber (Tableau / Salesforce)
4. Brian May (dstillery)
5. Russell Stringham (Adobe)
6. Kaustubha Govind (Google Chrome)
7. Nick Doty (CDT)
8. Dylan Cutler (Google Chrome)
9. Tim Cappalli (Microsoft Identity)
10. Jason Nutter (Microsoft Identity)
11. Akash Nadan (Google Chrome)
12. Matt Reichhoff (Google Chrome)
13. Sam Weiler (W3C/MIT)
14. Johann Hofmann (Google Chrome)
15. Martin Thomson (Mozilla)
16. Tess O'Connor (Apple)
17. Sam Goto (Google Chrome)
18. ?? TomJ
19. ?? Yi Gu (Google)
20. ?? Marijn Kruisselbrink (Google)

