# HTCondor Workshop Training Materials

For further reading and documentation, check out the official [HTCondor documentation](http://htcondor.readthedocs.io) and the Danforth Center [Data Science Facility documentation](https://datasci.danforthcenter.org/docs/).

## HTS data alignment and analysis

Data path: `/shares/bioinformatics/workshops/htcondor`

Data files:

* *Setaria viridis* genome: `Sviridis_500_v2.0.fa.gz`
* Illumina sequencing data: `Sviridis_81-79.fastq.gz`

### Walkthrough

#### Create an interactive session

Start by logging in to `stargate.datasci.danforthcenter.org`.

Request an interactive session from the HTCondor cluster:

```bash
condor_submit -i
```

In the interactive session, copy the workshop data and decompressed the genome file.

```bash
cp /shares/bioinformatics/workshops/htcondor/* .

gunzip Sviridis_500_v2.0.fa.gz
```

The first command can be read as copy all the files from `/shares/bioinformatics/workshops/htcondor/` to here.

The `gunzip` command will decompress the file `Sviridis_500_v2.0.fa.gz` and leave behind the file `Sviridis_500_v2.0.fa`.

Exit the interactive session to return back to `stargate`:

```bash
exit
```

#### Create individual HTCondor jobs for individual applications

Job files for this workshop are found in the `align_hts_data` folder of this repository. Job files are named in order from 1-5.

##### Index the genome file

Use `bwa index` to index the *S. viridis* genome (see job file `01.job.bwa.index.txt`.

This job requests 1 CPU and 1G of memory, the default on our system.

To submit the job to the cluster, run:

```bash
condor_submit 01.job.bwa.index.txt
```

You will see the job in the queue by running `condor_q`. 

To see all jobs, run `condor_q -all`. 

To monitor job performance and resource utilization, run `condor_fullstat`.

##### Align HTS data to the indexed genome

Use `bwa mem` to align *S. viridis* Illumina sequencing data to the *S. viridis* genome database created above (see job file `02.job.bwa.mem.txt`).

This job introduces the use of custom variables such (e.g. `name` and `cpus`). This job also shows how to request multiple CPU resources for parallel processing.

To submit the job to the cluster, run:

```bash
condor_submit 02.job.bwa.mem.txt
```

##### Convert SAM alignment data to binary BAM format

Use `samtools view` to convert the SAM alignment file produced above to BAM format (see job file `03.job.samtools.view.txt`).

To submit the job to the cluster, run:

```bash
condor_submit 03.job.samtools.view.txt
```

##### Sort the BAM alignment data

Use `samtools sort` to sort the BAM alignment data (see job file `04.job.samtools.sort.txt`).

This job demonstrates how to request additional memory resources to speed up sorting.

To submit the job to the cluster, run:

```bash
condor_submit 04.job.samtools.sort.txt
```

##### Generate an alignment summary statistics report

Use `samstat` to produce an alignment summary statistics report (see job file `05.job.samstat.txt`).

To submit the job to the cluster, run:

```bash
condor_submit 05.job.samstat.txt
```

#### Create an HTCondor DAG workflow to automate the process

Configure an HTCondor DAG workflow (see workflow file `dag.align_hts_data.txt`) by first defining jobs and then defining the relationship between jobs.

Jobs are defined using the `JOB` keyword, followed by the job name and then the job file.

Relationships between jobs are defined using the `PARENT` keyword, followed by the parent job name, then the `CHILD` keyword, followed by the name of the job that depends on the parent job.

To submit the workflow and run all five steps automatically (with email notification), run:

```bash
condor_submit_dag -notification complete dag.align_hts_data.txt
```
