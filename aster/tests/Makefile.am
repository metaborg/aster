STRCFLAGS = \
	-la stratego-lib \
	-la strc \
	-I ../syntax \
	-I ../trans \
	-I ../runtime \
	--format-check 0 \
	-O 0 \
	-Xcc -O0

ASTRS=$(wildcard *.astr)
METAS=$(wildcard *.meta)

RUNTESTS = $(ASTRS:.astr=)

EXTRA_DIST =\
  $(ASTRS) \
  $(METAS) \
  Stratego-Attributes.testsuite

check : Stratego-Attributes.runtestsuite \
        $(RUNTESTS:=.str) $(RUNTESTS) \
        $(RUNTESTS:=.run)

# check-prepare : $(RUNTESTS:=.str)
#	$(MAKE) -j3 $(RUNTESTS)

CLEANFILES = $(tests) $(ASTRS:.astr=.str)

clean :
	rm -f $(RUNTESTS) $(RUNTESTS:=.str) test-attributes.str

%.str : %.astr $(top_srcdir)/trans/aster
	$(top_srcdir)/trans/aster -i $< -o $@ --trace --verbose 2

%$(EXEEXT) : %.str
	strc -i $< -m main-`basename $*` $(STRCFLAGS) >/dev/null

$(top_srcdir)/trans/aster$(EXEEXT) :
	$(MAKE) -C $(top_srcdir)/trans aster$(EXEEXT)

test-attributes.str : test-attributes.astr $(top_srcdir)/trans/aster
	$(top_srcdir)/trans/aster -i $< -o $@ --verbose 2

%.run : %
	@echo
	@echo ==== `basename $@ .run` ====
	@./$* --verbose 2

%.runtestsuite : %.testsuite
	parse-unit --no-heuristic-filters -i $< -p ../syntax/Stratego-Attributes.tbl
