STRCFLAGS = \
	-la stratego-lib \
	-I ../syntax \
	-I ../trans \
	-I ../runtime \
	--format-check 0 \
	-Xcc -O0

TESTS = empty find-minimum repmin repmin-anonymous repmin-hag repmin-hag-decorated binary-numbers caching cons-nil anonymous-rules circular-basic circular-liveness circular-liveness-rewrites decorators

clean :
	rm -f $(RUN_TESTS) $(STR_TESTS) test-stratego-rerag test-attributes.str

%.str : %.astr ../trans/stratego-rerag
	../trans/stratego-rerag -i $< -o $@ --trace --verbose 2

%$(EXEEXT) : %.str ../trans/stratego-rerag
	strc -i $< -m main-`basename $*` $(STRCFLAGS) >/dev/null

../trans/stratego-rerag :
	$(MAKE) -C .. trans/stratego-rerag

test-stratego-rerag : test-attributes.str

%.run : %
	@echo
	@echo ==== `basename $@ .run` ====
	@./$* --verbose 2

%.runtestsuite : %.testsuite
	parse-unit --no-heuristic-filters -i $< -p ../syntax/Stratego-Attributes.tbl