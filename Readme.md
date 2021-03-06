# Reaper

A simple module that turns DeprecationWarning into errors when your library
version or requirements change

It is notoriously difficult to keep track of deprecated function and clean dead
code regularly release after release. In particular, when marking a function as
deprecated, it is common to forgot mentioning the timeline of deprecation.

This forces you to actually pass a simple specification of when the deprecation
should happen, and dead code be removed.


## Use case

Deprecate component of your own library. 

Let's say the current version of reaper have bad API, I intend to remove for
0.5, but want to keep around for the time being. I can use the following:

```python

from reaper import deprecate

@deprecate("reaper", ">=0.5.0")
def throw_warnings(predicate, category, version, message):
    """
    This is too hard to use !
    """
    pass

```


This will show a `DeprecationWarning` to user calling the function in any
version from 0.1.x to 0.4.x, and raise a `DeprecationException` to any user
using 0.5.0-dev, or above.


## Future Plans


- Allow to raise early on Continuous Integration system to catch using
  deprecated API early in development process, and still allowing you to use
  code locally.

- Ability to do the same base on time/date 

- versions which is not a decorator/ context manager

- ability to mark version of dependency supported, and warn/raise when the
  minimal dependency list change:

```python

if deprecate(`tornado`, `<=4.3.1`):
    #do stuff
else:
    # do other stuff
```

The first branch (or actually the predicate) should warn/raise as soon as
something like `tornado>=4.4` appears in requirements.txt, that some dead code
has been found. 

