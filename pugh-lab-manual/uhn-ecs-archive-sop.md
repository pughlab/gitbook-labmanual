# UHN ECS archive SOP

EMC Elastic Cloud Storage (ECS) is an S3 compatiable object storage for UHN archive. With the user id and key you received, you have full control of your group’s ECS bucket. There are differences between the ECS and our regular file systems.

· Although you can create directory like hierarchical structure, the object storage does not have directories. Everything is just an object. A “directory” is just a prefix of an object.

· You will lose permission when you transfer files to ECS

· Awscli (the tool below) will follow symlinks, so it is possible to create endless loop

· AWScli will skip empty directories.



## AWSCLI

We recommend you use awscli to archive data from our cluster. The AWS Command Line Interface (CLI) is a unified tool to manage AWS services. The AWS CLI introduces a new set of simple file commands for efficient file transfers to and from S3 storage.

### To use awscli, you need to put it into your PATH first.

On h4huhndata1 (h4h):

export PATH= /cluster/tools/software/centos7/aws:$PATH

### Configure

You need to set up your credentials before you can use awscli.

aws configure

AWS Access Key ID \[None]: \<Your user id>

AWS Secret Access Key \[None]: \<Your s3 key>

Default region name \[None]:

Default output format \[None]:

Some Examples:

### List File in Your Bucket

aws --endpoint-url=https://ecs.uhn.ca/ s3 ls s3://\<your bucket name>/

### Archive a file(This can only done by those have the write access)

aws --endpoint-url=https://ecs.uhn.ca/ s3 cp ny\_file s3://\<your bucket name>/my\_dir/my\_file

### Download files&#x20;

aws s3 cp --recursive --endpoint-url=https://ecs.uhn.ca s3://pughlab-ecsresgen/my\_dir/my\_file

### Download large files

aws --endpoint-url=[https://ecs.uhn.ca/](https://ecs.uhn.ca/)  **s3api get-object** --bucket pughlab-ecsresgen --key my\_dir/my\_file    --body ./my\_file

### For more help

You can use “aws s3 help” and “aws s3 \<cmd> help”

TIPS:

You can use “--only-show-errors" to only display errors and warnings.

· If ECS returns errors when uploading big files, you can try to use “s3api” instead of “s3 cp”. It disables multipart upload so the speed is slower. Example:

aws --endpoint-url=https://ecs.uhn.ca/ s3api put-object --bucket \<your bucket name> --key my\_dir/my\_file --body my\_file
