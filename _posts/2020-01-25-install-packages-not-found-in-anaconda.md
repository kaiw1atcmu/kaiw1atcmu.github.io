---
title: Install Packages Not Found In Anaconda
permalink: install_packages_not_found_in_anaconda.html
date: 2020-01-25
tags: [deep_learning]
published: false
---

Conda is an open source package management system and environment management system that runs on multiple OSes. It was
created for Python programs but it can package and distribute software for any language (e.g. C++/Java), with a focus on
data sciences. The conda package and environment manager is included in all versions of Anaconda, Miniconda and Anaconda
Repository.

According to its project website, Anaconda Distribution is the tool of choice for solo data scientists who want to use
Python or R for scientific computing projects, which provides integrated package installation and environment management
utilities supported by Conda, for Python IDEs like PyCharm. However, not all Python packages are allowed to install from
within Anaconda, such as jieba, a frequently used natural language processing package for Chinese mandarin.

## The Naive Solution
A naive solution is to install packages not found in Anaconda on the commandline prompt using `pip install jieba` or
`conda install jieba` and import the packages using `import jieba`. However, this method does not work consistently with
Python projects created with Conda, since the repository associated with Conda might lack the package you wish to
install, or you might get an error reported afterwards when trying to import the package installed.

## The Alternative Solution
The alternative solution is to:

1) create a new Python project environment using the Conda interpreter (where the default choice is a Python interpreter
without a virtual environment).

<img src="{{ "images/20200125-2.png" }}" alt="conda"/>
_Figure 1: Create new a Python project environment using the Conda interpreter._

2) download the desired package file `jieba-0.42.1.tar` from its official website or other trusted third-party sources,
and unzip it into Anaconda's pkgs directory whose full path may be OS-dependent. On my laptop, it is located under the
directory `C:\ProgramData\Anaconda3\pkgs`. If its parent folder `C:\ProgramData` is hidden, you need to have it shown.
Pay attention to the file hierarchy and make sure the unzipped folder is not improperly nested in that a subfolder
shares the name with its parent, such as `C:\ProgramData\Anaconda3\pkgs\jieba-0.42.1\jieba-0.42.1`. On my laptop, for
example, the uncompressed folders are organized like:

```python
C:\ProgramData\Anaconda3\pkgs\jieba-0.42.1\jieba
C:\ProgramData\Anaconda3\pkgs\jieba-0.42.1\test
C:\ProgramData\Anaconda3\pkgs\jieba-0.42.1\setup.py
...
```

3) open the Anaconda prompt under the Python project for which you wish to install the package.

<img src="{{ "images/20200125-1.png" }}" alt="anaconda"/>
_Figure 2: Suppose we wish to install the package for project dl. Click Open Terminal under the environment for project dl._

4) change current directory to the unzipped jieba folder using

```python
cd C:\ProgramData\Anaconda3\pkgs\jieba-0.42.1
```

and execute jieba's installer

```python
python setup.py install
```

5) open the Jupyter notebook or PyCharm, checking whether an import error is reported in doing so:

```python
import jieba
```

It should work out fine if you do not encounter any issues by importing the package installed.

{% include links.html %}
