---
layout: post
title: "RecordSeek, Redesigned From the Ground Up"
description: "More Power, Same Product"
category: RecordSeek
tags: [RecordSeek, Genealogy, FamilySearch, Ancestry]
comments: true
imagefeature: "delivery.jpg"
overlay_opacity: 0
overlay_color: 
---

RecordSeek, for those who don’t know, is a powerful utility to attach webpages as sources to FamilySearch.org. Two weeks ago our service went down. Why did it go down? Well, when I created RecordSeek the FamilySearch Platform API (the way third parties connect to FamilySearch) was brand new. In fact, I was the first one to use it. But, I didn’t exactly use it the proper way (that was still being created). I connected through the FamilySearch site code, which was a proxy to the Platform API. Long story short, FamilySearch changed their code and RecordSeek went down.

Recently, there has been an opportunity for me to do some consulting related to a model similar to RecordSeek. It gave me the drive to essentially redesign RecordSeek from the ground up. I am happy to announce that today that redesign has just been deployed, and you don’t have to change a thing from before to start using it!  So, there have been a bunch of changes. I’d like to go over those now, and talk about some of the new goodies we have to offer.

## Media Upload Disabled (for now)  :(
The sad news is, right now Media (image/PDF) upload has been turned off. Why you ask? Because we're working on getting re-certified to use the FamilySearch [memories API](https://familysearch.org/photos/). That means, in the near future, all media will be uploaded directly to FamilySearch instead of using our little proxy client. Bear with us, as we're working with FamilySearch now to make that happen.

## Customizable Sources Flow
In the old RecordSeek, the moment you updated the source and clicked next, the source was created on FamilySearch. We now give you the option of immediately creating the source, or you can go through the whole process and the source will be created and attached when you find an ancestor. It’s entirely up to you.

## Much Better Searching & Selecting
Our searching for Ancestors has improved a LOT. You’ll see similar features to FamilySearch, and it’s sure to put a smile on your face. You can now even do advanced searches and navigate through all the results.

## [Ancestry.com](http://ancestry.com) Integration!
Yes, you read correctly. RecordSeek now allows you to save websites as sources to Ancestry.com! We’re using a very basic implementation for now, but it's fully functional. Give it a try and let us know your thoughts.

## A Bigger Focus on Support
We now have a [dedicated support tracker](https://github.com/dovy/recordseek/issues). I really am sorry for my abysmal support to date, I’m fixing that now.  :)  If you have a question, concern, or even an idea, head over to our new [support tracker](https://github.com/dovy/recordseek/issues) and I’ll be glad to help.

## Third Party Site Integrations
Do you have a website that would benefit from RecordSeek integration? We offer services to third parties to integrate FamilySearch, and now also Ancestry.com, to your website. We do request a donation for that use. If you would like to learn more, please contact me at [me@dovy.io](mailto:me@dovy.io?subject=RecordSeek Integration) today.

## Please [Donate](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=TWXLZKGQZJFZY) to the Project
RecordSeek is a labor of love. I have no desire to every charge for this source linking. It does, however, take substantial effort to maintain at times. If you like RecordSeek, please [consider donating to keep the effort live](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=TWXLZKGQZJFZY). I appreciate your support.

## Thank You For Using RecordSeek
Honestly, you never know how well your product is liked until it goes down, ha! The outpouring of concern has been wonderful to me. I am SO glad you like RecordSeek and I am so glad it's saving you time. It's whole purpose is to make your life easier. I'm glad to hear it's doing just that.  :)
