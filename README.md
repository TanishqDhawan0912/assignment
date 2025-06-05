# Data Ingestion API System

A RESTful API system for submitting data ingestion requests and checking their status. The system fetches data from a simulated external API, processes it in batches asynchronously, and respects rate limits and priority ordering.

## Features

-   **Ingestion API** (`POST /ingest`): Submit a list of IDs with a priority level for processing
-   **Status API** (`GET /status/:ingestionId`): Check the status of an ingestion request
-   **Priority-based processing**: Higher priority requests are processed before lower priority ones
-   **Rate limiting**: Process at most 3 IDs every 5 seconds
-   **Batched processing**: Process data in batches of up to 3 IDs
-   **Asynchronous processing**: Non-blocking background job processing

## Technical Implementation

-   **Framework**: Express with TypeScript
-   **Storage**: In-memory data store for simplicity
-   **Async Processing**: Background job queue with priority sorting
-   **Testing**: Comprehensive test suite for API endpoints

## Getting Started

### Prerequisites

-   Node.js (v22 or higher)
-   pnpm (or npm)

### Installation

1. Clone the repository:

2. Install dependencies:

    ```
    pnpm install
    ```

3. Start the development server:

    ```
    pnpm dev
    ```

4. Build for production:
    ```
    pnpm build
    pnpm start
    ```

## API Documentation

### Ingestion API

-   **Endpoint**: POST /ingest
-   **Input**: A JSON payload containing a list of IDs (integers) and Priority
    ```json
    {
        "ids": [1, 2, 3, 4, 5],
        "priority": "HIGH" // can be HIGH, MEDIUM, LOW
    }
    ```
-   **Output**: A unique ingestion_id as a JSON response
    ```json
    {
        "ingestionId": "abc123"
    }
    ```

#### Ingestion API Success Screenshot
![image](https://github.com/user-attachments/assets/38329654-a7d1-4d62-abdb-2b62b4015b2e)


### Status API

-   **Endpoint**: GET /status/:ingestionId
-   **Output**: A JSON response showing the overall status and details of each batch
    ```json
    {
        "ingestionId": "abc123",
        "status": "triggered",
        "batches": [
            { "batchId": "uuid-1", "ids": [1, 2, 3], "status": "completed" },
            { "batchId": "uuid-2", "ids": [4, 5], "status": "triggered" }
        ]
    }
    ```

#### Status API Success Screenshot
![image](https://github.com/user-attachments/assets/1618aa2f-f637-4930-ba3d-1c45466f85ed)


## Design Choices

-   **In-memory store**: Used for simplicity, but can be replaced with a database in production.
-   **Priority-based queue**: Implemented using a sorted array of batches to ensure higher priority requests are processed first.
-   **Singleton pattern**: Used for the ingestion service to maintain a single processing queue.
-   **Async processing**: The system processes batches asynchronously, allowing for high throughput.
-   **Rate limiting**: Implemented using timestamps to ensure the processing rate does not exceed the limit.

## Deployment

The application can be deployed to any Node.js hosting platform:

1. Build the TypeScript code:

    ```
    pnpm build
    ```

2. Start the production server:
    ```
    pnpm start
    ```
