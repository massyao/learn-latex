PDFCROP = pdfcrop
RM = /bin/rm
TIKZDIR=src/tikz/

ifndef DEBUG
PDFLATEX = pdflatex -interaction=batchmode
LATEXMK = latexmk -silent -f -g -pdf
COMPRESS = gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dNOPAUSE -dQUIET -dBATCH -sOutputFile=compressed.pdf
else
PDFLATEX = pdflatex
LATEXMK = latexmk -pdflatex='$(PDFLATEX)' -g -pdf
COMPRESS = gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dNOPAUSE -dBATCH -sOutputFile=compressed.pdf
endif

TIKZFILES = $(wildcard src/tikz/*.tex)
TIKZPDF = $(patsubst %.tex,%.pdf,$(TIKZFILES))

default: tikz.fmt main.fmt $(TIKZPDF)
	$(LATEXMK) main.tex || [ $$? -eq 12 ]
	$(COMPRESS) main.pdf

src/tikz/%.pdf: src/tikz/%.tex
	echo $<
	$(PDFLATEX) -fmt="tikz.fmt" -output-directory="src/tikz/" $<
	$(PDFCROP) $@ $@

main.fmt:
	$(PDFLATEX) -ini -jobname="main" "&pdflatex include/header.tex\dump"

tikz.fmt:
	$(PDFLATEX) -ini -jobname="tikz" "&pdflatex include/tikz.tex\dump"

clean : .PHONY
	$(RM) -f *.{aux,bak,bbl,blg,dvi,fdb_latexmk,fmt,log,out,toc,tdo,fls}
	$(RM) -f src/tikz/*.{aux,bak,bbl,blg,dvi,fdb_latexmk,.mt,log,out,toc,tdo,fls}

depclean : clean
	$(RM) -f src/tikz/*.pdf

distclean : depclean
	$(RM) -f main.pdf
	$(RM) -f compressed.pdf

.PHONY:
