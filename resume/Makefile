# Build the resume PDF and place it at the site root, so it is served at
# https://slava.lol/resume.pdf. Two pdflatex passes are required: the header
# band is a tikz overlay that only resolves its position on the second pass.
LATEX = pdflatex -interaction=nonstopmode -halt-on-error
OUT   = ../resume.pdf

.PHONY: all clean
all: $(OUT)

$(OUT): resume.tex simplehipstercv.cls simplehipstercv.sty $(wildcard *.jpg) ../me.png
	$(LATEX) resume.tex
	$(LATEX) resume.tex
	mv resume.pdf $(OUT)
	rm -f resume.aux resume.log resume.out

clean:
	rm -f resume.pdf resume.aux resume.log resume.out resume.fdb_latexmk resume.fls resume.synctex.gz
