---
layout: post
title: Typesetting Gregorian Chant
date: 2023/11/06
---

For a couple of years now I have been using `Lilypond` to engrave sheet music for performance purposes, a nice side effect is that `Lilypond` also produces a midi file, which I can play through a synthesizer for practice purposes.  

However, recently I needed to typeset some Gregorian chant, and while `Lilypond` has capability for this, I quickly ran into situations where the engraved stave was less readable than the original I was transcribing.

# Gregorio
Today I came across `Gregorio` which allows transcription of chant to a simple Ascii format, a `.gabc` file, which is converted by the `gregorio` binary into a `Latex` file which is then processed by that processor to embed the chant into a `.tex` document.

## Install
I had to
```
sudo apt install texlive-music
sudo apt install texlive-fonts-extra
sudo apt install texlive-lualatex
<download> gregorio-6.0.0.tar.bz2
tar -xvjf gregorio-6.0.0.tar.bz2
./configure --disable-version-in-exe
make
sudo ./install.sh
```

After this it was a simple learning exercise running through the Gregorio Tutorial, and building my files with
```
lualatex thing.tex
```
### Gotcha 1
Neither `pdflatex` nor `xelatex` processes the input - a specific message indicating `lualatex` is required was emitted.

### Gotcha 2
The Tutorial syntax didn't work for me, specifically it had, eg
```
\gabcsnippet{(c4) Text}
```
and I found I had to use
```
\gabcsnippet{(c4)
Text
}
```
to avoid obscure error in the lualatex processing.
## Example
```
\documentclass[12pt, a4paper]{article}

\usepackage{fullpage}

\usepackage{fontspec}
\usepackage{libertine}

\usepackage[autocompile]{gregoriotex}

\title{Media vita}
\date{}
\author{}

\begin{document}
\maketitle
\grecommentary{Salisbury Antiphoner}
\greannotation{4.}
\gabcsnippet{(c4)
Me(d)di(e)a(f) vi(g)ta(fe..) *(,)
in(dh~) mor(h)te(gf) sum(ghGFE)us(e.) (;)
quem(hg~) quae(h)ri(g)mus(ghg) ad(fe~)ju(fg)to(g)rem(gffvED.) (;z)
ni(dc)si(fh) te(gh) Do(e)mi(egFE)ne?(e.f.) (:)
qui(d) pro(e') pec(f)ca(g)tis(e) no(fvE~D~)stris(ed..) (;)
ju(dg'h)ste(g) i(g)ra(gf/gh)sce(egFE)ris.(e.) *(:)

San(hg[alt:ALL])cte__(gg/h!iwjIHGFfd) (`) De(ef!gvF~E~)us,(e.) +(;)
San(hg)cte__(gg/h!iwjIHGFfd) (`) for(ef!gvF~E~)tis,(e.) +(;)
San(hg)cte__(gg/h!iwjIHG) (`) et(ff) mi(d)se(e)ri(f)cors(g) Sal(e)va(fvE~D~)tor,(ed..) (;)
a(f)ma(h)rae(g) mor(h)ti(e) ne(dg) tra(gf/gh)das(egFE) nos(e.) (::)
}

\end{document}

```
which renders to
![Example of typeset gregorian chant](/siddalp-actual/assets/MediaVita.jpg)
