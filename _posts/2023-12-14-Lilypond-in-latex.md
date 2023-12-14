---
layout: post
title: Lilypond in LaTeX
date: 2023/12/10
---

With Gregorio requiring LaTeX to format ancient music, the next question
is, how can I embed 'modern' music in LaTeX too - maybe with a view to
mixing the two on the same sheet of paper!

## Lilypond-Book

The Lilypond documentation describes the lilypond-book utility which 
converts lilypond files into a lilytex output suitable for embedding in
LaTeX documents - however I quickly ran into problems with this and just
a little Googling suggested it has been effectively superceded by the
lyluatex package.

## LyLuaTex

Works much the same way as gregorio, in that a LaTeX document refers to
or inlines a piece of music written in the lilypond format.  At text 
compilation time, a callout is made to lilypond to perform the formatting
and the output pulled back into the compiled document.

Because, like gregorio processing, a call-out is made from LaTeX, if must
be invoked to allow this:
```
lualatex --shell-escape {your_docname}
```

The music fragments themselves are either references to a lilypond file,
for example:
```
\lilypondfile{your_file.ly}
```
or some inline lilypond code:
```
\lilypond[staffsize=24]{\relative c'' { c b a g f e d c }}
```
Notice that formatting options can be passed in square brackets to both `\lilypondfile{}` and `\lilypond{}` macros.

### Some Teething problems I found
1) DON'T PUT SPACES IN LaTeX or Lilypond FILENAMES
1) This meant that I tried indirection and found your can't `\include{}` a LaTeX file which itself contains an `\include{}`.  But you can (and it works as an indirection mechanism) use lilypond includes: eg
>myProj.tex contains
>```
>\lilypondfile{nameWithNoSpaces.ly}
>
>```
>
>nameWithNoSpaces.ly contains
>```
>\include "name with spaces.ly"
>```
1) in addition to specifying formatting options in square brackets in the `\lilypond...` macros, they can be set directly - it seems the `lyluatex` documenation hasn't quite caught up with the code's change to use the `options` package, options are now specified with:
>```
>\setluaoptions{ly}{keyword}{value}
>```
1) if your `\documentclass{}` macro is using option `draft` eg to find too long hbox issues.  Then no music is embedded in the output, just a reference to which file would be. eg something like:
>```
>\documentclass[a4paper, 12pt, draft]{article}
>``` 
causes the problem
1) remove any lilypond `\book{ formatting option }` directives as these have the effect of causing the music score to be right justified against the edge of the 'paper'.
1) the callout to the lilypond formatter, results in a build up of temporary files - these can be magically removed using the `cleantmp` option, sic
>```
>\usepackage[cleantmp=True]{lyluatex}
>```