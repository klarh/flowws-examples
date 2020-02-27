
[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/klarh/flowws-examples/master)

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/klarh/flowws-examples/blob/master/)

# Introduction

This repository demonstrates several workflows for simulation,
visualization, and analysis using
[flowws](https://github.com/klarh/flowws).

To launch an interactive notebook server using mybinder.org, use the
link above; otherwise, install the prerequisites below to run
locally. Google colab is also supported somewhat, but see the caveats
in the section below.

# Prerequisites

Note that not all of these are required for all notebooks. Everything
except for hoomd is available on the PyPI and is available using `pip
install -r requirements.txt`:

- flowws
- flowws-analysis
- flowws-freud
- freud-analysis
- gtar
- [hoomd-blue](https://hoomd-blue.readthedocs.io/en/stable/installation.html)
- hoomd_flowws
- matplotlib
- plato-draw
- pyriodic-aflow
- pyside2
- vispy (if using a notebook, make sure to follow the [installation instructions](http://vispy.org/installation.html))

# Running on Google Colab

Example notebooks can also run in [Google
Colab](https://colab.research.google.com), although it can be annoying
to install prerequisites like HOOMD on a colab notebook. Note that
interactive plato backends are unlikely to work in colab. You can use
the following code in each notebook to install prerequisites:

```
!pip install -U -q PyDrive

# download compiled hoomd release
from pydrive.auth import GoogleAuth
from pydrive.drive import GoogleDrive
from google.colab import auth
from oauth2client.client import GoogleCredentials
auth.authenticate_user()
gauth = GoogleAuth()
gauth.credentials = GoogleCredentials.get_application_default()
drive = GoogleDrive(gauth)
fileId = '1sbyj-ruKLtEK-9qVlsXSJU0hxCHf_blD'
downloaded = drive.CreateFile({'id': fileId})
downloaded.GetContentFile('hoomd.tar.gz')

# extract hoomd
!mkdir -p "$(python -c 'import site;print(site.USER_SITE)')"
!tar xC "$(python -c 'import site;print(site.USER_SITE)')" -f hoomd.tar.gz

# install other dependencies
!apt-get install povray
!pip install flowws-analysis flowws-freud freud-analysis gtar hoomd-flowws matplotlib plato-draw pyriodic-aflow

# hoomd won't be able to be imported the first time this cell is run, so
# restart the process.
try:
    import hoomd
except ImportError:
    exit()
```

**Note:** The hoomd package is compiled with GPU support; select a GPU
runtime ("Runtime -> Change runtime type") to be able to correctly import it.
