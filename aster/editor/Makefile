SYNTAX     = Stratego-Attributes
EXTENSIONS = astr
PACKAGE    = org.strategoxt.imp.editors.strategoattributes
SOURCE     = ../syntax/$(SYNTAX).def
START      = Module

SDF2IMP    = sdf2imp
ESVS       = ${wildcard *.esv}

editor : eclipse-deps

clean :
	rm -f src/parser/$(SYNTAX)ParseController.java plugin.xml
	rm -rf src/parser/ast
	# ...

eclipse-deps : plugin.xml # and others

plugin.xml : $(SYNTAX).def $(SYNTAX).tbl Makefile $(ESVS)
	$(SDF2IMP) --verbose 2 -i $< -p $(SYNTAX).tbl -e $(EXTENSIONS) -s $(START) -m $(SYNTAX) -n $(PACKAGE)

$(SYNTAX).def : $(SOURCE)
	ln -s $< $@

$(SYNTAX).tbl : $(SYNTAX).def
	sdf2table -i $< -o $@ -m $(SYNTAX)

$(SYNTAX).rtg : $(SYNTAX).def
	sdf2table -i $< -o $@ -m $(SYNTAX)editor :
  
