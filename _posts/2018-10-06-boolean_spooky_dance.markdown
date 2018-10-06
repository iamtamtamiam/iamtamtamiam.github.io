---
layout: post
title:      "Boolean != Spooky Dance"
date:       2018-10-06 16:03:05 +0000
permalink:  boolean_spooky_dance
---


Booleans....intial thoughts on the word: "boo" = something spooky..."lean" = slim...maybe tilting the body...like a dance (*Now lean back, lean back, lean back, lean back*).. Turns out, it has nothing to do with scary dances or lean ground beef.

![](https://images.rapgenius.com/66ee01fbc08867775b7b199f78963e49.472x316x10.gif)

Quick google and it turns out *boolean* actually comes from the man who invented Boolean Logic in the 19th century – George Boole. Thanks to this guy, true/false logic was brought to the programming world. 

**Boolean Cheat Sheet** (Learn.co does it pretty well, but I'll try to simplify it even more)

2 key points to keep in mind:
* "=" is to set a variable;  "==" is for equalities
* "!" reverses the logical state; for example, "!=" is read as "not equals"

Boolean operators:
* "&&"  ("and" ) -  both values of either side must be true to evaluate true
* " || " ("or") - only one value on either side must be true to  evaluate  true


**Example lab completed with booleans**

Objective: in order for the move to be valid, it must be on the board and the position can't be taken
Thinking process:
* must be between indexes 0-8. So "index.between?(0.,8)
* should use an && because two conditions have to be true 
* already created #position_taken...it evaluates true if the position is taken...but for #valid_move, I need the position to be empty...
* use the ! to reverse #position_taken; in a sense, changed position_taken to position not taken
* okay lets put it all together:

```
def valid_move?(board, index)
  index.between?(0,8) && !position_taken?(board, index) 
end
```

Boom, it worked. Basic boolean logic at its finest. 
