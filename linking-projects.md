# Linking Pompeii Projects for Development

In the case of the ```lib``` files, specifically, sub-projects are imported into 
their parents' by creating a symlink to the standard ```src``` directory with
the name of the module as you would see it on a python ```import``` line.

```
ln -s pompeii/lib/pompeii.lib.core/src/ pompeii/lib/pompeii.lib.core/plib_core
```

and then add the path to that file-like object to 
```resources/virtualenv/lib/python2.7/site-packages/plib_core.pth``` or any 
other ```*.pth``` file you want to add to that folder.

Then ```deactivate``` if neccessary and the virtualenv will load the contents of
the folder pointed to by the symlink as a package named the same as the link
name.

## Reasoning

Doing this allows the virtualenv to update these linked packages 'live' since 
they are loaded from the disk, not from a compiled egg file.  WARNING: this
could cause complexities when maintaining multiple VCS repositories, so use with
caution and commit everything and often.

Given the disclaimer, though, it allows the virtual environment to simulate 
the availability of the package as if it had been pip installed without requiring
any boilerplating at all.  The reasoning here is that I intend to simultaneously
be able to develop against "the pompeii library" without having to keep everything
in the same path for the sake of git.  

This allows the file structure to be what
git needs it to be and the Python packages to be available regardless of their
on-disk location.  In addition, by isolating the change to the virtualenv and 
a symbolic link (which is committable), we keep the separation of those two 
functions split in the best place; the development environment only.

## Alternatives

During the process of coming to this point, I have used various configurations
of git submodules, pip's ability to install as editable (from a requirements.txt
file), and strange placements of ```__init__.py``` files.  In the end, this has
been the best solution.

## Context

Python submodules can be broken out into separate Git repositories, with this
method, such that we can encourage the atomization of modules and their
development.

Consider: if the ```*.pth``` file and the symbolic link have to be comitted to
VCS anyway, and one of those resides in the ```site-packages``` folder of the
virtual environment anyway, would it be easier to just link as:
```ln -s pompeii.lib.core/src/ resources/virtualenv/lib/python2.7/site-packages/plib_core```
to begin with.  This is probably a task for LPM to manage, though.