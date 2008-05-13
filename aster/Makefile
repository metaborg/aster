STRCFLAGS = \
	-la stratego-lib \
	-la stratego-sglr \
	-I syntax \
	-I runtime \
	-I ~/.nix-profile/share \
	--format-check 0 \
	`pkg-config --variable=strcxtcflags strategoxt`

DEPS = \
    trans/stratego-rerag.str \
    $(wildcard trans/*.str) \
    $(wildcard trans/*/*.str) \
    $(wildcard trans/*/*/*.str) \
    syntax/Stratego-Attributes-in-Stratego.tbl \
    syntax/Stratego-Attributes-in-Stratego.tbl

# I don't care much for nullary constructor warnings unless it's about lowercase ones, such as 'start'
STRCSTFU=2>&1 | grep -vE 'warning ] Nullary constructor .*[A-Z]|warning ] multiple external| \[ExtSDef\(|stratego-trm|module-name-checked|module-name-checked"' \
                 >&2

all : syntax/Stratego-Attributes.tbl \
      syntax/Stratego-Attributes.def \
      syntax/Stratego-Attributes.str \
      syntax/Stratego-Attributes-in-Stratego.tbl \
      trans/stratego-rerag

check : all
	$(MAKE) -C tests check

clean :
	rm -f syntax/*.tbl syntax/*.def syntax/*.dep trans/stratego-rerag trans/stratego-rerag-no-check trans/stratego-rerag.copy trans/front/check.str
	$(MAKE) -C tests clean

bootclean :
	rm -f trans/stratego-rerag.copy trans/stratego-rerag-no-check trans/stratego-rerag

% : %.str trans/front/check.str $(DEPS) $(wildcard runtime/*.str)
	@rm -f $@.c
	strc -i $< -o $@ -m main-`basename $*` $(STRCFLAGS) $(STRCSTFU)
	@cp $@ $@.copy
	@rm $@.c

trans/stratego-rerag-no-check : $(DEPS)
	[ -e trans/stratego-rerag.copy ] && cp trans/stratego-rerag.copy trans/stratego-rerag-no-check ||\
	( rm -f $@.c && \
	  cp trans/front/check.bootstrap.str trans/front/check.str && \
	  strc -i $< -o $@ -m main-stratego-rerag -Xcc -O0 $(STRCFLAGS) $(STRCSTFU) && \
	  rm trans/front/check.str $@.c )

trans/front/check.str : trans/front/check.astr trans/stratego-rerag-no-check
	trans/stratego-rerag-no-check --verbose 2 -i $< -o $@ || $(MAKE) bootclean

%.rtg : %.def
	sdf2rtg -i $< -m `basename $*` -o $@ --ignore-missing-cons

%.str : %.rtg
	rtg2sig -i $< -o $@	

%.def : %.sdf syntax/*.sdf
	pack-sdf -i $< -o $@  -I syntax

Mix-%.sdf : %.def
	gen-sdf-mix -i $< --main `basename $*` --name Mix-`basename $*` -o $@

%.tbl : %.def
	sdf2table -m `basename $*` -i $< -o $@

