---
author: admin
comments: true
date: 2014-09-03 18:29:59+00:00
layout: post
slug: cleaning-things-up
title: Cleaning Things Up
wordpress_id: 1012
categories:
- Blog
tags:
- custom post type
- rss
- wordpress
---

This has been something I've been trying to do for quite a while. I have my registered domain name that I use for my personal blog, which originally started as a primarily networking (IT related) blog. I wouldn't consider it popular by any means, yet it does get some traffic. So when I would write a non-networking related post, it felt quite out of place. I went back and forth on whether or not to break these out into separate sites, but I always felt like I wanted to keep them under my "umbrella".

What this boils down to is that I am a real live person (a single entity), and my domain name is reflective of that. However, I have multiple roles, I'm a husband, a father, I have a job ("professional" is debatable), I have other business ventures, hobbies, etc. I've wanted to separate these things because I don't think that people reading about my coffee shop ventures are interested in the latest technology Cisco has to offer. I used Wordpress.org as the platform for this site and I debated between separate sites (installs), Wordpress MultiSite, and just using plain categories. I like and have used the categories, but I still felt like it wasn't separated enough for my tastes and I didn't like the robertjuric.com/category/whatever URL format. Then I ready an article about custom post types, and I was enlightened.

Custom post types are a way to customize Wordpress into doing things it was never conceived to do. I've seen them used in Wordpress plugins so instead of just posts and pages, you can have custom types for instance called books, players, teams, events, etc. What I like about this is the shortened URL (robertjuric.com/networking/) as well as the separate management of the different posts types. The All Posts menu only shows my normal "posts" I have a separate menu for "networking" and "code".


### How I'm Doing It


Plugins, Home Page, Firehose

Creating custom post types is done in code, and can be done with a very quick and simple custom plugin. I decided to look around and settled on the existing [Types plugin](https://wordpress.org/plugins/types/). It was very simple to install and then I just added Custom Post Types for each of my desired categories. One thing I noticed was the whole singular/plural thing. Since networking isn't really a noun it was hard to pluralize, so what I did was add 'post' to the name, but I left the slug what I desired the URL to be. For instance, my networking Custom Post Type is called "Network Post", "Network Posts", and "networking" is the slug. Then I would check all of the 'Display Sections' so that I could turn off comments, or set a featured image.

That was great, now I have a custom spot on my single blog/domain name to separate my different roles. But I've had this blog for a few years now, how do I pull all of my old 'posts' into the new custom post types I've created? Enter the fairly obvious plugin [Post Type Switcher](http://wordpress.org/plugins/post-type-switcher/). This plugin adds a setting to the Edit and Quick Edit screens to configure which post type the post belongs to. What I did, since I already had my different posts assigned to different categories, was view all the posts in a category and bulk edit them to change the post type.

The next thing I had to do update my themes functions.php to show the custom post types on the home page. If you really wanted to keep things separate you could skip this step, and I'm probably going roll this back on my current home page as I get more generic content added to my primary blog. All you have to do is add this code to your themes function.php file. Just replace the "slug#" with the slugs you configured on your custom post types.

`// Added for custom post types, Show posts of 'post', 'slug1' and 'slug2' post types on home page
add_action( 'pre_get_posts', 'add_my_post_types_to_query' );
function add_my_post_types_to_query( $query ) {
if ( is_home() && $query->is_main_query() )
$query->set( 'post_type', array( 'post', 'slug1', 'slug2', 'slug3' ) );
return $query;
}`


### Menu and RSS Feeds


The next thing I wanted to do was make it easier for any readers I had left with an easy way to select which content that they wanted to view. What I have done is created a widget with simple URLs to the RSS feeds for each of my custom post types. This was quite easy to do by creating links to the feeds with this URL format: `http://example.com/feed/?post_type[]=slug1`

To get the menu items to appear I had to create custom URL menu items with links to the slugs (robertjuric.com/slug1).


### Conclusion


If you get creative you can really do some cool things with Wordpress and Custom Post Types. Well this was meant to be a quick introduction to Custom Post Types and I hope it served that purpose. It also is the final nail in the coffin of my different roles/sites. So if you're one of my fellow network geeks, update your RSS feeds so you stay up to date with my mediocre technical blogging.
