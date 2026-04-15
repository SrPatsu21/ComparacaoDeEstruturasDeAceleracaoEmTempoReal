# ComparacaoDeEstruturasDeAceleracaoEmTempoReal

## compile

```shell
docker run --rm -v $(pwd):/data -w /data texlive/texlive bash -c "pdflatex main.tex; bibtex main; makeglossaries main; pdflatex main.tex; pdflatex main.tex"
```
