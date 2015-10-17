########################################################################
#                                                                      #
#              « DO WHAT YOU WANT TO » PUBLIC LICENSE                  #
#                       Version 2, December 2004                       #
#                                                                      #
# Copyright (C) 2004 Sam Hocevar                                       #
#  22 rue de Plaisance, 75014 Paris, France                            #
# Everyone is permitted to copy and distribute verbatim or modified    #
# copies of this license document, and changing it is allowed as long  #
# as the name is changed.                                              #
#                                                                      #
#                  DO WHAT YOU WANT TO PUBLIC LICENSE                  #
#   TERMS AND CONDITIONS FOR COPYING, DISTRIBUTION AND MODIFICATION    #
#                                                                      #
#  0. You just DO WHAT YOU WANT TO.                                    #
#  																	   #
#  Somes Changes made by Thomas PABST on 01/2011					   #
#                                                                      #
########################################################################

PDF_PRODUCER=pdflatex
PDF_PRODUCERSILENTOPTIONS="-interaction=nonstopmode"
INDEX_MAKER=makeindex
BIBLIO_MAKER=bibtex
PDF_VIEWER=acroread

LATEX_SOURCES=$(shell grep -l '^\\begin{document}' *.tex)
LATEX_FILES=$(wildcard *.tex)
TARGETS=$(LATEX_SOURCES:%.tex=%.pdf)
TARGETS_SILENT=$(LATEX_SOURCES:%.tex=%.silent)
TARGETS_CLEAN=$(LATEX_SOURCES:%.tex=%.clean)
TARGETS_POLISH=$(LATEX_SOURCES:%.tex=%.polish)
TARGETS_VIEW=$(LATEX_SOURCES:%.tex=%.view)
TARGETS_WC=$(LATEX_SOURCES:%.tex=%.wordcount)

.PHONY: all silent clean view polish wordcount do_anyway

all: $(TARGETS) wordcount

silent: $(TARGETS_SILENT)

clean: $(TARGETS_CLEAN)

view: all
	$(PDF_VIEWER) $(TARGETS)

scour: polish
polish: $(TARGETS_POLISH)

wordcount: $(TARGETS_WC)

do_anyway:

%.pdf : do_anyway
	@echo -e "\033[31mFirst compilation\033[0m"
	$(PDF_PRODUCER) "$*.tex" \

	@echo -e "\033[31mMaking glossary\033[0m"
	-$(INDEX_MAKER) -t "$*.glg" \
					-o "$*.gls" \
					-s "$*.ist" \
					   "$*.glo" \

	@echo -e "\033[31mMaking index\033[0m"
	-$(INDEX_MAKER) "$*.idx" \

	@echo -e "\033[31mMaking bibliography\033[0m"
	-@$(BIBLIO_MAKER) "$*" \

	@echo -e "\033[31mSecond compilation\033[0m"
	$(PDF_PRODUCER) "$*.tex" \

	@echo -e "\033[31mThird compilation\033[0m"
	$(PDF_PRODUCER) "$*.tex" \


%.silent : do_anyway
	@echo -e First compilation...
	$(PDF_PRODUCER) $(PDF_PRODUCERSILENTOPTIONS) "$*.tex" &> /dev/null
	
	@echo -e Making glossary...
	-$(INDEX_MAKER) -t "$*.glg" \
					-o "$*.gls" \
					-s "$*.ist" \
					   "$*.glo" \
					   
	@echo -e Making index...
	-$(INDEX_MAKER) "$*.idx" \

	@echo -e Making bibliography...
	-@$(BIBLIO_MAKER) "$*" \

	@echo -e Second compilation...
	$(PDF_PRODUCER) $(PDF_PRODUCERSILENTOPTIONS) "$*.tex" &> /dev/null

	@echo -e Third compilation
	$(PDF_PRODUCER) $(PDF_PRODUCERSILENTOPTIONS) "$*.tex" &> /dev/null

%.clean: do_anyway
	@echo -e "\033[31mCleaning $*.*...\033[0m"
	@rm -f   -- $*.aux \
				$*.bbl \
				$*.blg \
				$*.glg \
				$*.glo \
				$*.gls \
				$*.fax \
				$*.idx \
				$*.ilg \
				$*.ind \
				$*.ist \
				$*.lof \
				$*.log \
				$*.loh \
				$*.loi \
				$*.lot \
				$*.nav \
				$*.out \
				$*.snm \
				$*.tns \
				$*.toc \
				$*~ \
				$*.mtc* \
				$*.maf \
								
%.view : %.pdf
	$(PDF_VIEWER) $<

%.scour: %.polish

%.polish: %.clean
	@echo -e "\033[31mCleaning more...\033[0m"
	@rm -f -- $*.pdf 		\
			  x.log 		\
			  missfont.log 	\
			  texput.log 	\
			  *~			\

			  
%.wordcount: do_anyway
	@echo -e -n "\033[34mWord count\033[0m - $* - "
	@echo -e $(shell ps2ascii "$*.pdf" | wc -w)

