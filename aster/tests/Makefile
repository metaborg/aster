TESTS = empty find-minimum repmin binary-numbers
RUN_TESTS = $(TESTS:=.run)
STR_TESTS = $(TESTS:=.str)

STRCFLAGS = \
	-la stratego-lib \
	-la stratego-sglr \
	-I ../syntax \
	-I ../runtime \
	-I ~/.nix-profile/share \
	--format-check 0 \
	-Xcc -O0

check : Stratego-Attributes.runtestsuite $(STR_TESTS) $(RUN_TESTS)

clean :
	rm -f $(RUN_TESTS) $(STR_TESTS)

%.str : %.astr ../trans/stratego-rerag
	../trans/stratego-rerag -i $< -o $@ --trace

%.run : %.str
	strc -i $< -m main-$* $(STRCFLAGS)
	@echo
	@echo ==== `basename $@ .run` ====
	@./`basename $@ .run`

%.runtestsuite : %.testsuite
	parse-unit --no-heuristic-filters -i $< -p ../syntax/Stratego-Attributes.tbl
