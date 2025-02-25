---
date   2010-06-08 17:00:00
---

The more time that passes between writing code and finding the bug, the harder it is to fix it. For that reason, we should strive to tighten the feedback loops in our development process. 

From best to worst.

# Compiler
By far the quickest way to get feedback is through the compiler. Can you build your API in such a way that the compiler can enforce good behaviour?
This is even easier if you're using a statically typed language 

# Unit tests

# Automated UI/API tests

# Early runtime feedback (ui or exception if trully exceptional)
not right for, but for example if developer tries to map wrong entity in database (not sure if good eample)
google right time to throw exceptions

# During manual testing