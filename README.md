# [Coarray Fortran Support Status](#coarray-fortran-support-status) 

 *  [Compilers]
     * [OpenCoarrays-Aware Compilers]
     * [Non-OpenCoarrays-Aware CAF Compilers]
     * [Non-CAF Compilers]
 *  [OpenCoarrays Libraries]
     *  [libcaf_mpi]
     *  [libcaf_gasnet]
     *  [Unsupported libraries]
 *  [Known Issues]
 *  [Basic Communication Support]
 *  [Other Feature Coverage]

<a name="end-user-installation">
## Compilers##
</a>

The OpenCoarrays CMake build and test scripts detect the compiler's identity and version number and the operating system and  use that information to build functionality into the [OpenCoarrays Libraries] that is appropriate for the chosen compiler.  We target three categories of compilers:

<a name="opencoarrays-aware-compilers">
* **[OpenCoarrays-Aware Compilers]**</a> accept CAF syntax and generate calls to the OpenCoarrays application binary interface ([ABI]).  Current OpenCoarrays-aware compilers include
* **[Non-OpenCoarrays-Aware CAF Compilers]** accept CAF syntax but do not generate calls to the OpenCoarrays ABI.  Users can mix the compiler's native CAF support with calls to Fortran wrappers for the OpenCoarrays [ABI].  Users acess the wrappers via use association with the [opencoarrays] module: for example, `use opencoarrays, only : co_sum`.  In one common use case, the compiler handles all point-to-point communication via bracketed codimensions (`a = b[1]`), while OpenCoararys handles all collective communication via the collective subroutines proposed for Fortran 2015 in the draft Technical Specification [TS 18508]

* [Non-CAF Compilers]
* [Cray Compiler Environment]: fully Fortran 2008 compliant; also supports the proposed Fortran 2015 collective subroutine
[GNU Fortran] 5.1 or later: supports most Fortran 2008 CAF features and some proposed Fortran 2015 CAF features   

For the status of the compiler-side coarray support in GCC/gfortran,  
see https://gcc.gnu.org/wiki/Coarray and below.  User submissions of 
bug reports are welcomed via the [Issues](https://github.com/sourceryinstitute/opencoarrays/issues) page.  
To contribute code, including documentation and tests, see [CONTRIBUTIONS](./CONTRIBUTIONS).

OpenCoarrays builds the following libraries
-------------------------------------------

* LIBCAF_MPI (Default CMake build): Uses the Message Passing Interface  
  (MPI) 3.0 one-sided communication API, which exploits a hardware  
  platform's native support for Remote Direct Memory Access (RDMA) if 
  available.

* LIBCAF_GASNet (Advanced Make build): Experimental version that is 
  currently out-of-date but might exhibit higher performance than MPI  
  on platforms for which GASNet provides a conduit tuned for the .  
  plaform Contact the [OpenCoarrays Google Group](https://groups.google.com/forum/#!forum/opencoarrays) 
  for further information.
  
* Also included: unsupported Single (sequential) and ARMCI libraries.
  These are not maintained and are provided primarily for historical 
  reasons or as a starting point for future projects that choose to 
  update and build upon these implementations.


Known Issues
------------
* The extensions in the [opencoarrays module](./src/extensions/opencoarrays.F90)  
  provide some coarray capabilities to users of non-OpenCoarrays-aware 
  compilers.  For a listing of the limitations, execute the "caf" bash 
  script with no arguments.  CMake builds the "caf" script in the "bin"
  directory of the OpenCoarrays installation path.  The most significant 
  limitation is the lack of support for square-bracket syntax. The 
  extensions are currently
  usefulf in applications for which the [proposed Fortran 2015 collective 
  subroutines](http://isotc.iso.org/livelink/livelink?func=ll&objId=17181227&objAction=Open) 
  cover the applications communicaiton needs or applications or
  applications for which another communication library provides any 
  communication needs not covered by the proposed collectives
* Efficient strided array transfer support is available only for intrinsic types.
* Efficient strided array transfer support is not available for remote-to-remote transfers.
* Overwriting a coarray with itself is not managed efficiently for strided transfers.
   
Basic Communication Support
---------------------------

* Mostly complete
* Vector subscripts not yet implemented
* For character assignments, some issues with padding and
  character kind conversions exist.
* For array assignments, some issues with numeric type conversion exist.

Compiler-side (GCC) issues
--------------------------
* Allocatable coarrays and derived-type coarrays are supported but 
  derived-type coarrays containing allocatable/pointer components are not 
  yet handled properly.
* Problems exist with array access to a corray in combination with a scalar
  component access: coarray(:,:)[i]%comp.
* Internal compiler error (ICE) with nonallocatable polymorphic coarrays and
  ASSOCIATE or SELECT TYPE

Atomics
-------

* Implemented in LIBCAF_MPI.

Locking
-------

* Implemented in LIBCAF_MPI.

Critical Sections
-----------------

* Implemented in LIBCAF_MPI.

Collectives
-----------

* Implemented in LIBCAF_MPI without compiler-side (GCC) support for 
  finalization or allocatable components.
* A limited set of intrinsic types and kinds are supported, e.g., 
  integer(c_int), real(c_double), character(kind=c_kind), logical, 
  and complex(c_double).  Users are invited to submit pull requests with
  support for additional kinds.
  
To-Do List
----------
* Additional tests and documentation.
* Improvement of error handling and diagnostics, including but not
  limited to filling the ERRMSG= variable in case of errors.
* Providing a diagnostic mode with run-time consistency checks.
* Better integration with the test cases of GCC.  For more information,
  see the GCC source code files in gcc/testsuite/gfortran.dg/, 
  in particular, the "dg-do run" tests in coarray*f90 and coarray/).

[Non-OpenCoarrays-Aware CAF Compilers]: #non-opencoarrays-aware
[opencoarrays]: ./src/extensions/opencoarrays
[ABI]: https://gcc.gnu.org/onlinedocs/gfortran/Function-ABI-Documentation.html#Function-ABI-Documentation
[Compilers]: #compilers
[OpenCoarrays-Aware Compilers]: #opencoarrays-aware-compilers
[Other CAF Compilers]: #other-caf-compilers
[Non-CAF Compilers]: #non-caf-compilers
[OpenCoarrays Libraries]: #opencoarrays-libraries
[libcaf_mpi]: #libcaf-mpi
[libcaf_gasnet]: #libcaf-gasnet
[libcaf-x]: #libcaf-x
[Known Issues]: #known-issues
[Basic Communication Support]: #basic-communication-support
[Other Feature Coverage]: #other-feature-coverage
[TS 18508]: http://isotc.iso.org/livelink/livelink?func=ll&objId=17181227&objAction=Open
