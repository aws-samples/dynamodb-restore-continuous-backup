# DynamoDB continuous backup restore utility

Amazon DynamoDB is a fully managed NoSQL database service that provides fast and predictable performance with seamless scalability.  
DynamoDB tables can be [fully backed up to S3](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-importexport-ddb-part2.html) and [restored if necessary on another DynamoDB table](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-importexport-ddb-part1.html) using Amazon Data Pipeline.  
However, some customers may need to continuously backup their DynamoDB tables in order to restore time-framed dataset. The [DynamoDB Continuous Backup Utility](https://github.com/awslabs/dynamodb-continuous-backup) provides a simple way to automate continuous backup operations of DynamoDB tables on an S3 bucket.  
This project gives a full automated Datapipeline code example to help our customers restore a DynamoDB table which has been backed up via the [DynamoDB Continuous Backup Utility](https://github.com/awslabs/dynamodb-continuous-backup).

## How to

This Datapipeline template aims to show you how to build a stack that automates the building of an EMR cluster, loads data on S3 with Hives and retore them on a DynamoDB table.  
You will have to provide as parameters of the template some schema metadata as well as the S3 prefix for all data to be imported. Hopefully, backup data from the [DynamoDB Continuous Backup Utility](https://github.com/awslabs/dynamodb-continuous-backup) are sorted by date formated directories which will help to target the right data source to restore.

## Running the pipeline

To run the pipeline, go to the DataPipeline service, and choose the provided dynamodb-restore-continuous-backup.json file as a template (Import a definition).  
Then, fill in needed paramaters, knowing that they will stand for the following purposes:

 * Input S3 prefix: The s3 prefix from which the source data are to be imported in your targeted DynamoDB table.
 * DynamoDB write throughput ratio: The write throughput ratio to be used for the import operation. Don't hesitate to artificially scale out the write capacity value on your DynamoDB console if you want to get your data restored in a reasonnable time.
 * DynamoDB table name: The targeted Dynamo table name.
 * Dynamodb Column Mappings: A comma seperated column definitions. For e.g. VendorID string,TestProjectID string,CreateDate string,DefaultStage string,ModifiedDate string,Name string,TenantID string,DefaultTenantTestObjectID string,ProductDisplayName string
 * S3 to DynamoDB Column Mapping: A comma separated mapping of S3 to DynamoDB. For e.g. VendorID:VendorID,TestProjectID:TestProjectID,CreateDate:CreateDate,DefaultStage:DefaultStage,ModifiedDate:ModifiedDate,Name:Name,TenantID:TenantID,DefaultTenantTestObjectID:DefaultTenantTestObjectID,ProductDisplayName:ProductDisplayName. Please take care of not using spaces in between the commas.
 * Newimage Field Mapping: A comma seperated json path for the DynamoDB backup objects loaded in memory. It is always formated as 'NewImage.{field_name}.{field_type}'. For e.g. NewImage.VendorID.S,NewImage.TestProjectID.S,NewImage.CreateDate.S,NewImage.DefaultStage.S,NewImage.ModifiedDate.S,NewImage.Name.S,NewImage.TenantID.S,NewImage.DefaultTenantTestObjectID.S,NewImage.ProductDisplayName.S
 * Log Uri: S3 log path to capture the pipeline logs.


 # License

 Copyright 2017 Amazon.com, Inc. or its affiliates. All Rights Reserved.
 SPDX-License-Identifier: MIT-0
