include $(top_srcdir)/Makefile.xt

bin_PROGRAMS                  = stratego-rerag
nodist_stratego_rerag_SOURCES = stratego-rerag.c

all : stratego-rerag.copy

CLEANFILES=stratego-rerag stratego-rerag-no-check stratego-rerag.copy front/check.str $(RTREES)

SCFLAGS = \
	--format-check 0 \
	-la stratego-lib \
	-la stratego-sglr \
	-la stratego-xtc \
	-la strc \
	# `pkg-config --variable=strcxtcflags strategoxt`

LDADD += $(STRATEGO_XTC_LIBS) $(STRATEGOXT)/lib/libstrc.la

STRINCLUDES = \
	-I ../syntax \
	-I ../runtime

STRS=stratego-rerag.str  $(wildcard *.str) $(wildcard */*.str) $(wildcard ../runtime/*.str) $(wildcard ../runtime/*/*.str) front/check.str
RTREES=$(STRS:.str=.rtree)

bootclean :
	rm -f stratego-rerag.copy stratego-rerag-no-check stratego-rerag front/check.str

stratego-rerag.c : $(RTREES)
	$(SCOMPILE) -c -i $< -o $@ $(SCFLAGS) $(STRINCLUDES) -m main-stratego-rerag
	
stratego-rerag.copy : stratego-rerag
	cp stratego-rerag stratego-rerag.copy

stratego-rerag-no-check : stratego-rerag.str $(RTREES)
	$(MAKE) -C .. all-local # ensure build-time XTC repository is complete at this point
	[ -e stratego-rerag.copy ] && cp stratego-rerag.copy stratego-rerag-no-check ||\
	( rm -f $@.c && \
	  cp front/check.bootstrap.str front/check.str && \
	  $(SCOMPILE) -i $< -o $@ $(STRCFLAGS) $(SCFLAGS) -m main-stratego-rerag -Xcc -O0 -Xcc -DDEFAULT_XTC_REPOSITORY="\"$(top_srcdir)/BUILDTIME_XTC\"" $(STRINCLUDES) && \
	  rm front/check.str $@.c )

front/check.str : front/check.astr stratego-rerag-no-check
	./stratego-rerag-no-check --verbose 2 -i $< -o $@ || $(MAKE) bootclean

