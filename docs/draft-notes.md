## Compilar archivo latex usando un contenedro textlive usando podman

```bash
# file reporte.tex
cd ~/projects/tarea1-ia && \
podman run --rm -v $(pwd):/data:Z -w /data docker.io/texlive/texlive:latest pdflatex reporte.tex


# compilar varias veces para resolver referencias cruzadas
podman run --rm -v $(pwd):/data:Z -w /data docker.io/texlive/texlive:latest bibtex reporte && podman run --rm -v $(pwd):/data:Z -w /data docker.io/texlive/texlive:latest pdflatex reporte.tex && podman run --rm -v $(pwd):/data:Z -w /data docker.io/texlive/texlive:latest pdflatex reporte.tex

###
# Voy a recompilar en el orden correcto (pdflatex → bibtex → pdflatex ×2) dentro del contenedor Podman para que BibTeX recoja las citas nuevas y se resuelvan las referencias cruzadas.
podman run --rm -v $(pwd):/data:Z -w /data docker.io/texlive/texlive:latest pdflatex reporte.tex && podman run --rm -v $(pwd):/data:Z -w /data docker.io/texlive/texlive:latest bibtex reporte && podman run --rm -v $(pwd):/data:Z -w /data docker.io/texlive/texlive:latest pdflatex reporte.tex && podman run --rm -v $(pwd):/data:Z -w /data docker.io/texlive/texlive:latest pdflatex reporte.tex

# Agregaré \usepackage{url} al preámbulo de reporte.tex para soportar el comando \url{...} generado por BibTeX, luego recompilaré (pdflatex→bibtex→pdflatex×2).
git -C /home/chris/projects/tarea1-ia add reporte.tex && git -C /home/chris/projects/tarea1-ia commit -m "LaTeX: add \usepackage{url} to preamble to support \url in .bbl"
```