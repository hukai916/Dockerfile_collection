# scMNTI container
The tool seems to depend on an older version of gcc (gcc v14 does not work, but gcc v8 and below seems to work well).
Therefore, attempted to use Ubuntu 18.04 with gcc7, and can successfully install and repeat tutorial examples.

To use the image:
```
docker pull hukai916/scmnti_18.04:0.1
```

cd scMNTI
# Thenm, run scMNTI like in its official tutorial:
Code/scMTNI -f ExampleData/testdata_config.txt -x50 -l ExampleData/TFs_OGs.txt -n ExampleData/AllGenes.txt -d ExampleData/celltype_tree_ancestor.txt -m ExampleData/testdata_ogids.txt -s ExampleData/celltype_order.txt -p 0.2 -c yes -b -0.9 -q 2 
```

Dev notes:
```
docker build --platform linux/amd64 -t hukai916/scmnti_18.04:0.1 . # failed to run, only works on arm64
```
