# The following make file was inspired by https://scaron.info/blog/makefiles-for-latex.html

ALLTEX=$(wildcard *.tex ../Plots/*.pgf ../Figures/*.tex)
ALLCACHE=$(wildcard cache/* cache)
ALLFIG=$(wildcard Figures/*.pdf_tex)
NAME=Presentation_TheoKoppenhoefer
MAIN=$(NAME).tex
SHELL=/bin/bash
TEMPLATES=/mnt/64BB-4184/Filing/Education/University/Lund/Courses/MasterThesis/Latex_Templates

all: $(ALLTEX) $(ALLCACHE) $(ALLFIG) ## Build full thesis (LaTeX + figures)
	make figures
	rubber --pdf --unsafe $(MAIN)				# main run
	biber $(MAIN:.tex=)			 # bibliography
	rubber --unsafe $(MAIN)				# incremental run
	evince $(MAIN:.tex=).pdf 2>&1 &               # show result
   
# ../Figures/FlowchartTranscriptionFactors_modified.tex : ../Figures/FlowchartTranscriptionFactors_modified.dia
# 	cd ../Figures; dia --export=FlowchartTranscriptionFactors_modified.tex FlowchartTranscriptionFactors_modified.dia; ./modify_diagrams.sh


figures:
	cd ../Figures; for file in $$(ls *.svg); do inkscape --export-pdf=$${file%.svg}.pdf --export-latex $$file; done
	make quick

bibliography:
	biber $(NAME)
	make quick

quick: $(MAIN) $(TEMPLATES)
	export TEXINPUTS=$(TEMPLATES)//:; pdflatex -shell-escape $(MAIN)

clean: $(MAIN) ## Clean LaTeX and output figure files
	rubber --clean $(MAIN)
	rm cache/*

watch: $(MAIN) ## Recompile on any update of LaTeX or SVG sources
	@while true; do		  \
		sleep 0.01; inotifywait -e modify,delete,delete_self $(MAIN);	\
		sleep 0.01;			\
		make quick; echo "\n----------\n"; done
