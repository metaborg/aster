include $(top_srcdir)/Makefile.xt
include $(wildcard *.dep)

STRS   = aster-runtime.str $(wildcard aster/runtime/*.str)
RTREES = $(STRS:.str=.rtree)

pkgdata_DATA = $(STRS) $(RTREES)
CLEANFILES   = $(RTREES)

