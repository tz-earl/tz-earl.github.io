---
layout: post
title:  "Within a Jupyter Notebook - ModuleNotFoundError: No module named 'numpy'"
date:   2019-05-14 00:00:00 -0700
categories: 
---
I had created a new virtual environment to run python 3 via the `virtualenv` command, then cd into its directory and
activated it. I knew that I would need the numpy package, so I did `pip3 install numpy`.

However, when I launched a jupyter notebook and did `import numpy as np` I got the error `ModuleNotFoundError: No module named 'numpy'`. What?!

In the terminal window I ran `python` to get an interactive python window and typed in the import statement, which worked fine. 

Eventually, I realized that it was something to do with jupyter, not with python. `which jupyter` showed that I was running `/home/earl/.local/bin/jupyter`, in other words, a copy of jupyter from outside the environment. Consequently, python packages were being searched from a `sys.path` variable that did not include paths within the environment's directory. 

The fix I used was to also install jupyter.

So don't forget to  `pip3 install jupyter` in your virtual env, too.
