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
FAILASTRS=$(wildcard fail/*.astr)

RUNTESTS  = $(ASTRS:.astr=)
FAILTESTS = $(FAILASTRS:.astr=)

EXTRA_DIST =\
  $(ASTRS) \
  $(METAS) \
  Stratego-Attributes.testsuite

check : Stratego-Attributes.runtestsuite \
        $(RUNTESTS:=.str) $(RUNTESTS) $(RUNTESTS:=.run) \
        $(FAILTESTS:=.rtree) $(FAILTESTS:=.fail)

CLEANFILES = $(RUNTESTS) $(RUNTESTS:=.str) $(FAILTESTS:=.rtree)

%.str : %.astr $(top_srcdir)/trans/aster
	$(top_srcdir)/trans/aster -i $< -o $@ --verbose 2

%$(EXEEXT) : %.str
	strc -i $< -m main-`basename $*` $(STRCFLAGS) >/dev/null

$(top_srcdir)/trans/aster$(EXEEXT) :
	$(MAKE) -C $(top_srcdir)/trans aster$(EXEEXT)

test-attributes.str : test-attributes.astr $(top_srcdir)/trans/aster
	$(top_srcdir)/trans/aster -i $< -o $@ --verbose 2

%.run : %$(EXEEXT)
	@echo
	@echo ==== `basename $@ .run` ====
	@./$* --verbose 2

FAILTESTS :
	@echo
	@echo ==== failure tests ====

# HACK: these should be called atrees or something
%.rtree : %.astr
	sglri -p $(top_srcdir)/syntax/Stratego-Attributes.tbl -i $< -o $@

%.fail : %.rtree FAILTESTS
	@echo fail/`basename $@ .fail`
	@$(top_srcdir)/trans/aster -i $< 2>/dev/null && echo "TEST FAILED: No error reported."; [ ] || [ 1 ]

%.runtestsuite : %.testsuite
	parse-unit --no-heuristic-filters -i $< -p ../syntax/Stratego-Attributes.tbl
