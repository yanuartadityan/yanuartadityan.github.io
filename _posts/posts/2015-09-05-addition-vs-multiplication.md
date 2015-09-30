---
layout: post
title: "Addition vs Multiplication"
modified:
category: blog
excerpt: "No need for such an argue for this problem"
tags: [sweden, chalmers, algorithm]
image:
  feature: 
  credit: 
  creditlink: 
comments: true
---

OK, let's settle this. If you're given a question, which is faster, between addition and multiplication? Or in more formal concise way of problem set, given `x, y` be two `integers`. Which one is easier, an `addition` or `multiplication` for those two integers?

I'd rather believe that people will spontaneously chose to add is faster than to multiply. But in what sense people think that way? A conceptual framework is needed to motivate this answer. And I'll help you on this post.

We're going through some definitions here. `Addition` and `multiplication` are examples of **problems**. Some inputs, in this case `x, y` are the inputs, or precisely, instances for which the problem has to solve. In principle, a problem has infinite instances that should be able to be solved.

Problems are often solvable by **algorithms**. Intuitively, an algorithm is an instruction saying how to do the calculation that solves the problem for every possible instance. It must be precise and unambiguous, such that it can be delegated to a machine. The steps of an algorithm must consist of simple operations which can be done mechanically, without human intervention or appeal to human intelligence. They must not leave any room for subjective interpretations.

Computer programs are nowadays the way to execute algorithms, but algorithms existed already long before the advent of computers. The word is derived from the name *al-Khoarizmi*, an Persian scholar (780–850) who wrote an early textbook on arithmetic calculations. Much earlier, various algorithms were already known to ancient mathematicians.

Examples of algorithms are the well-known “school methods” for adding or multipling two numbers on paper. These methods fulfill all the above criteria. They specify which simple operations with digits we have to do in which order, and how these partial results have to be combined to the correct final result.

We stress that an algorithm and a computer program is not the same thing! A program implements an algorithm, i.e., it realizes an algorithm in a specific programming language. But an algorithm as such is an abstract mathematical entity. This distinction is more than just academic hair-splitting, rather, it has two major practical consequences.

OK, then which one is the answer mister?

The answer lies on what we know with **Time Complexity**. Good luck study on this guys. Welcome to the Algorithm class.