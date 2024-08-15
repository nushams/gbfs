# GBFS Monitoring Solution

## Introduction

This project aims to monitor JSON files published by various GBFS (General Bikeshare Feed Specification) providers, extract vehicle statistics, and visualize historical data on a dashboard. The solution leverages Azure services like **Logic Apps**, **Cosmos DB**, and **Synapse Analytics** to build a scalable and observable infrastructure.

---

## Architecture Overview

### High-Level Architecture

The solution consists of the following key components:

1. **Azure Logic Apps**: Automates the fetching of JSON data from GBFS providers on a schedule.
2. **Azure Cosmos DB**: Serves as the NoSQL database for storing historical data fetched from the providers.
3. **Azure Synapse Analytics**: Provides a data pipeline for processing and visualizing statistics on a dashboard.

### Data Flow

1. **Data Ingestion**:
   - Azure Logic Apps periodically fetches JSON data from GBFS providers.
   - The data is parsed, and relevant statistics (e.g., the number of available vehicles) are extracted.
   - The extracted data is then stored in Cosmos DB for historical reference.

2. **Data Processing & Analysis**:
   - Azure Synapse Analytics processes the data from Cosmos DB to generate reports and visualizations.
   - These visualizations are published to a Synapse workspace, providing a dashboard view of the bike-sharing data trends.

3. **Alerts & Monitoring**:
   - Alerts can be set up in Azure Monitor to notify when certain conditions (e.g., low vehicle availability) are met.

**Architecture Diagram**: The architecture diagram can be seen in [this file](./diagram.eraserdiagram).

**Example of a dashboard**: Exported snapshot of a Synapse dashboard is [here](./dashboard-result.png).

---

## Steps to Build and Run the Solution

### 1. Set Up Azure Resources

#### Step 1: Create Azure Resources

Using the Azure Portal, create the necessary resources:
1. **Resource Group**: Create a new resource group for all project resources.
2. **Azure Cosmos DB**: Create a Cosmos DB instance with a database and container to store GBFS data.
3. **Azure Synapse Workspace**: Set up a Synapse workspace for data processing and dashboard creation.
4. **Azure Logic App**: Create Logic Apps to automate data ingestion from the GBFS providers.

#### Step 2: Set Up Azure Cosmos DB

1. Create a database (e.g., `GBFSData`) in Cosmos DB.
2. Create a container (e.g., `VehicleStats`) with an appropriate partition key (e.g., `/providerName`).

#### Step 3: Set Up Logic Apps

1. **Create a New Logic App**: Use the Azure Portal to create a Logic App that will trigger on a schedule (e.g., every 15 minutes).
2. **Define Workflow**:
   - Create a timer trigger or another trigger.
   - Use the `HTTP` action to call the GBFS provider URLs.
   - Parse the JSON response.
   - Extract relevant statistics (e.g., number of available vehicles, number of stations).
   - Insert the parsed data into Cosmos DB using the `Azure Cosmos DB` connector.

#### Step 4: Set Up Azure Synapse Analytics

1. **Create a Data Pipeline**: Create a pipeline in Synapse to process data from Cosmos DB.
2. **Data Transformation**: Use Synapse SQL scripts to aggregate and transform data for dashboard visualizations.
3. **Dashboard Creation**:
   - Use Synapse Studio to create visualizations based on the processed data.
   - The dashboard could include trends in vehicle availability, usage patterns, and comparisons across providers.

### 2. Deploy and Test the Logic App

- Once the Logic App is created, manually trigger it to test the workflow.
- Verify that data is being stored correctly in Cosmos DB.
- Check the Synapse dashboard to ensure that data is being processed and visualized as expected.

### 3. Set Up Monitoring and Alerts

- Use **Azure Monitor** to track the performance of the Logic App and Cosmos DB.
- Set up alerts to notify you when specific thresholds (e.g., low vehicle availability) are met.

---

## Things to Improve with More Time

1. **Write proper app using i.e. Functions App**: Implement proper solution for higher loads and process more provider data.
2. **Real-Time Data Processing**: Integrate Azure Event Hub or Stream Analytics for real-time data processing and dashboard updates.
3. **Automated Testing**: Implement more extensive testing to ensure reliability.
4. **Scalability**: Use multiple Logic Apps or Azure Functions for parallel processing if the number of GBFS providers increases significantly.

---

