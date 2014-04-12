cudf_remote_proxy
=================

CUDF solver stub that behaves like a CUDF solver, but delegates solving to a remote solving farm


Rationale
---------

A key point of the approach which we set forth in the [Mancoosi project](http://www.mancoosi.org "Mancoosi Project") is that a modern package manager should clearly separate a platform specific front-end from a platform
independent back-end in charge of solving dependencies in order to perform
upgrades or downgrades according to the user's preferences.

### The CUDF format
To this end, the [CUDF format](http://www.mancoosi.org/cudf "CUDF format") has been developed as
pivot format allowing multiple package managers to access a variety of efficient dependency solvers.

Using the CUDF format as a pivot, we can share the effort of developing
efficient solvers among multiple package managers, and can even use different
solvers in the same package manager if the default one hits its limits (which
can happen, as dependency solving is an NP-complete problem).

### The CUDF solvers

There are already several efficient CUDF-compatible dependency solvers, most of them as free software,
see for example the following ones, that are based on different solving technologies, and participated
to the [MISC Competitions](http://www.mancoosi.org/misc "MISC Competitions"):

1.   [`aspcud`](http://sourceforge.net/projects/potassco/files/aspcud/ "Aspcud") is based on *Answer Set Programming*
2.   [`mccs`](http://www.i3s.unice.fr/~cpjm/misc/mccs.html "Mccs") is based on *Mixed Integer Linear Programming*
3.   [`packup`](http://sat.inesc-id.pt/~mikolas/sw/packup/ "Packup") is based on *Weighted MAX-SAT*


All of these solvers are packaged today in Debian, so they can be used as external solvers for compatible
package managers, like [apt-get](http://manpages.debian.org/cgi-bin/man.cgi?query=apt-get)
or [opam](http://opam.ocamlpro.com).

### The CUDF solver farm

On many platforms, though, precompiled version of CUDF solver are not readily available, and on mobile
platforms the cost of dependency solving may be too high.

As a service to the community, [Irill](http://www.irill.org) is hosting a
`remote CUDF solver farm`, that can be seamlessly accessed using the utility
found in this repository, which is a drop-in replacement for a local solver.

Usage
-----

To access the remote solver farm, just install the `cudf_remote_proxy` utility
in your path, and then tell your package manager to use it:

1. for `opam`, use `opam --solver cudf_remote_proxy`
2. for `apt-get`, use `apt-get --solver cudf_remote_proxy`

### Remarks
The default version of the utility compresses and decompresses the data before
transmission, to avoid delays on slow networks. On fast networks, you may
use `cudf_remote_proxy_plain` instead.

### References

1. [A modular package manager architecture](http://www.dicosmo.org/Publications/MPM2012ITS.html) *Information and Software Technology*, 55(2):459 - 474, 2013. Special Section: Component-Based Software Engineering (CBSE), 2011

2. [Dependency solving: A separate concern in component evolution management](http://www.dicosmo.org/Publications/DepSolversNP2012.html) *Journal of System and Software Science*, 85(10):2228 - 2240, 2012. Automated Software Evolution.
