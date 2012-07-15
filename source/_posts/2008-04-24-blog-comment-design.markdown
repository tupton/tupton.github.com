---
date: '2008-04-24 16:24:47'
layout: post
slug: blog-comment-design
status: publish
title: Blog Comment Design
wordpress_id: '206'
categories:
- Code
- Design
tags:
- comments
- css
- php
---

I updated my blog comments from the [Hemingway] [] default to a more iconic design.

[hemingway]: http://warpspire.com/hemingway

A [post about blog comments] [comment design] that I saw a few weeks ago inspired me to use the "big number" idea for my own comments.

[comment design]: http://css-tricks.com/better-ordered-lists-using-simple-php-and-css/

I also enabled [Gravatars] [] and have highlighted my own comments when I make them. I have posted the comment loop and the relevant comment CSS in case anyone else wants to use this idea.

[gravatars]: http://en.gravatar.com/

This is the comment loop in comments.php. You can [download it as a PHP snippet] [php].

[php]: http://www.thomasupton.com/wp/wp-content/uploads/2008/04/comment-snippetphp.zip

    

    	



    	

    	
    		
  1. 
    			
    				
    				date_format() . '.y' ) ?> / 
    				
    			
    			


    				
    				comment_approved == '0') : ?>
    				_Your comment is awaiting moderation._
    				
    				
    			


    			


    		


    		


    	

    	

    	

     

      comment_status) : ?>
    		

    	 
    		
    		

Comments are closed.



    	
    

This is the CSS section for my comments. You can [download it as a CSS snippet] [css].

[css]: http://www.thomasupton.com/wp/wp-content/uploads/2008/04/comment-snippet.css

    /* Comments */
    ol#comments li .content span.number {
    	position: relative;
    	top: 0px;
    	left: 0px;
    	font-size: 10em;
    	color: #c7c7c7;
    	float: right;
    }

    ol#comments li {
    	padding: 10px 0;
    }

    ol#comments li.blog-author {
    	background-color: #ddd;
    }

    ol#comments li.alt .content {
    	background-color: #ddd;
    }

    ol#comments li cite span.date a {
    	text-decoration: none;
    }

    ol#comments li cite span.gravatar {
    	margin-top: 13px;
    }

    ol#comments li cite span.gravatar img {
    	padding: 3px;
    	border: 1px solid #ccc;
    }
