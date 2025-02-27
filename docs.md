# Documentation 

## Pre-requisites

Make sure to have configured the XPACK license and the plugin as described 
in the [README](README.md#configuration) document. 

## AWS Batch pro executor 

The `xpack-amzn` plugin implements an advanced version of AWS Batch executor 
for Nextflow that allows using a shared file system in place of AWS S3 bucket 
as the pipeline work directory to ingest pipeline data. 

## Use of AWS EFS file system with Nextflow 

AWS [EFS](https://aws.amazon.com/efs/) is a shared file-system based on the 
NFS protocol provided by AWS. 

To learn about EFS details and how to create EFS instance check the AWS documentation
at [this link](https://docs.aws.amazon.com/efs/latest/ug/creating-using-create-fs.html).

Once you have created one or more file systems, to make accessible in your 
pipeline execution add the `efsVolumes` declaration in your configuration 
file as shown below:

```
aws.batch.efsVolumes.'efs-1234567890'.mountPath = '/mnt/fsx'
```

In the above snippet replace `efs-1234567890` with the ID of your EFS instance and 
the path `/mnt/fsx` with one of your choice. 

Repeat the above configuratio for all file system instance you want to configure 
in your pipeline. 


| Config option 	                  | Description 	              |
|---	                              |---	                        |
| `aws.batch.efsVolumes.'<ID>'.mountPath`  | The host path to which the file system should be made available (default: none )
| `aws.batch.efsVolumes.'<ID>'.rootPath`   | The file system directory that should be made available throught the mount point (optional, default: `/`) 
| `aws.batch.efsVolumes.'<ID>'.readOnly`   | When `true` only allows the read of files (optional, default: `false`)

Note: Replace the `<ID>` placeholder in the above table with your EFS file system identifier.  

## Use of a POSIX-based shared file-system 

Using `xpack-amzn` plugin you can use any POSIX-based shared file-system, along with 
AWS Batch such as [AWS FSx](https://aws.amazon.com/fsx/), [Qumolo](https://qumulo.com/), [Weka](https://www.weka.io/), etc.

The configuration of such file systems is out of the scope of this guide and it's 
expected to be managed by the user. 

To use this file system as work directory in your Nextflow pipeline execution, 
you will only need to specify a shared directory path in the Nextflow command-line 
with the `-w` option, e.g. 

```
nextflow run <MY-PIPELINE> -w /mnt/some/shared/dir
```

To access shared file paths, other than the pipeline work directory, it is required to 
declare such file or directory paths using the Nextflow `volumes` configuration 
option as described as [this link](https://www.nextflow.io/docs/latest/awscloud.html#volume-mounts).
