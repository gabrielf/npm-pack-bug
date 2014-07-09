npm pack bug
============

npm pack yields inconsistent results where files are sometimes included and sometimes not.

To reproduce the bug run:

    while npm pack && tar -tf npm-pack-bug-0.0.0.tgz | grep generated  ; do : ; done

This will run npm pack and look for expected files in the resulting tar file. It will run
until these files are not present (the bug). This takes up to a minute but does happen
on OS X.

The files we look for emulate generated files since these are ignored in .gitignore,
to avoid being checked in, but present in package.json => files since they should be
included in the tarball, which is a state which triggers the bug (there might be other).

The bug seems to be related to the precense or absence of the .gitignore and .npmignore file 
and/or the precense of the files attribute in package.json

To avoid the bug (at least for this repo) just add an empty .npmignore file. This is OK since
we've specified the files that should go into the package in package.json => files. We don't
need to also specify what we don't want in .npmignore