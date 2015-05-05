---
layout: post
title:  "WordPress.org for Theme & Plugin Development - The New Line"
date:   2015-05-5 12:00:00
categories: WordPress
tags: PHP
banner_image: "/media/banners/passing.jpg"
featured: true
comments: true
---

In recent news WordPress.org has drawn a very specific line in the sand. The goal, as it appears, is to finally and forever make the division between <u>what is a theme</u> and <u>what is a plugin</u>. This is a battle that has raged within the WordPress.org repos for years and now it seems as though it is finally at it’s peak.

I’m the lead dev of <a href="http://reduxframework.com/">Redux Framework</a>, a framework used by hundreds of thousands of users and over 7,500 developers from all across the world. I’d say that I have quite some experience in designing for users and developers alike. Because of my user base, I have to fully understand what this means. I have to be aware of every angle so I know what to tell my developers. Obviously this hasn’t kept me from stating my own opinion. I may have ruffled some feathers to get here, but now I believe I understand the actual motivation for these changes. Though I do not completely agree, it is what it is. The line is set. And developers best adapt if they wish to stay on WordPress.org.

## What is a Theme?
> <a href="https://wordpress.slack.com/archives/themereview/p1430845093010068">@chipbennett</a> - Theme(s) create the location/opportunity. The user creates the content. 

The concept is simple. A theme is the **design and display of content**, nothing more. All too often developers (because it’s SO easy) want their theme to embody every feature they can think of. And who’s to blame developers, that’s what may differentiate them from another theme shop! But that is not the intent of themes on WordPress.org.

#### What About WP Editor?
But wait! What about my custom footer text? My homepage area builder? My super cool slider.
According to the TRT (Theme Review Team), this should all be embodied in the other built-in features of WordPress, or a plugin. As a result, the WordPress Editor is nowhere to be found in the customizer, and the TRT desires it to stay that way. Content should never be a function of a theme. A theme can employ any extra feature WordPress or plugins can offer, but the theme itself should not create the content opportunity.


> <a href="https://wordpress.slack.com/archives/themereview/p1430845093010068">@chipbennett</a> - Widgets are powerful. Post formats are powerful. Menus are powerful. Custom post meta is powerful.

This may cause many devs to scratch their heads and scream as some of the top authors on WordPress.org have such features built into their themes. Well, in 6 months time, unless they adapt they can’t push further updates to their theme. In fact, no new theme can submit unless it’s following this requirement closely.

### As I see it, the motivation is two fold. 

#### Data transportability. 
WordPress.org theme users switch themes constantly! If developers house custom functionality in the theme, then POOF, the user’s data is gone and users start emailing WordPress or developers. To do away with this, they decouple content from themes.

#### Security
Things that greatly alter the WordPress core, or that add extra features should be plugin territory. It’s easier to update a plugin. A theme should, in concept, never have the power to bring a site down. It’s much easier to disable a function of a site, than to lose the entire design.

> <a href="https://wordpress.slack.com/archives/themereview/s1430844154009958">@chipbennett</a> - I think a properly coded Theme and a properly coded Plugin should play well together, without either one ever knowing about the other.



### But Wait, This is New! 
Not really. Apparently WordPress has been working on this for the last 5 years, the last two especially. They’re working to clean up the WP.org theme repo and make themes only be what they were originally designed to be.


## Plugins

Also I’ve learned recently that Plugins are changing a bit as well, and I don’t think it’s bad. The PRT is no longer accepting frameworks of any kind in the WordPress.org repo. All the frameworks currently there, are now grandfathered in. (Option Framework, Option Tree, Redux, Etc). If a plugin developer wishes to use those frameworks, they MUST use <a href="http://tgmpluginactivation.com">TGM</a> to have the user install the other plugin. Why? Well, the same reason I made the <a href="http://wordpress.org/plugins/redux-framework/">Redux Framework</a> plugin. It takes the update needs out of the developer (who doesn’t maintain Redux), and back to the team behind Redux.

Essentially, if there is a plugin in the repo and you embed the plugin in your plugin, you’re going to get flagged. They want you to only maintain the code that you’ve created. You can employ anything you want and suggest it to you users, but if you didn’t build it (and it’s in the repo), it should be TMG’d.


## Personal Thoughts
I'll be honest. I think the TRT could have made it much more obvious they were going to make such a vote. I don't think the vote accurately outlines what the community as whole feels. Yet, I see the direction and the desire. I just think it could have been handled much more elegantly.

For example, this <a href="https://developer.wordpress.org/themes/advanced-topics/customizer-api/">wonderful guide</a> that should have been release about 6-18 months ago. And how about doing a soft launch for new themes only and see how it goes before requiring it of all developers.

There are ways to avoid the conflicts that could have been employed. They weren't, so alas we must make the best of it.

I will say I do admire certain people in the TRT who are quite positive about this and are honestly doing what they can. This change was sure to bring animosity upon the whole TRT. I applaud you who are dealing with it with proper candor, and suggest to those taking offense that it's ok if people don't feel as you do. Be open, and you might learn a thing or two you hadn't thought of before.

## Conclusion
In the end the decision is here. The heels are dug in. The line is drawn, and complaining apparently won't make a world of difference. It's sad, but it is what it is.

Now the question I pose to you is are you ok with the decision or should you, like many others, develop your themes for another marketplace?

