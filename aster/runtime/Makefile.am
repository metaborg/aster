include $(top_srcdir)/Makefile.xt
include $(wildcard *.dep)

STRS   = aster-runtime.str $(wildcard aster/runtime/*.str)
RTREES = $(STRS:.str=.rtree)

aster-runtime.rtree : $(STRS)

pkgdata_DATA = $(STRS) $(RTREES)
EXTRA_DIST   = $(STRS) $(RTREES)
CLEANFILES   = $(RTREES)

