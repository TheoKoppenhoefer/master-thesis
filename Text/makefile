# The following make file was inspired by https://scaron.info/blog/makefiles-for-latex.html

ALLTEX=$(wildcard *.tex ../Plots/*.pgf ../Figures/*.tex)
ALLCACHE=$(wildcard cache/* cache)
ALLPYTH=$(wildcard ../*.py)
MAIN=Thesis_TheoKoppenhoefer.tex
SHELL=/bin/bash

all: $(ALLTEX) $(ALLCACHE) ## Build full thesis (LaTeX + figures)
	rubber --pdf --unsafe $(MAIN)				# main run
	biber $(MAIN:.tex=)			 # bibliography
	rubber --unsafe $(MAIN)				# incremental run
	evince $(MAIN:.tex=).pdf 2>&1 &               # show result
   
# ../Figures/FlowchartTranscriptionFactors_modified.tex : ../Figures/FlowchartTranscriptionFactors_modified.dia
# 	cd ../Figures; dia --export=FlowchartTranscriptionFactors_modified.tex FlowchartTranscriptionFactors_modified.dia; ./modify_diagrams.sh

quick: $(MAIN)
	pdflatex -shell-escape $(MAIN)

clean:  ## Clean LaTeX and output figure files
	rubber --clean $(MAIN)
	rm cache/*

watch:  ## Recompile on any update of LaTeX or SVG sources
	@while true; do		  \
		sleep 0.01; inotifywait -e modify,delete,delete_self $(MAIN);	\
		sleep 0.01;			\
		make quick; echo "\n----------\n"; done
