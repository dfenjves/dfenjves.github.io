---
layout: post
title: How Legal Is Web Scraping?"
date: 2014-04-18 13:53:13 -0400
comments: true
categories: law, scraping, internet law, nokogiri, open-uri, flatiron school
---

I’ve recently learned how to perform basic web scraping using nothing but Nokogiri, Open-Uri, Ruby, a paperclip, and the internet. It’s an awesome feeling - to be able to MacGyver your way deep in to the code of a massive website and pull out exactly what you need, throw it into a database, and then manipulate that data to your heart’s content. It’s like I’ve discovered that I have a secret superpower, and am only beginning to see what I can do with it.

-> {% img /images/macgyver.jpg %} <-

-> *How I felt when I scraped my first website* <-

But, as the cliched superhero movie line goes: Son, with great power comes great responsibility. Cue John Williams music.

What is that responsibility when it comes to web scraping, exactly? Well, web scraping lives in a grey area of internet law. It’s a weird twilight zone of legality, where people operate in an uncertain haze- unsure of whether scraping is within or beyond the bounds of the law. Let’s examine this grey area in further depth.

But first, a primer on scraping. Imagine being able to set up a system on your computer that goes through every link on Amazon.com and finds the name of every book in their catalog, it’s price, and it’s release date. Or building a program that is able to go through your Facebook friends and pull their publicly available information in to your computer. Or grabbing prices on Craigslist for all 400 apartment listings within a mile of your house in New York without having to manually click on each link. That is web scraping in a nutshell. Using a variety of programs, a scraper is a computer program developed to copy publicly available information from a website.

Notice that I said *publicly available*. Obviously scraping someone’s online bank accounts will get you thrown in jail faster than you can say *identity theft*. It’s the stuff that’s out there for everyone to see that makes for an interesting exploration. Is information in the yellow pages subject to copyright protection? Or listings on craigslist? What about the titles and urls of YouTube videos? Check out these interesting legal cases and see what conclusion you can come to:

**Feist Publications, Inc., v. Rural Telephone Service Co.**, *499 U.S. 340 (1991),[1] is a decision by the Supreme Court of the United States establishing that information alone without a minimum of original creativity cannot be protected by copyright. In the case appealed, Feist had copied information from Rural’s telephone listings to include in its own, after Rural had refused to license the information. Rural sued for copyright infringement. The Court ruled that information contained in Rural’s phone directory was not copyrightable and that therefore no infringement existed.*

Ok, so pulling information from something like a telephone directory isn’t copyright infringement, because they’re facts and not an artistic creation. Cool. But what is the equivalent of that online? Craigslist? Yellowpages.com (ha.)?

**American Airlines vs. Farechase**
 *AA successfully obtained an injunction from a Texas trial court, stopping FareChase from selling software that enables users to compare online fares if it also searches AA’s website. The airline argued that FareChase’s websearch software trespassed on AA’s servers when it collected the publicly available data. FareChase filed an appeal in March 2003. By June, FareChase and AA agreed to settle and the appeal was dropped.[9]*

Woah, woah, woah. Farechase was blocked from scraping AA’s site, despite the data begin publicly available and factual (although sometimes airline prices can definitely seem like fiction) ? That seems to contradict the Feist case, doesn’t it? If you dig a little deeper in to the decision though, you’ll note that American claimed their ‘web fares’ were restricted to only their site (as opposed to the fares disseminated to other websites - think Orbitz or Kayak). Since they were restricted to their own site, perhaps the scrape wasn’t pulling *publicly available* information, per se. Although that seems like something of a leap, if I may say so.

**eBay v. Bidder’s Edge**, *100 F.Supp.2d 1058 (N.D. Cal. 2000), was a leading case applying the trespass to chattels doctrine to online activities. In 2000, eBay, an online auction company, successfully used the ‘trespass to chattels’ theory to obtain a preliminary injunction preventing Bidder’s Edge, an auction data aggregator, from using a ‘crawler' to gather data from eBay's website.[1] The opinion was a leading case applying ‘trespass to chattels’ to online activities, although its analysis has been criticized in more recent jurisprudence.*

So web crawling differs from web scraping in several ways, but the idea is the same - pulling data out of  a website. The eBay case is a big one for internet law - Bidder’s Edge was enjoined from crawling eBay for auction data on the grounds of ‘trespass to chattel’ - *a tort whereby the infringing party has intentionally (or, in Australia, negligently) interfered with another person’s lawful possession of a chattel (movable personal property).*

Another additional consideration: While scraping may or may not be of dubious legality, websites set their own terms of service and can ban you from accessing them if you engage in practices that violate these terms. For example, from the YouTube terms:

*You agree not to use or launch any automated system, including without limitation, “robots,” “spiders,” or “offline readers,” that accesses the Service in a manner that sends more request messages to the YouTube servers in a given period of time than a human can reasonably produce in the same period by using a conventional on-line web browser.*

I guess the conclusion to draw here is that while the legality of scraping is pretty unclear, unless you’re willing to take on a company in court, err on the side of using an API or other acceptable method of accessing their data. Or don’t, you rebel. It’s up to you.

(Additional interesting reading from the New Yorker [here](http://www.newyorker.com/online/blogs/elements/2014/02/when-programmers-scrape-by.html))