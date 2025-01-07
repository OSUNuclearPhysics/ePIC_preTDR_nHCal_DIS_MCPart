# ePIC_preTDR_nHCal_DIS_MCPart
Analysis of ePIC reconstructed data. Just follow these steps:


### Create directories for condor jobs:

```Sh
mkdirCondor.sh
```

### Create ```S3setup.sh``` to enable access to S3 storage and fill the access keys:


```Sh
#!/bin/bash

export S3_ACCESS_KEY=<S3_ACCESS_KEY>
export S3_SECRET_KEY=<S3_SECRET_KEY>
```

### Submit jobs:

```Sh
condor_submit submitRecoAnalysis.job
```

### Get container and run ```eic-shell```:

```Sh
curl --location https://get.epic-eic.org | bash -s -- -c jug_xl -v 22.11-main-stable
./eic-shell
```

### When jobs finish merge results:

```Sh
cd condor/output/
cmod u+x haddmerge.pl . | tee hadd.log
mkdir ../../output/data
cp <merged_output.root> ../../output/data/output_primary_full.root
```


### Run macros:

```Sh
cd output/data/
root -l -b -q drawMCpartEtaE.C+
root -l -b -q drawMCpartEtaMom.C+
```

### Get the plots in:

```Sh
ls output/outputSpectra/eta_primary/
```

