SUFFIXES= .tex .dvi .aux .log .pdf .ps .sty .dtx .ins .dic .ist .bst .bib .bbl .lof .lot .toc .ind .gls .glo .idx .out .brf .exa .chl .ilg .4dx 
#\begin{setting}[valiable]
TEX=	platex-euc -kanji=euc
BIBTEX=	jbibtex
DVIPS=	dvips -t a4
DVIPDFMX=dvipdfmx -p a4
MAKEIDX=mendex -E
F=	kimi
REF=	kimi.bib #latexbase.bib latexbase2.bib latextools.bib manual.bib
IST=	sind.ist
GST=	sgls.ist
BST=	splain.bst
#\end{setting}
#\begin{setting}[sources]
SRC=	01preface.tex \
	02thanx.tex \
	03basics.tex \
	04latexe.tex \
	05classe.tex \
	06macros.tex \
	07applic.tex \
	memo3.tex \
	memo4.tex \
	memo5.tex \
	memo9.tex \
	fdl.tex 
SRC2=	jisyo.dic 
STY=	jlisting.sty okumacro.sty 
MAC=lll.sty lll.dtx lll.ins lll.dvi
#\end{setting}
#\begin{target}
all: lll.sty $(F).pdf #$(F)hyper.pdf cleanany
#
lll.sty:	lll.dtx lll.ins
	yes | $(TEX) lll.ins
#
lll.dvi:	lll.dtx
	$(TEX) lll.dtx
	$(MAKEIDX) -s gind.ist -o lll.ind lll.idx
	$(TEX) lll.dtx
	rm -f lll.aux lll.glo lll.idx lll.ilg 
	rm -f lll.log *~
#
$(F)hyper.tex: $F.tex
	echo -e "\AtBeginDvi{\special{pdf:tounicode 90ms-RKSJ-UCS2}}"\
	> $@
	grep "%" $< >> $@

$(F).ps: $F.dvi
	$(DVIPS) -o $@ $<

$(F).pdf: $F.dvi
	$(TEX) $F
	$(DVIPDFMX) -o $@ $<

$(F)hyper.pdf: cleanany $(F)hyper.tex
	$(TEX) $(F)hyper.tex
	$(TEX) $(F)hyper.tex
	$(MAKEIDX) -s $(IST) -d jisyo.dic -o $(F)hyper.ind $(F)hyper.idx
	$(MAKEIDX) -s $(GST) -d jisyo.dic -o $(F)hyper.gls $(F)hyper.glo
	$(TEX) $(F)hyper
	$(TEX) $(F)hyper
	$(DVIPDFMX) -o $@ $(F)hyper

$(F).dvi:$(F).ind $(F).gls# $(F).bbl 
	$(TEX) $F
	(while egrep "may have changed" $F.log; \
           do $(TEX) $F; done)

$(F).ind: $(F).idx 
	$(MAKEIDX) -s $(IST) -d jisyo.dic -o $@ $<

$(F).gls: $(F).glo
	$(MAKEIDX) -s $(GST) -d jisyo.dic -o $@ $<

$(F).bbl: $(F).aux
	$(BIBTEX) $F

$(F).glo: $(F).aux $(SRC)
	$(TEX) $F

$(F).idx: $(F).aux $(SRC)
	$(TEX) $F

$(F).aux: $(F).tex $(SRC)
	$(TEX) $F

html:
	jlatex $(F)
	jlatex $(F)
	jtex "\def\filename{{$(F)}{idx}{4dx}{ind}} \input idxmake.4ht"
	jmakeindex -o $(F).ind $(F).4dx
	ht --cleanup jlatex $(F)

tar: $(F).tex $(STY) $(BST) $(IST) $(GST) $(SRC) $(SRC2) $(REF) \
	Makefile $(MAC) $(F)hyper.tex
	mkdir -p $(F)src/images && mkdir -p $(F)src/sjis
	cp $(REF) $(SRC) $(SRC2) $(IST) $(GST) $(BST) $(STY) $(F).tex \
           $(F)src/
	cp images/*.* $(F)src/images/
#	./sjis.sh && cp sjis/*.* $(F)src/sjis/
	cp Makefile $(MAC) $(F)hyper.tex $(F)src/
	tar czf $(F)src.tgz $(F)src/
	rm -fr $(F)src

clean:
	rm -f $(F).glo $(F).blg $(F).lof $(F).lot
	rm -f $(F).out $(F).toc $(F).ind $(F).aux
	rm -f $(F).dvi $(F).idx $(F).ilg $(F).brf
	rm -f $(F).gls $(F).exa $(F).chl

cleanh:
	rm -f $(F)hyper.bbl $(F)hyper.blg $(F)hyper.lof 
	rm -f $(F)hyper.lot
	rm -f $(F)hyper.out $(F)hyper.toc $(F)hyper.ind 
	rm -f $(F)hyper.aux
	rm -f $(F)hyper.dvi $(F)hyper.idx $(F)hyper.ilg 
	rm -f $(F)hyper.brf

cleanany: clean cleanh
	rm -f *.log *.aux *~ *.flc
#\end{target}
