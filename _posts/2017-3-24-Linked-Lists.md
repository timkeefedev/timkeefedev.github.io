---
layout: post
title: Wondering about Linked Lists in JavaScript
---

I have been reading about implementing Data Structures in JavaScript.

I found the implementation of the Linked List data structure to be peculiar.  The structure appears to be very inefficient with respect to time complexity.

Unlike many subjects regarding JavaScript where you can find 100 different ways to implement something, all the resources I read used the same mechanism to build Linked Lists.  

They create a node object with “link to” properties and then, for some reason, nest that node instance within a chain of all the node instances.

	Ex. object = { ‘data’: ‘head’, ‘next’: { ‘data’: 7, ‘next’:{ ‘data’: 2, ‘next’: null } } }

Why nest the node? You have to do a linear time search through the chain to find the node.

Why not just use an object (key/value pairing) with the keys being the data and the values being the next node?

	Ex. object =   { ‘head’: 7,
                     ‘2’: null,
                     ‘7’: 2 }

Now your search is constant time...just use object [ item ]. 

So I figured:
Folks just used “the nest” due to tradition, since older languages actually require this nest structure.
I was just missing something.

Hence, I hatched up a project to simulate people clicking on an object (an article, song, product, person) and rank the most often clicked objects.  Tracking the ranks in a Linked List.  

When an object is clicked:
Search for the object id in the List.
If not found, then add it just before the tail.
If found, then update its click count:
Check to see if its position is now higher on the List.
If rank changes, then update its position to reflect its popularity.

I have put the code on my github page https://github.com/timkeefedev if you want to play around with it.

It consists of 7 files:

1.  app.js  -- the main page which times how long each structure takes to process the array
2.  arrayMaker.js  -- creates an array with an element that equates to the clicked object id
3.  DLLStandardOriginal.js  -- basically a copy of a typical Linked List constructor in js
4.  DLLStandardAdjusted.js  -- adjustments to make the program accept the array input
5.  DLLModified.js  -- my DLL constructor code using object keys to store the object id
6.  trackerStandard.js  -- creates a typical DLL instance and processes the array
7.  trackerModified.js  --  creates a modified DLL instance and processes the array

#### Things I expected:

The Modified structure would always be faster.
As the number of objects your tracking grows the time savings with the Modified structure would become, on average, increasingly larger.  Since the modified structure is “constant time” search (search for item and if found search for its updated position), remove, insert; while the Standard structure has a “linear time” search.

#### Several things I discovered:

1. The Standard structure appears to be faster than the Modified structure if you are tracking a small number of objects, say anything less than 10 objects or so.

2. I am not exactly sure of what is going on under the hood:
-  The performance times always got lower on subsequent passes using the same array data.
-  Near an indifference point (about 14 objects for 100,000 clicks), the performance time for the Modified structure on the second pass was always longer than for the Standard - even if it was slightly faster on the first pass.

Here is a sample run of app.js at (what I call) an indifference point (14 objects for 100,000 clicks):

| __14 objects for 100,000 clicks:__ | |

| __pass 1__ | |

|Modified: | 12.273ms	-- Times always decreased on subsequent passes. Why? |

|Standard: | 12.417ms|

| __pass 2__ | |

|Modified: | 9.725ms	  -- Modified time was always higher on pass2. Why? |

|Standard: | 9.547ms|

| __pass 3__ | |

|Modified: | 7.507ms |

|Standard: | 9.653ms |

| __pass 4__ | |

|Modified: | 7.410ms |

|Standard: | 9.754ms |

| __pass 5__ | |

|Modified: | 7.817ms |

|Standard: | 9.706ms |

[Finished in 0.3s]

Increasing the objects being tracked should result in:

- a stable time measurement for the modified linked list (which fails) and
- an increasing time measurement for the traditional link list. 

| __100 objects for 100,000 clicks:__ | |


| __pass 1__ | |

|Modified: | 16.181ms |

|Standard: | 33.645ms |

| __pass 2__ | |

|Modified: | 14.496ms |

|Standard: | 32.038ms |

| __pass 3__ | |

|Modified: | 12.117ms |

|Standard: | 31.426ms |

| __pass 4__ | |

|Modified: | 11.762ms |

|Standard: | 31.560ms |

| __pass 5__ | |

|Modified: | 12.236ms |

|Standard: | 31.585ms |

[Finished in 0.4s]

| __200 objects for 100,000 clicks:__ | |


| __pass 1__ | |

|Modified: | 21.855ms |

|Standard: | 58.850ms |

| __pass 2__ | |

|Modified: | 19.158ms |

|Standard: | 56.086ms |

| __pass 3__ | |

|Modified: | 16.965ms |

|Standard: | 56.470ms |

| __pass 4__ | |

|Modified: | 16.827ms |

|Standard: | 56.141ms |

| __pass 5__ | |

|Modified: | 16.607ms |

|Standard: | 56.095ms |

[Finished in 0.5s]


#### Conclusion
People must implement JavaScript Linked Lists with a nesting structure out of tradition.  It appears the modified version is typically more efficient, but
  1.  The modified structure is not constant time for some reason?
  2.  Why would the first pass always take significantly more time than subsequent passes with the same array?
  
I still need more understanding about how things work.  I have created more questions for myself due to: 
  1.  the misuse of console.time(),
  2.  ignorance related to how Javascript creates and destroys objects,
  3.  a nuance related to how my CPU technically works…
  4.  and/or...and/or...and/or...

