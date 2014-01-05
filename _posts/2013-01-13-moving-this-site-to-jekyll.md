---
layout: post
title: Moving this Site to Jekyll
comments: true
description: Or "A quest to find the absolute best speed and security for my website"
tags:
- jekyll
- meta
- programming
---
I've known PHP for four years, and I have fond memories of building this site using the PHP framework [Yii](http://yiiframework.com).  Initially, I was quite happy with it.  Yii is a relatively fast framework, and my site was pretty quick.  On my minimalist VPS server (single core CPU and 512 MB of RAM) I was averaging roughly 65 requests/sec with eAccelerator on Nginx.  That all changed when I decided to add a blog.

After several weeks of intense on and off coding, I launched it.  I took advantage of some cool features of Yii, and I was happy I got to work with PostgreSQL for storage.  However, I immediately noticed my blog was slow; on average, my blog supported 9 requests/sec with 100% CPU utilization.  That was unacceptable.

I started digging into why my blog was so slow, and I encountered some interesting things.  Using Yii's web log, I was able to determine how grossly inefficient Active Record is.  I was using Active Record extensively for my database interactions, and I was aware it was no speed demon.  But once I actually looked at all the queries that were made for loading a single post, I was astounded.  Loading a single post with comments took approximately 18 queries. Active Record is certainly convenient...but at what cost?!  I began to look into caching.

I had recently been using memcached at work, so I decided to go that route for my blog.  I had to make some changes to my code and how my queries were ran (I switched to using eager loading, instead of Yii's default lazy loading), and my changes helped.  A little.  With memcached enabled and all my code changes I got about 15 requests/sec.  A significant improvement, but still not nearly where I wanted it to be.  But I was frustrated with the little effect all my changes had made, so I shelved the project for a while.

Soon after, I began to get large numbers of spam comments on my blog.  I wrote an Akismet plugin for Yii that I abandoned before I completed the finishing touches, and just completely disabled comments on my blog.  I was fed up with how complicated my blog was becoming, and I didn't want to complicate it further by adding comment filtering.

So I began to think of alternatives.  I stumbled upon [Jekyll](http://jekyllrb.com/) one day, and thought it sounded interesting.  A completely static site would be incredibly fast, I could use variables and templates with Liquid, I could have a full blown blog with posts, templates, and comments (using Disqus), and I could have a super easy deploy method using Git.  Not to mention, my site and blog would be completely backed up with full version control.  That was extremely appealing to me.

I began to research Jekyll and all the functionality it has, and I soon began the process of switching everything over.  I'm quite happy with how it has turned out, Jekyll is a pretty awesome tool.  My new Jekyll site averages roughly 170 requests/sec, blog included.

I think I can have a lot of fun with this.
