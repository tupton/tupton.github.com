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

This is the comment loop in comments.php.

``` php
    <?php if ($comments) : ?>

        <ol id="comments">

        <?php $i = 1 ?>

        <?php foreach ($comments as $comment) : ?>
            <li id="comment-<?php comment_ID() ?>" class="<?php echo get_comment_author_email() == get_the_author_email() ? ' blog-author' : $oddcomment ?>">
                <cite>
                    <span class="author"><?php comment_author_link() ?></span>
                    <span class="date"><a href="#comment-<?php comment_ID() ?>"><?php comment_date( $hemingway->date_format() . '.y' ) ?> / <?php comment_date('H.i') ?></a></span>
                    <span class="gravatar"><?php echo get_avatar( $comment, $size = '80'); ?></span>
                </cite>
                <div class="content">
                    <span class="number"><?php echo $i ?></span>
                    <?php if ($comment->comment_approved == '0') : ?>
                    <em>Your comment is awaiting moderation.</em>
                    <?php endif; ?>
                    <?php comment_text() ?>
                </div>
                <div class="clear"></div>
            </li>

            <?php $i++; ?>


        <?php /* Changes every other comment to a different class */
            if ('alt' == $oddcomment) $oddcomment = '';
            else $oddcomment = 'alt';
        ?>

        <?php endforeach; /* end for each comment */ ?>

        </ol>

     <?php else : // this is displayed if there are no comments so far ?>

      <?php if ('open' == $post->comment_status) : ?>
            <!-- If comments are open, but there are no comments. -->

         <?php else : // comments are closed ?>
            <!-- If comments are closed. -->
            <p class="nocomments">Comments are closed.</p>

        <?php endif; ?>
    <?php endif; ?>
```

This is the CSS section for my comments.

``` css
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
```
