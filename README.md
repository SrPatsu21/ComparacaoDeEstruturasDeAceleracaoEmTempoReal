# ComparacaoDeEstruturasDeAceleracaoEmTempoReal

## compile

```shell
docker run --rm -v $(pwd):/data -w /data texlive/texlive \
bash -c "pdflatex main.tex; pdflatex main.tex; biber main || true; pdflatex main.tex; pdflatex main.tex"
```
