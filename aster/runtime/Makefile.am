include $(top_srcdir)/Makefile.xt
include $(wildcard *.dep)

PACKED  = aster-runtime.packed-rtree
STRS    = aster-runtime.str $(wildcard aster/*.str) $(wildcard aster/*/*.str) $(wildcard aster/*/*/*.str)
ASTRS   = $(wildcard aster/*.astr) $(wildcard aster/*/*.astr) $(wildcard aster/*/*/*.astr)
RTREES  = $(STRS:.str=.rtree)
ARTREES = $(ASTRS:.astr=.rtree)

$(PACKED) : $(RTREES) $(ARTREES)
	strc -i aster-runtime.rtree --format-check 0 --ast -o $@

# To pack them using strc, astr files must be provided as rtree files
%.rtree : %.astr
	sglri -p ../syntax/Stratego-Attributes.tbl -i $< -o $@

pkgdata_DATA = $(PACKED) $(STRS) $(ASTRS)
EXTRA_DIST   = $(PACKED) $(STRS) $(ASTRS)
CLEANFILES   = $(PACKED) $(RTREES) $(ARTREES) $(ASTRS:.astr=.str)

