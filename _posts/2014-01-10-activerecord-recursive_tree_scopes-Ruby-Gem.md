---
layout: post
title: activerecord-recursive_tree_scopes Ruby Gem
---

A few months back I was working on a project that needed some basic tree structured data. I'd used [acts_as_tree](https://github.com/rails/acts_as_tree) back when it was baked into Rails. It's fine and all, but if you want to query for all a node's ancestors or descendants you've got to recurse with indvidiual queries. Surely there's a way to make the wonderful PostgreSQL do something cooler.

I did some Googling and came across [a great post by Joshua Davey](http://hashrocket.com/blog/posts/recursive-sql-in-activerecord). He explains how to use PostgreSQL's built in support for recursive queries. Tree querying with a single SQL statement is a great thing. I wrote a gem a few months back that builds ActiveRecord scopes for tree traversal: [activerecord-recursive_tree_scopes](https://github.com/jwulff/activerecord-recursive_tree_scopes). 
