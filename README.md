# ADF-prject-delete-file-by-size
This project demonstrates an automated data orchestration pipeline in Azure Data Factory to identify and delete files larger than 1 MB from an Azure Blob Storage container. The solution is implemented using a combination of metadata-driven processing, conditional logic, and control flow activities within a pipeline named `pipeline1`. This project highlights key Azure Data Factory features such as metadata inspection, parameterized datasets, conditional branching, and delete activity based on dynamic evaluation.

## Components and Concepts Used

- **Azure Blob Storage**: Acts as the source data store where CSV files are uploaded. Both large (over 1 MB) and small (under 1 MB) files are stored in the same container for evaluation.
  
- **Datasets and Parameters**:
  - `DelimitedText1`: A dataset representing the folder from which metadata of all files is to be fetched.
  - `DelimitedText2`: A parameterized dataset used to access individual files dynamically using the file name passed during the iteration in the ForEach loop.

- **Pipeline Structure**:
  - **Get Metadata1 Activity**: This activity retrieves a list of all child items (files) present in the source folder. The output of this activity serves as the input array for the next `ForEach` activity.
  - **ForEach Activity**: Iterates over each file retrieved from `Get Metadata1`. For each item (file), it performs the following actions:
    - **Get Metadata2 Activity**: Fetches the file name (`itemName`) and its size in bytes (`size`) for the current file being iterated over. This step is essential for applying conditional logic based on file size.
    - **If Condition Activity**: Evaluates whether the file size is greater than 1000 bytes (approximately 1 KB for testing or scaled up to represent 1 MB in real deployments). If the condition is true, the following action is executed:
      - **Delete Activity**: Deletes the file from the blob storage. The deletion is dynamic based on the filename retrieved from metadata. Logging is enabled for traceability, and the operation uses the same Azure Blob Storage linked service.

- **Linked Services**:
  - `AzureBlobStorage1`: Linked service that connects Azure Data Factory to the Azure Blob Storage account for reading, evaluating, and deleting files.

- **Key ADF Concepts Demonstrated**:
  - Dynamic dataset parameterization using expressions (e.g., passing filename to the dataset).
  - Use of metadata activities to drive control logic.
  - Conditional branching in pipelines with the `IfCondition` activity.
  - Programmatic file deletion based on evaluated metadata properties.
  - Logging enabled for deletion activity to track file operations.

## How It Works

1. The pipeline begins by listing all files in the designated blob folder using the `Get Metadata1` activity.
2. The `ForEach` activity processes each file individually.
3. For each file, the pipeline fetches the `itemName` and `size` using `Get Metadata2`.
4. An `IfCondition` activity checks whether the file size exceeds the defined threshold.
5. If true, the `Delete1` activity deletes the file using its name, with activity logging enabled for tracking purposes.

## Usage

This project can be used in scenarios where periodic clean-up of large or outdated files is required from data lakes or blob containers. It helps reduce storage costs and maintain a clean data staging environment in ETL/ELT processes.

## Prerequisites

- Azure Blob Storage with files to evaluate.
- Azure Data Factory with access to the storage account.
- Linked services and datasets configured as per the pipeline setup.

## Conclusion

This project showcases a practical implementation of Azure Data Factory’s ability to manage data lifecycle operations dynamically using metadata and expressions. It reflects good practices for automating data quality, governance, and cost optimization tasks in a cloud environment.
