SYNTAX     = Stratego-Attributes
SOURCE     = ../syntax/$(SYNTAX).def

editor : plugin.xml # and others

clean :
	rm -f src/parser/$(SYNTAX)ParseController.java plugin.xml
	rm -rf src/parser/ast
	# ...

plugin.xml : $(SYNTAX).def $(SYNTAX).tbl Makefile ${wildcard src/*.esv}
	sdf2imp --verbose 2 -i $< -p $(SYNTAX).tbl -m $(SYNTAX)

$(SYNTAX).def : $(SOURCE)
	ln -s $< $@

$(SYNTAX).tbl : $(SYNTAX).def
	sdf2table -i $< -o $@ -m $(SYNTAX)

$(SYNTAX).rtg : $(SYNTAX).def
	sdf2table -i $< -o $@ -m $(SYNTAX)editor :
  
