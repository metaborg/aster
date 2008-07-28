STRCFLAGS = \
	-la stratego-lib \
	-I ../syntax \
	-I ../trans \
	-I ../runtime \
	--format-check 0 \
	-O 0 \
	-Xcc -O0

RUNTESTS = empty find-minimum repmin repmin-no-copy-rules repmin-anonymous repmin-hag repmin-hag-decorated binary-numbers caching cons-nil anonymous-rules decorators circular-basic circular-liveness circular-liveness-rewrites # circular-propconst

check : Stratego-Attributes.runtestsuite \
        test-aster test-aster.run \
        $(RUNTESTS:=.str) $(RUNTESTS) \
        $(RUNTESTS:=.run)

# check-prepare : $(RUNTESTS:=.str)
#	$(MAKE) -j3 $(RUNTESTS)

CLEANFILES = $(tests)

clean :
	rm -f $(RUNTESTS) $(RUNTESTS:=.str) test-attributes.str

%.str : %.astr ../trans/aster
	$(top_srcdir)/trans/aster -i $< -o $@ --trace --verbose 2

%$(EXEEXT) : %.str ../trans/aster
	strc -i $< -m main-`basename $*` $(STRCFLAGS) >/dev/null

../trans/aster :
	$(MAKE) -C .. trans/aster

test-aster : test-attributes.str

test-attributes.str : test-attributes.astr
	$(top_srcdir)/trans/aster -i $< -o $@ --verbose 2
	

%.run : %
	@echo
	@echo ==== `basename $@ .run` ====
	@./$* --verbose 2

%.runtestsuite : %.testsuite
	parse-unit --no-heuristic-filters -i $< -p ../syntax/Stratego-Attributes.tbl
