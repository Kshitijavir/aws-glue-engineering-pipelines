# AWS Glue Production Pipelines

This repository contains AWS Glue ETL pipelines implementing core batch data engineering patterns using Amazon S3 and PySpark.

## Architecture

S3 (Raw Layer)  
   ↓  
AWS Glue ETL Job  
   ↓  
S3 (Refined Layer - Parquet)

## Pipelines Covered

- CSV to Parquet Conversion  
- Multi-format to Parquet (CSV / JSON / TXT)  
- SCD Type 1 (Overwrite)  
- Incremental Append Load  
- Glue Job Bookmarks  
- SCD Type 2 (Hash-based)  
- Soft Delete Handling  
- File-based CDC  
- Partitioned Writes  
- Pushdown Predicate  
- Multi-key SCD2  
- Schema Validation (Schema Guard)  
- External Library Injection (.whl / .zip)  
- Scheduled Glue Jobs  

## Tech Stack

- AWS Glue (Spark 3.x)  
- Amazon S3  
- PySpark  
- DynamicFrame  
- Partitioned Parquet  

## Repository Structure

Each pipeline has its own folder containing:
- Glue job script  
- Sample input data  
- Execution notes  

## Objective

To build production-ready AWS Glue pipelines covering overwrite, incremental, CDC, SCD, partitioning, and optimization patterns for real-world data engineering scenarios.
