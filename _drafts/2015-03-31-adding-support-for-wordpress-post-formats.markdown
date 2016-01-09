---
author: admin
comments: true
date: 2015-03-31 21:37:34+00:00
layout: post
slug: adding-support-for-wordpress-post-formats
title: Adding Support for Wordpress Post Formats
wordpress_id: 1121
categories:
- Wordpress
tags:
- origin
- post format
- theme
- wordpress
- wordpress.org
---

I use the AlienWP Origin theme on this site, and I'm quite happy with it. I've recently changed the way I share things online (I cancelled my social media accounts), so I wanted a way to share other formats on my website, sort of like Tumblr format. I thought the built-in Wordpress Post Formats would be a great way to accomplish this. Until I realized my Origin theme doesn't support them. So what I did is extract the theme to create a child theme (something I should have done some time ago), finally after some searching and kludging I was able to find a working solution. The information is easily found online, in various formats, but for my lack of Wordpress experience it took a little trial and error to find the relatively simply solution.



### Child Theme


The first thing I did was create a child theme from the Origin them. They offer this on their website but you have to pay for a support subscription. I'm sorry I'm not going to pay for a blank child version of a theme you offer for free under the GPL. I did leave all references to AlienWP and their Origin theme. To create the child theme I created a file `style.css`:

    
    /**
    * Theme Name: Origin Child
    * Theme URI: http://alienwp.com/themes/origin/
    * Description: Origin is a minimalistic theme with responsive layout. The theme supports custom header and background, plus a number of extra settings: link color picker, typography options, and a nice selection of Google fonts - built into the new WordPress theme customizer.
    * Version: 0.5.6
    * Author: AlienWP
    * Author URI: http://alienwp.com
    * Template: origin
    * Tags: flexible-width, theme-options, threaded-comments, microformats, translation-ready, rtl-language-support, one-column, two-columns, right-sidebar, sticky-post, custom-background, featured-images
    * License: GNU General Public License v2.0
    * License URI: http://www.gnu.org/licenses/gpl-2.0.html
    *
    */



Next I created a `functions.php`:

    
    <?php
    add_action( 'wp_enqueue_scripts', 'theme_enqueue_styles' );
    function theme_enqueue_styles() {
        wp_enqueue_style( 'parent-style', get_template_directory_uri() . '/style.css' );
    	wp_enqueue_style( 'child-style',
            get_stylesheet_directory_uri() . '/style.css',
            array('parent-style')
        );
    }
    ?>



I also had to copy the `index.php` file to the child folder. Because I don't like constantly updating files, you can go ahead and create 2 empty files for now, `content.php` and also `content-quote.php`, or you can create these as you go and upload them individually to your theme folder.

The rest I'm going to generalize for the sake of simplicity, it easily applies to any theme.


### Adding Post Format Support


The first step necessary is to add support for the post formats to your `functions.php` file:


    
    add_theme_support( 'post-formats', array( 'aside', 'gallery', 'link', 'image', 'quote', 'video' ) );
    	add_post_type_support( 'page', 'post-formats' );



Next go into your `index.php` file and **extract** the entire loop. Mine started with `<?php while ( have_posts() ) : the_post(); ?>` and ended with `<?php endwhile; ?>`. So cut out everything between those lines and paste it into your `content.php` file. This is used when a post has a non-standard post format specified.

Then go into your `index.php` file again and where that empty space is where you removed your original loop code, paste this new code:


    
    <?php get_template_part( 'content', get_post_format() ); ?>



This replaces your loop with a call to individual content-_whatever_.php formats, or calls the default content.php format depending if it's specified or not. Next for each format type you will need a content- file. For example we will use `content-quote.php`:

    
    <?php
    		/**
    		* The template for displaying posts in the Quote post format
    		*
    		*/
    		?>        
                <h2><blockquote><?php the_content(); ?></blockquote></h2>



Believe it or not it really is that simple. At this point you should be able to go to Add New post and select the Quote format, when you publish it you should see it on your front page with the specialized styling. From here you create content files for each post format and style them as you wish. If you have any questions hit up the comments.

EDIT:
To get the next post to show up on my summary front page I had to make some changes to the `content-quote.php` file. I'm not sure if this is core, or just related to my origin theme, but I thought I would share:

    
    <?php
    /**
     * The template for displaying posts in the Quote post format
     *
     */
    ?>      
    <?php do_atomic( 'before_content' ); // origin_before_content ?>  
    <div id="post-<?php the_ID(); ?>" class="<?php hybrid_entry_class(); ?>">
    	<?php do_atomic( 'open_entry' ); // origin_open_entry ?>
            <div class="entry-summary">
                    <h2>
            	<blockquote:before>
    		<?php the_content(); ?>
    	        </h2>
            </div><!-- .entry-summary -->
    	<?php do_atomic( 'close_entry' ); // origin_close_entry ?>
    </div><!-- .hentry -->
    <?php do_atomic( 'after_entry' ); // origin_after_entry ?>
