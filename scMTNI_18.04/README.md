# scMNTI container
The tool depends a older version of gcc, tried to use Ubuntu 18.04 with gcc7, and can successfully install and repeat tutorial examples.

To use the image:
```
docker pull hukai916/scmnti_18.04:0.1
```
Or with Singularity:
```
singularity pull docker://hukai916/scmnti_18.04:0.1
```

After booting up the container:
```
cd scMNTI
# Thenm, run scMNTI like in its official tutorial:
Code/scMTNI -f ExampleData/testdata_config.txt -x50 -l ExampleData/TFs_OGs.txt -n ExampleData/AllGenes.txt -d ExampleData/celltype_tree_ancestor.txt -m ExampleData/testdata_ogids.txt -s ExampleData/celltype_order.txt -p 0.2 -c yes -b -0.9 -q 2 
```