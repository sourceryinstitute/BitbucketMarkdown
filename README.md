
Improved Installation, Testing, and Workflow
==================================

New features and development practices
------------------------------------------------------

* Adoption of the [GitHub Flow](https://guides.github.com/introduction/flow/) workflow.
* A formalized change review policy, including using the [Travis-CI](https://travis-ci.org) continuous integration tool to run our test suite on Linux and OS X after every push.  
* Expanded testing, including installation script testing via Travis-CI and code coverage reporting via [codecov-io](https://github.com/codecov-io).
* Improved documentation, including fixed links and on-demand documentation PDF-generation on GitHub.  (Broken links were detected automagically with a broken-link checking tool).
* Improved detection of the version number of each prerequisite.
* A gfortran version requirement of 5.3.0 in order to support the Fortran 2015 co_reduce collective.
* Fixed building of CMake on OS X when necessary.

Improvements
-------------------

* Added the GCC prerequisite m4 to the dependency tree in install.sh. 
* install.sh now prints download instructions for missing prerequisite packages if the platform does not have the default download mechanism (which can be ftp or wget).

As before, a default installation can be obtained simply by typing “./install.sh” with the present working directory set to the top level of the OpenCoarrays source directory.   By default, the script installs OpenCoarrays and any missing prerequisites inside the install_prerequisites subdirectory.  The script downloads, builds, and installs the following prerequisite packages only if the prerequisite is needed to build a tree ancestor:

    opencoarrays
    ├── cmake-3.4.0
    └── mpich-3.1.4
        └── gcc-5.3.0
            ├── flex-2.6.0
            │   └── bison-3.0.4
            │       └── m4-1.4.17
            ├── gmp
            ├── mpc
            └── mpfr
  
