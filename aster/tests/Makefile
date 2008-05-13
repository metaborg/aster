STRCFLAGS = \
	-la stratego-lib \
	-I ../syntax \
	-I ../trans \
	-I ../runtime \
	--format-check 0 \
	-Xcc -O0

TESTS = empty find-minimum repmin binary-numbers caching cons-nil anonymous-rules circular-basic circular-liveness circular-liveness-rewrites
RUN_TESTS = $(TESTS:=.run)
STR_TESTS = $(TESTS:=.str)
EXE_TESTS = $(TESTS:=)
EXEEXT =

check : Stratego-Attributes.runtestsuite \
        test-stratego-rerag test-stratego-rerag.run test-stratego-rerag.str \
        $(STR_TESTS) $(EXE_TESTS) $(RUN_TESTS)

clean :
	rm -f $(RUN_TESTS) $(STR_TESTS) test-stratego-rerag test-attributes.str

%.str : %.astr ../trans/stratego-rerag
	../trans/stratego-rerag -i $< -o $@ --trace

%$(EXEEXT) : %.str ../trans/stratego-rerag
	strc -i $< -m main-`basename $*` $(STRCFLAGS) >/dev/null

../trans/stratego-rerag :
	$(MAKE) -C .. trans/stratego-rerag

test-stratego-rerag : test-attributes.str

circular-liveness-rewrites.str : circular-liveness.str

%.run : %
	@echo
	@echo ==== `basename $@ .run` ====
	@./$* --verbose 2

%.runtestsuite : %.testsuite
	parse-unit --no-heuristic-filters -i $< -p ../syntax/Stratego-Attributes.tbl
