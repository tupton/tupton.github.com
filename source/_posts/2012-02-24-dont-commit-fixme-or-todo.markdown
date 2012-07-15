---
date: '2012-02-24 12:41:13'
layout: post
slug: dont-commit-fixme-or-todo
status: publish
title: Don't Commit FIXME or TODO
wordpress_id: '721'
categories:
- Programming
- Rant
---


<code data-language="php">
    function getObject($id) {
        $object = null;
            
        // TODO Do we need to validate username?
        $objects = $this->db->retrieveForUsername($this->username);
            
        // FIXME Write `retrieveForIdAndUsername` instead of iterating here
        foreach ($objects as $o) {
            if ($o->id() == $id) {
                $object = $o;
                break;
            }
        }
            
        return $object;
    }
</code>

This shouldn't happen. I'm not talking about the comically bad code – it's a contrived example, but it proves the point – but about the little "notes" left by whoever committed this.

Do we need to validate the username? Probably. Find out, and take the necessary action. If you know your codebase well, it should take about ten seconds to know the answer.

The `FIXME` is a bit more time-consuming, but the payoff is greater. If you leave this `FIXME`, I'm going to assume one of two things: you're a lazy programmer, or you're a bad programmer. You're lazy because you can't be bothered to write something correctly or you're bad because you don't know *how* to write it correctly. [^bad-dev]

[^bad-dev]: One could argue that being aware of bad code is better than nothing, but that's fluffy. If you're aware of a problem, you're probably smart enough to do something about it.

Of course, use `TODO` or `FIXME` while you're developing to keep a list of tasks that you have to finish before your feature is complete. Just don't commit them to where other developers can see them and *certainly* don't merge them to trunk.

If you're lazy with that code now, there's no way you're going to overcome that laziness and come back to fix it later.
