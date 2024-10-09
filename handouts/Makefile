.PHONY: all

all: handout-lesson1.pdf handout-lesson2.pdf

clean:
	rm -f handout-lesson1.pdf handout-lesson2.pdf

%.pdf: %.tex common.tex
	docker run --rm -it -v ${CURDIR}:/mnt/ adnrv/texlive:latest bash -xc "cp /mnt/*.tex /tmp/; pdflatex --output-directory=/tmp -interaction=nonstopmode /tmp/$<; chown $$(id -u):$$(id -g) /tmp/$@; mv /tmp/$@ /mnt/"
