---
published: true
layout: post
title: My Blog Has A New Home
---


Lately I have been spending way too much time maintaining my blog.  Something on the server was spiking the CPU causing my blog to become unresponsive throughout the day.  To be honest I just don't have time to investigate issue like that any more so it was time to look into a more hands off solution.  A colleague of mine, [Jonathan Kaufman](http://jkaufman.io/), told me he just moved his blog to GitHub and is using [GitHub pages](https://pages.github.com/).  My main concnern about doing this was that I need the export all my content (posts, images, comments, etc.) from WordPress and make sure it would work on GitHub.  However, if it was possible, it will allow me to not have to host my own WordPress server and it would save me some money each month :) 

![github.pages.jpg]({{site.baseurl}}/assets/media/github.pages.jpg)


I ended up coming across this [blog post](http://www.girliemac.com/blog/2013/12/27/wordpress-to-jekyll/) which described in detail how to move your WordPress blog to GitHub pages.  Following this post I was able to first sucessfully import and move all my blogs comments over to [Disqus](https://disqus.com/).  Then using the [Jekyll export plugin for Wordpress](https://github.com/benbalter/wordpress-to-jekyll-exporter) I was able to export all the other content from my blog into a Jekyll style blog that would work on GitHub pages.  After doing all that is was just about choosing a Jekyll template getting the content up on GitHub and then modify my DNS settings to point to GitHub instead of my WordPress server.  This all took me about a day to get working, which was not that bad.  If you are already using Disqus than I imagine it would take you even less time.

I don't suggest that everyone take the approach of ditching WordPress for GitHub.  For example, if you are not a technical person who is comfortable using Git and GitHub than this approach is not for you.  However I did find this site called [Prose.io](http://prose.io/) that is kind of like a CMS system for your GitHub repos.  I am actually writing this blog post right now using it.  So far it so good using it, but it's not exactly like using the WordPress editor.  That said I hope that this move is benefitical to me and my readers.
