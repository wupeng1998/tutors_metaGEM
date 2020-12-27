# [metaGEM](https://github.com/franciscozorrilla/metaGEM/) workflow applied to samples from [unseen bio](https://unseenbio.com/)

### Objective
Showcase how the metaGEM workflow can be used to explore the human gut microbiome using whole metagenome shotgun sequencing data obtained from unseen bio's at-home test kits.

### Raw data
Unseenbio test kits were sent for sequencing on September 28 & October 21 2020, resulting in two 101 bp paired end reads sets with IDs S2772Nr1 and S2772Nr3.

### Setup

Obtain 4 main helper files from the [metaGEM](https://github.com/franciscozorrilla/metaGEM/) repo
* Snakefile: contains metaGEM workflow instructions.
* config.yaml: config file used for set up and tweaking pipeline parameters.
* cluster_config.json: config file used for job submissions to cluster.
* metaGEM.sh: parser used for easier user interface with Snakefile.

```
Usage: bash metaGEM.sh [-t TASK] [-j NUMBER OF JOBS] [-c ALLOCATED CORES] [-m ALLOCATED GB MEMORY] [-h ALLOCATED HOURS]
Options:
  -t, --task        Specify task to complete
  -j, --nJobs       Specify number of jobs to run in parallel
  -c, --nCores      Specify number of cores per job
  -m, --mem         Specify memory in GB required for job
  -h, --hours       Specify number of hours to allocated to job runtime
```

Load conda and activate environment:
```
module load Anaconda3
source activate metaGEM
```

Organize paired end reads in subdirectories as shown below.

![Screenshot 2020-12-27 at 18 14 41](https://user-images.githubusercontent.com/35606471/103177108-694a9f80-486f-11eb-8291-cc92dd6785db.png)

### Quality filtering

Submit one quality filtering job per sample, with 2 cores and 20 GB RAM each, with a maximum runtime of 2 hours:
```
bash metaGEM.sh -t fastp -j 2 -c 2 -m 20 -h 2
```

Visualize quality filtering results:
```
bash metaGEM.sh -t qfilterVis
```
![Screenshot 2020-12-27 at 18 44 38](https://user-images.githubusercontent.com/35606471/103177571-9dc05a80-4873-11eb-9f22-2972abff3081.png)

### Assembly

Submit one assembly job per sample, with 48 cores and 250 GB RAM each, with a maximum runtime of 100 hours:
```
bash metaGEM.sh -t megahit -j 2 -c 48 -m 250 -h 100
```
