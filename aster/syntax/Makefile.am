include $(top_srcdir)/Makefile.xt
include $(wildcard *.dep)

TBLS_DEFS = Stratego-Attributes.tbl \
            Stratego-Attributes.def \
            Stratego-Attributes.str \
            Stratego-Attributes-in-Stratego.def \
            Stratego-Attributes-in-Stratego.tbl \
            MixStratego-Attributes.def

SIGS = Stratego-Attributes.str

pkgdata_DATA   = $(TBLS_DEFS)                   $(SIGS)

EXTRA_DIST     = $(TBLS_DEFS) $(sdfdata_DATA)   $(SIGS)
CLEANFILES     = $(TBLS_DEFS) $(wildcard *.dep) $(SIGS) 

SDF2RTG_FLAGS  = -m $* --ignore-missing-cons
PGEN_FLAGS     = -m $*
SDFINCLUDES    = 

Stratego-Attributes.def : $(wildcard *.sdf)

Mix%.sdf : %.def
	gen-sdf-mix -i $< --main `basename $*` --name Mix`basename $*` -o $@
