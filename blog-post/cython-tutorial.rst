SciPy 2013 Cython Tutorial
-------------------------------------------------------------------------------

Thanks to everyone who attended the Cython tutorial [tut]_ at this year's
SciPy conference, and thanks to the conference organizers and tutorial chairs
for ensuring everything ran smoothly.   The enthusiasm of the students came
through in their questions, and there were several good conversations after
the tutorial throughout the week.

If you want the tutorial experience from the comfort of your couch, you can
download the tutorial slides, exercises, and demos [content]_, and follow
along with the videos [tut]_.  Please read the setup instructions on the
tutorial description.  The easiest way to satisfy the requirements for the
tutorial is to download and install an existing scientific Python
distribution, such as Enthought's Canopy.  You will need Cython version 0.19
or greater.

Tutorial Highlights
-------------------------------------------------------------------------------

Cython is a language for adding static type information to Python with the
objective of improving Python's performance; it is a compiler (the `cython`
command) for generating Python extension modules; and it helps wrap C and C++
libraries in a nice and Pythonic way with a minimum of overhead.  These three
aspects of Cython are of course tightly integrated with each other and
shouldn't necessarily be thought about in isolation: is it common to have
Cython code that is intended to both speed up a pure-Python algorithm and that
also calls out to Cython-wrapped C or C++ libraries.

As compared with some of the newer Python JIT compilers that are on the rise,
Cython is relatively mature -- not SWIG mature, but it has certainly been
around long enough that it has grown features beyond its core functionality:
annotated source files for compile-time performance profiling, runtime
profiling that integrates with Python's profilers, cross-language debugging
capabilities, integration with IPython and the IPython notebook via the
`%%cython` magic and other magic commands, the `pyximport` import hook
support, Python 2 and Python 3 support, parallelization support via OpenMP,
and others.  I wanted this tutorial to touch on several of these extra
capabilities which help make Cython easier to use.

The tutorial was in the advanced track because I wanted to dive into newer and
more advanced Cython features, especially typed memoryviews [memviews]_.
Typed memoryviews are Cython's interface to PEP-3118 [3118]_ buffers, the new
buffer protocol, which is a protocol for accessing and passing around
(possibly strided) blobs of memory without copies.  This is, of course, of
considerable interest to scientific computing audiences for whom non-copying
array operations are essential.  NumPy arrays support this protocol, and are
the primary object used with this protocol.  Cython's syntax for typed
memoryviews is nice, and taking a slice of a typed memoryview is another typed
memoryview and does not copy memory.  

I was able to cover typed memoryviews towards the end of the tutorial, but
didn't have quite enough time to demonstrate their full power.  Typed
memoryviews are in every way superior to the existing numpy buffer support in
Cython (the `cdef np.ndarray[double, ndim=2]` declarations) -- they are
faster, they are supported in function signatures for every kind of Cython
function definition (`def`, `cdef`, and `cpdef`), they are easier to declare
and use, and they do not have any external dependencies (i.e., you do not have
to `cimport` anything to use them, and you do not have to add extra include
flags when compiling).

Another advanced topic touched on in the tutorial was wrapping templated C++
classes.  Cython's syntax for wrapping templated C++ is fairly easy to work
with if wrapping just one template instantiation.  Once you need to wrap
several template instantiations, it is recommended to use a code generation
tool like cheetah or jinja2 to avoid manual code duplication.  It is often
helpful to provide a top-level wrapper class for a more Pythonic experience.
Examples of this approach are in the tutorial material zip file.  Cython's
fused types can alleviate the need for these workarounds, but can require some
gymnastics to use.

I also wanted to demonstrate how to wrap modern Fortran: user derived types,
assumed shape and assumed size arrays, module procedures, etc.  You can
accomplish this using the ISO_C_BINDING module that is part of the Fortran
2003 standard and supported by nearly every modern Fortran compiler.  Alas,
this had to be cut due to time constraints.


.. [tut] http://conference.scipy.org/scipy2013/tutorial_detail.php?id=105

.. [content] https://public.enthought.com/~ksmith/scipy2013_cython/scipy-2013-cython-tutorial.zip

.. [memviews] http://docs.cython.org/src/userguide/memoryviews.html

.. [3118] http://www.python.org/dev/peps/pep-3118/
