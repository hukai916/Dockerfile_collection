# A docker image containing R-4.4.1, Monocle3, and related tools

**Docker image:** 
```
docker pull hukai916/monocle3:0.1
```

Use the following R script to check if libraries are loadable:
```
libs <- c("BiocGenerics", "DelayedArray", "DelayedMatrixStats", "limma", "S4Vectors", "SingleCellExperiment", "SummarizedExperiment", "batchelor", "HDF5Array", "terra", "ggrastr", "lme4", "HDF5Array", "monocle3", "Seurat", "SeuratObject")

for (x in libs) {
  if (!requireNamespace(x, quietly = TRUE)) {
    stop("Not found!", x)
  } else {
    message("Found: ", x)
  }
}
```