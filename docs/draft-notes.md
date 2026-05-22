## Compilar archivo latex usando un contenedro textlive usando podman

```bash
# file reporte.tex
cd ~/projects/tarea1-ia && \
podman run --rm -v $(pwd):/data -w /data docker.io/texlive/texlive:latest pdflatex reporte.tex
```