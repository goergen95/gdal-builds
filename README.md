
<!-- README.md is generated from README.Rmd. Please edit that file -->

# gdal-builds

<!-- badges: start -->
<!-- badges: end -->

The goal of gdal-builds is to create a single image with all geospatial
libs up to date and with R and Python packages using them in alignment.

This builds:

- `gdal-dev`, this is latest commit on osgeo/gdal ready to go (built
  from their gdal_deps) but we add a recent PROJ build for pyproj
- `gdal-dev_python`, adds python packages (we already have GDAL a.k.a.
  osgeo)
- `gdal-dev_python_R`, this adds R and installs packages (not complete)

You can do this to get into an interactive session, you’ll see bleeding
edge GDAL and very recent PROJ and geos installs.

    docker run --rm -ti ghcr.io/mdsumner/gdal-builds:gdal-dev  

To get the whole shebang run this one:

    docker run --rm -ti  ghcr.io/mdsumner/gdal-builds:gdal-dev_python_R 

In there you can start R, and `install.packages` or
`remotes::install_github`, and you can immediately do

``` r
library(reticulate)
gdal <- import("osgeo.gdal")
rioxarray <- import("rioxarray")
geopandas <- import("geopandas")
```

and see that `show_versions()` of them all are in alignment (hurrah!).

IF you want `/vsicurl` then you must run with this, for example:

    docker run --rm -ti --security-opt seccomp=unconfined ghcr.io/mdsumner/gdal-builds:gdal-dev_python_R 

See the other containers here:
<https://github.com/mdsumner/gdal-builds/pkgs/container/gdal-builds>

The `gdal-dev_python` build took a while to get everything aligned -
upgrading pip was the missing piece in the end.

All very much WIP, I can’t otherwise get rocker to do what I want yet
(it’s kind of built in the other direction, first they layer up R base
and get to rstudio, but I just want the libs, then R, and python, then I
can add rstudio and publishing and packages etc.). Python quirks with it
installing binary libs - wheels - and cli tools with a package are a
total obstacle for how I want to work, so I’m starting fresh with what I
understand.
