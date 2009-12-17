include $(top_srcdir)/Makefile.xt

EXTRA_DIST = $(STRS) $(METAS) front/check.astr
bin_PROGRAMS                  = aster
nodist_stratego_rerag_SOURCES = aster.c
nobase_pkgdata_DATA = $(RTREES)

all : aster.copy

CLEANFILES=aster aster-no-check aster.copy aster.c aster-no-check.c front/check.str $(RTREES)

SCFLAGS = \
	--format-check 0 \
	-la stratego-lib \
	-la stratego-sglr \
	-la stratego-xtc \
	-la stratego-tool-doc \
	-la strc \
	# `pkg-config --variable=strcxtcflags strategoxt`

LDADD += $(STRATEGO_XTC_LIBS) \
         $(STRATEGOXT)/lib/libstrc.la \
         $(STRATEGO_TOOL_DOC_LIBS)

STRINCLUDES = \
	-I $(top_srcdir)/syntax \
	-I $(top_srcdir)/runtime

STRS  =aster.str  $(wildcard *.str)  $(wildcard */*.str)
METAS =           $(wildcard *.meta) $(wildcard */*.meta)
RTREES=$(STRS:.str=.rtree) front/check.rtree

cleanstrap :
	rm -f aster.copy aster-no-check aster front/check.str

aster.c : $(RTREES) ../runtime/aster-runtime.packed-rtree front/check.rtree
	$(SCOMPILE) -c -i $< -o $@ $(SCFLAGS) $(STRINCLUDES) -m main-aster

# TODO: $(STRC_JAVA)/bin/strj?

java/org/strategoxt/aster/Main.java : $(RTREES) ../runtime/aster-runtime.packed-rtree front/check.rtree
	[ -e java/org/strategoxt/aster ] || mkdir -p java/org/strategoxt/aster
	strj -clean -i $< -o $@ $(SCFLAGS) $(STRINCLUDES) -m main-aster

java/aster.jar : java/org/strategoxt/aster/Main.java
	strj-jar -i $< -o $@

aster.copy : aster
	cp aster aster.copy

aster-no-check : aster.str $(RTREES) front/check.rtree
	@$(MAKE) -C .. all-local # ensure build-time XTC repository is complete at this point
	[ -e aster.copy ] && cp aster.copy aster-no-check ||\
	( rm -f $@.c && \
	  cp front/check.bootstrap.str front/check.str && \
	  $(SCOMPILE) -i $< -o $@ $(STRCFLAGS) $(SCFLAGS) -m main-aster -Xcc -O0 -Xcc -DDEFAULT_XTC_REPOSITORY="\"$(top_srcdir)/BUILDTIME_XTC\"" $(STRINCLUDES) && \
	  rm front/check.str $@.c )

front/check.str : front/check.astr aster-no-check
	./aster-no-check --verbose 2 -i $< -o $@ || ($(MAKE) cleanstrap; exit 1)

