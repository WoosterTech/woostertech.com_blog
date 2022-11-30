+++
categories = ["TIL"]
date = 2022-09-01T07:00:00Z
description = "test description"
draft = true
image = "/images/sphinx-logo.png"
tags = ["django", "python", "sphinx"]
title = "Make Sphinx Work With Django Project"
type = "post"

+++
Was having a heck of a time trying to get my Sphinx documentation to properly `automodule` my project. Eventually found [this](https://stackoverflow.com/a/34462027/19251950) answer on a stackoverflow question, so here’s how they did it (so simple!).

```
from django import setup as django_setup

sys.path.insert(0, os.path.abspath('../..'))
print("Path: {}".format(sys.path[0]))

os.environ['DJANGO_SETTINGS_MODULE'] = 'web_project.settings'

django_setup()
```

There are a few pieces of magic here. First, the sys.path.insert is done to the root of the project. That allows for everything else to be relative properly. That’s imperative for the environment variable.

Next, the module for the settings can be specified relative to that location. So clean!

Last, we let Django do the setup of everything for us. All of the apps get setup properly, as well as the settings.

The only issue I have is that when using any of the auto tools, the entire path gets listed. In my project, that turned into purchases.models.models_data.getsimpleattrs().