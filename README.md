# tarea1-ai

Tarea #1 del curso de Inteligencia Artificial.

Este repositorio contiene el archivo LaTeX `reporte.tex` y los recursos necesarios para generar el PDF de la carátula.

## Compilar (Docker / Podman)

Se recomienda compilar usando una imagen de TeX Live en contenedor para evitar instalar paquetes locales.

- Con `docker` (funciona en la mayoría de distribuciones):

```bash
cd ~/projects/tarea1-ia
docker run --rm -v "$(pwd)":/data -w /data docker.io/texlive/texlive:latest pdflatex reporte.tex
```

- Con `podman` en Fedora (SELinux activo, use `:Z` para etiquetar el volumen):

```bash
cd ~/projects/tarea1-ia
podman run --rm -v $(pwd):/data:Z -w /data docker.io/texlive/texlive:latest pdflatex reporte.tex
```

- Con `podman` en Ubuntu (normalmente sin SELinux, no se necesita `:Z`):

```bash
cd ~/projects/tarea1-ia
podman run --rm -v $(pwd):/data -w /data docker.io/texlive/texlive:latest pdflatex reporte.tex
```

### Nota sobre `svg` y `-shell-escape`
Si el documento usa el paquete `svg` (convierte SVG a PDF en tiempo de compilación), `pdflatex` debe ejecutarse con `-shell-escape`:

```bash
podman run --rm -v $(pwd):/data:Z -w /data docker.io/texlive/texlive:latest pdflatex -shell-escape reporte.tex
```

Sin embargo, la imagen de TeX Live en el contenedor puede no incluir `inkscape` o herramientas para procesar SVG. Para evitar este problema, convierte el logo localmente a PDF y agrégalo al repositorio:

Usando ImageMagick (`convert`):

```bash
convert assets/logo.svg assets/logo.pdf
```

O usando `inkscape`:

```bash
inkscape assets/logo.svg --export-filename=assets/logo.pdf
```

Luego en `reporte.tex` usar `\includegraphics{assets/logo.pdf}` en lugar de `svg`.

## Resultado esperado

Después de compilar verás `reporte.pdf` en el directorio del proyecto. Puedes abrirlo con tu visor PDF preferido:

```bash
xdg-open reporte.pdf
```

## Notas
- Si obtienes errores de permisos al montar volúmenes con `podman` en Fedora, asegúrate de añadir `:Z` al final de la especificación `-v`.
- El archivo `assets/logo.pdf` está incluido en el repositorio para simplificar la compilación dentro del contenedor.
