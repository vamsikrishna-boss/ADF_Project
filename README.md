# Azure Data Factory (ADF) – On-Premises to Cloud Data Integration
## 1. Objective
To connect on-premises data sources to Azure Data Lake and process them through Azure Data Factory (ADF) pipelines, applying transformations and storing results in structured layers (Bronze, Silver, Gold) for analytics.

![image](https://github.com/vamsikrishna-boss/ADF_Project/blob/main/ChatGPT%20Image%20Aug%2012%2C%202025%2C%2006_26_32%20PM.png)

## 2. Connecting On-Premises Data to Cloud
### Step 1 – Create Azure Resources
In the Azure portal:

Create a Resource Group.

Inside it, create an Azure Data Factory instance.

### Step 2 – Configure Self-Hosted Integration Runtime (SHIR)
In ADF, navigate to Manage → Integration Runtimes.

Click New → Self-Hosted.

Provide a name and create the runtime.

In the setup window:

Copy the authentication key.

Download and install the latest SHIR software on the on-prem server.

Paste the key in the SHIR installer and register.

Ensure SHIR status is Running.

### Step 3 – Create Linked Service for On-Prem Data
In ADF, create a Linked Service.

Modify connection details and select the SHIR you created.

Provide the on-prem file path or database details.

Run the PowerShell commands as administrator for required setup.

Authenticate with Microsoft username and password.

Click Test Connection → should be successful.

### Step 4 – Create Cloud Storage
In Azure, create a Storage Account.

Inside it, create a container named Bronze.

Create another Linked Service in ADF for Azure Data Lake Storage Gen2.

Provide storage account details and create.

## 3. Working with Datasets and Parameters
Create datasets in ADF with parameterized file names.

Publish all changes.

Move activities using Ctrl+X / Paste into the desired pipeline sections.

For dynamic mapping:

Use JSON mapping in copy activity.

Keep as string if using JSON functions; as object if not.

## 4. API Connections
Open API source file in GitHub.

Click Raw Data and copy the URL.

In ADF, use Web Activity to call APIs.

If needed, use Copy Activity for tabular format ingestion.

## 5. SQL Database Integration
SQL Database – For general relational storage.

SQL Managed Instance – For lift-and-shift from on-prem SQL.

SQL Virtual Machine – For OS-dependent database migration.

Example Table Creation:

sql
Copy
Edit
CREATE TABLE FactBookings (
    booking_id INT PRIMARY KEY,
    passenger_id INT,
    flight_id INT,
    airline_id INT,
    origin_airport_id INT,
    destination_airport_id INT,
    booking_date DATE,
    ticket_cost DECIMAL(10,2),
    flight_duration_mins INT,
    checkin_status VARCHAR(10)
);
## 6. Incremental Loading & Watermarking
Maintain a watermark table to track the last load date.

If the watermark table is empty, add default columns.

Use Lookup Activity to dynamically fetch values for incremental loads.

## 7. Error Handling & Alerts
Create a parent pipeline to execute other pipelines and pass parameters.

Convert array-type parameters to strings using dynamic content.

For failure alerts:

Use Logic Apps to send email notifications when a pipeline fails.

## 8. Data Transformation (Data Flows)
# Bronze Layer
Raw ingestion from on-prem sources.

# Silver Layer
Apply transformations such as:

Derived columns (e.g., uppercase conversion).

Column renaming.

Filtering records (e.g., age > 25).

Removing unwanted country values.

Replacing spaces with underscores in names.

Splitting full names into first and last names.

# Gold Layer
Business-specific aggregated views.

Example: In DimAirlines, aggregate revenue by airline and find top performers.

Remove duplicate or unnecessary columns.

Use window functions for advanced analytics.

## 9. DevOps Integration
Connect ADF to Azure DevOps Repos.

Create pipelines in feature branches.

Use Pull Requests (PR) to merge into the main branch.

Manage CI/CD for deployment.

## 10. Summary
This process enables:

Secure and efficient on-prem to cloud data movement.

Parameterized and reusable pipelines.

Real-time and incremental data loads.

Automated alerts and error handling.

Scalable transformation workflows for business-ready analytics.

## Conclusion

The Azure Data Factory (ADF) on-premises to cloud integration project successfully establishes a secure, scalable, and automated data pipeline architecture that modernizes legacy workflows. By leveraging SHIR for on-prem connectivity, Azure Data Lake for structured storage, and parameterized pipelines for flexibility, the solution enables efficient ingestion, transformation, and delivery of high-quality data into Bronze, Silver, and Gold layers. Integrated monitoring, error handling, and Logic Apps-based alerts ensure reliability, while DevOps integration supports continuous delivery and version control. This modernization not only streamlines data operations but also empowers business teams with timely, accurate insights for better decision-making.
