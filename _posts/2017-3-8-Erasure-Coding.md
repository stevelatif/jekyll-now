---
layout: post
title: Getting Started With Erasure Coding!
---

So you want to learn about Erasure Coding??

A while back a colleague at work showed me a presentation https://web.eecs.utk.edu/~plank/plank/papers/FAST-2005.pdf about erasure coding in a storage system, which he had used as a spec to code up in python. 
This is a fun little project and I spent a couple of hours implementing a version in Python of the Reed Solomon codes. The presentation is a high level overview, but if you use the Python numpy module and guess some of the details you can come up with something that sort of works. Here is a little program that may or may not be a correct implementation but gives you an idea about what an implementation might look like:
https://github.com/stevelatif/erasure_coding/blob/master/erasure.py

This is a toy implementation - and its an incorrect implementation of the RS algorithm! So what exactly is wrong with it? The first couple of slides outline an implementation along the lines of
  * Convert your data into a vector of lenght n
  * Create an n*m matrix that looks like the n*n identity matrix on top of a mysterious matrix n*m B
  * Multiply out your data vectcor with mystery matrix B to create an m+n vector that consists of the data and the coding 
 
 Each of the rows of the vector can get written out to a node in your storage cluster. 
 We can now consider what happens when a node fails, say one or more of the the rows in the m+n vector dissapears. 
 
   * You delete the corresponding rows in the matrix B creating a new matrix B'.
   * Invert B' to create B'^-1 and multiply your reduced data and code vector by B'^-1. 
 
 This last step will give you back your original data. 
 The slides leave out some essential details that make the implementation of this scheme someone straightforward, but if you try to implement it as described you have to answer the following questions:
 
   * How do you do matrix arithmetic easily?
     * We'll cheat by using the numpy library 
   * I have arbitrary data that I'd like to work with, but matirces work on numbers
 - We'll convert the data into its ASCII representation and operate on that
  * To invert a matrix, it needs to be non singular
 - The slide has no information on this, you can guess a matrix, which is what I did to start, but what you really need is a Vandermonde matrix - more on this later.
   * If you are going to delete arbitrary rows from your matrix you will likely end up with non invertable matrix.
 - We'll get around this by always deleting m rows from the matrix.
  * Dealing with arbitrary data, you will likely end up trying to operate on a vector with a smaller dimension than your matrix.       


![_config.yml]({{ site.baseurl }}/images/config.png)

The easiest way to make your first post is to edit this one. Go into /_posts/ and update the Hello World markdown file. For more instructions head over to the [Jekyll Now repository](https://github.com/barryclark/jekyll-now) on GitHub.
