---
layout: post
title: "Suggestions for the Theme Review Team"
description: "Please Improve Your Delivery"
category: WordPress
tags: [WordPress, Theme Review Team]
comments: true
imagefeature: "delivery.jpg"
overlay_opacity: 0
overlay_color: 
---

My name is Dovy Paukstys. I am the co-founder and lead developer of Redux Framework.  We represent over 7500 known developers with many of products, which serve close to 1+ million users.  Our developers are from all across world, many of which speak different languages.  I am lesser known in the WordPress community directly, coming onto the scene in 2013 and not spending much time on the WordCamp circuit.  During my time in this industry, I have grown Redux Framework from 200 developers, merged with 3 other frameworks, consequently creating the most powerful WordPress options framework to date.

Every change we make to Redux Framework affects many people directly.  My team and I are rabid about backwards compatibility as to not alienate our user base.

## If you haven't heard

Recently, the WordPress.org Theme Review Team (TRT) has made a few official changes, though - as pointed out repeatedly by the team - such changes are not newly discussed.  These changes include the barring of options panels that use the Settings API, forcing developer to use the Customizer only, that themes cannot generate user content (NOW including custom widgets or textarea fields in the customizer), and that a theme developer cannot optimize their theme for preview display in the Theme Preview on WordPress.org with demo content.  The primary argument from the TRT regarding these changes claimed that these policies were in place or discussed for years.  Now, the more recent decisions and enforcing of said rules have come as a shock to a larger group of developers than perhaps even the TRT believes.

## How I got involved

I came into the TRT discussion because the Customizer decision directly affected a small subset of the Redux Framework user-base. I wanted to support my users and not leave them without a solution.  I've built a complete customizer solution for Redux that I am working on finishing off, and will be releasing a subset of that offering as open source, to the community.

I will say, I have been one of the most vocal opponents of the customizer decision. Not only because I run the Redux Framework project, but because I do not feel that the customizer (until my implementation) was a sufficient replacement for any Settings API solution.  I also feel that options affecting display are OK for the customizer, but that theme developers should be permitted to use Settings API panels for less display-specific settings.  Alas, my suggestions were dismissed.  The policies were in place and the TRT were unwilling to bend. What shocks me is that not only did the "customizer only" discussion spark the [most discussed article of all time on WPTavern](http://wptavern.com/wordpress-org-now-requires-theme-authors-to-use-the-customizer-to-build-theme-options), but that the TRT still doesn't believe the debate to be of any concern.  The shockwaves of this one decision alone were so dire, one of the most successful WP.org theme shops up and sold their company because of the implications behind the TRT's decision.


## Frustration in the Approach, Not Entirely The Decision
I'd like to preface by saying that the TRT members are all good people.  They have devoted their time and effort, and are attempting to improve a market that has slowly degraded over time.  In fact, I commend their effort.  I do not believe anyone is malicious in nature, nor do I think they wish to cause the indignation they have created.  Some of it was mishap; other was some lack of care.

What has upset me is not so much any of the decisions the TRT has made as of late, instead it has more to do with the approach.  The interaction I have had has not so much been "we're willing to change," but more "we're willing to help you conform."  For a group that is allegedly so open, they're not giving developers the impression that their voices matter, this one included.

#### The Customizer Decision

Let's look at the customizer decision again.  The verdict to force themes submitted to wp.org to go 'customizer only' was the ONLY week in which the TRT did not issue a blog post about the forthcoming decision.  I believe wholeheartedly this lack of openness was a mistake and not a malicious decision.  I still believe the TRT should have respected the community and done a re-vote the following week.  Given the negative reaction, perhaps they could have seen that perhaps they'd misjudged, or not given the community the opportunity to voice a concern.  Alas, they did not, stating, "it had been in discussion for years."  That may be true, but those discussions occur only where others must specifically follow.  Most were completely unaware of the discussion, or where it took place.  In addition, from that moment on, all newly submitted themes were required to adhere to the new policy.

Had the TRT approached the implementation of the new 'customizer only' policy in a more phased way, the reaction might have been MUCH different, if not better.  I run a large options framework for Wordpress; I would never break compatibility in such a drastic way.  If for some reason I had to, I would allow a grace period.  The current customizer grace period for wp.org theme developers is six months.  That means existing themes in the wp.org theme repository that do not meet the new policies have six moths to conform, or risk the TRT rejecting updates, or even removal.  All new theme submission must comply immediately.  Could the TRT have given new theme developer three months to comply, giving them a chance for theme acceptance?  They could have.  It certainly would ease the suffering of newcomers.  Better yet, could the TRT have just as easily offered six months before enforcing the 'customizer only' compliance, giving the developer time to prepare?  Easily!  It's the complete disregard toward developers, their time, AND their opinions that I find appalling.

#### The Theme Content Decision
It's long been known that the TRT do not want themes to generate content, because said content disappears when users switch themes.  I understand the sentiment and could easily support it, where such an effort would make sense.  It's a noble and reasonable goal.  Now there is clarification from the TRT that states they will no longer permit Custom Widgets or even textarea fields unless said controls modify existing content.

Consider the following: 

Rather than having a footer field in the customizer, the TRT wants developers to use a footer widget area and inform users to append a Custom Text widget to that field for their footer.  Okay, not an elegant solution, but I guess we can get by.

Let's look at another example: Say your theme has a built in slider.  Some developers would previously use Widgets to generate a slider for the front page.  The TRT now says the only way a developer may accomplish this is via posts of a given post format, or category, and/or metaboxes.  To display something on your site (which is the point of the customizer) you must now have to leave view of your site to generate posts for the same effect.  The effort for the user to utilize such a feature has just increased.  The TRT is now debating the future of further features such as call-out boxes and the like.  I sincerely hope they table those for the time being, considering recent changes, and the reactions those decisions have spurned.


#### The Theme Preview Decision

Before one downloads a theme on WordPress.org, one can preview it.  This is a generic display of a theme's blog capabilities, with the approved post formats.  The problem is, many themes (minus Automattic's) have coded demo content for that display.  The TRT feels that showing features of a theme in this way is unfair, and all theme previews should simply consist of the standard blog content.

How detrimental will this be?  About 50% (or so developers say) of the top 20 themes have coded demos for the theme preview.  Suddenly, all those themes may no long demonstrate their features.  The TRT will now box those themes into the same demo, even if their capabilities are much larger.

## Disappointing Opinions & Conclusion

I think the problem is the TRT and it's members genuine disconnect from what it's like to be a new dev to WP.org.  Additionally, I feel they also have a great condescension toward anyone utilizing a freemium model.

The most disappointing comment I've read in these discussions is probably this quote from a prominent member, quoting another prominent member: 
<blockquote>"If a Theme submitted to a free directory has an adverse impact on your bottom line, then you might want to reconsider your business plan."</blockquote>

Really?  How much more condescending can one possibly be?  Many developers have built their livelihoods on a freemium model, using the WP.org repo.  I hardly think it's the place of a TRT member to disregard their decisions, just because they do not rely on that as a source of income.

My argument is thus:  With some subtle changes, developers could feel a whole lot less attacked.  I believe the TRT are all normal people, doing what they think is right, but perhaps they're been drinking their own kool-aid a little much as of late, and not listening to the community.  Yes, people respond positively in tickets, because they don't want to be blocked.  My interaction with developers has been less positive, in regards to the TRT.

In the end, many developers will jump ship, many will adapt, and eventually the waters will calm.  My hope is the TRT might learn some delivery and begin listening to their developers, instead of just the "Vocal Minority", specifically, their team.