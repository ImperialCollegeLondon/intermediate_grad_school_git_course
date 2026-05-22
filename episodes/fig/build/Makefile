IMAGES := branch1 branch2 branch3 branch4 branch5 branch6 branch7 branch8 branch9 \
	branching_full_example pull push push_u rebase

all: $(IMAGES)

$(IMAGES): %: ../%.png ../%-dark.png

../%.png: %.pdf
	magick -density 192 $< $@

%.pdf: %.tex
	sed 's/@@COLOUR_VAR@@/black/g' $< | pdflatex -jobname=$*

%-dark.pdf: %.tex
	sed 's/@@COLOUR_VAR@@/white/g' $< | pdflatex -jobname=$*-dark
