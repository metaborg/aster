STRCFLAGS = \
	-la stratego-lib \
	-I ../syntax \
	-I ../trans \
	-I ../runtime \
	--format-check 0 \
	-Xcc -O0

RUNTESTS = test-stratego-rerag \
           empty find-minimum repmin repmin-anonymous repmin-hag repmin-hag-decorated binary-numbers caching cons-nil anonymous-rules circular-basic circular-basic-decorators circular-liveness circular-liveness-rewrites decorators

check : Stratego-Attributes.runtestsuite $(RUNTESTS:=.str) $(RUNTESTS:=.run)

CLEANFILES = $(tests)

clean :
	rm -f $(RUN_TESTS) $(STR_TESTS) test-attributes.str

%.str : %.astr ../trans/stratego-rerag
	$(top_srcdir)/trans/stratego-rerag -i $< -o $@ --trace --verbose 2

%$(EXEEXT) : %.str ../trans/stratego-rerag
	strc -i $< -m main-`basename $*` $(STRCFLAGS) >/dev/null

../trans/stratego-rerag :
	$(MAKE) -C .. trans/stratego-rerag

test-stratego-rerag : test-attributes.str

test-attributes.str : test-attributes.astr
	$(top_srcdir)/trans/stratego-rerag -i $< -o $@ --verbose 2
	

%.run : %
	@echo
	@echo ==== `basename $@ .run` ====
	@./$* --verbose 2

%.runtestsuite : %.testsuite
	parse-unit --no-heuristic-filters -i $< -p ../syntax/Stratego-Attributes.tbl
