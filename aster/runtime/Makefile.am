include $(top_srcdir)/Makefile.xt
include $(wildcard *.dep)

STRS   = astr-runtime.str $(wildcard astr/runtime/*.str)
RTREES = $(STRS:.str=.rtree)

pkgdata_DATA = $(STRS) $(RTREES)
CLEANFILES   = $(RTREES)

