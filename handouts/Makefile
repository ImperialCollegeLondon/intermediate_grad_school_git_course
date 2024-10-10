.PHONY: all

all: git-course-handout.pdf

clean:
	rm -f git-course-handout.pdf

%.pdf: %.tex common.tex
	docker run --rm -it -v ${CURDIR}:/mnt/ adnrv/texlive:latest bash -xc "cp /mnt/*.tex /tmp/; pdflatex --output-directory=/tmp -interaction=nonstopmode /tmp/$<; chown $$(id -u):$$(id -g) /tmp/$@; mv /tmp/$@ /mnt/"
