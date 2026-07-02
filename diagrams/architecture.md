                  Web / Mobile Application
                           │
                    JSON Ride Events
                           │
                           ▼
                  Azure Event Hub (Kafka)
                           │
                           ▼
          Apache Spark Structured Streaming
                           │
        ┌──────────────────┴──────────────────┐
        │                                     │
Historical Ride Files                 Streaming Events
        │                                     │
        └──────────────Merge──────────────────┘
                           │
                           ▼
                  Bronze Delta Tables
                           │
                           ▼
             Silver Layer (Cleaning + OBT)
                           │
                           ▼
         Gold Layer (Fact + Dimension Tables)
                           │
                           ▼
               Interactive Power BI Dashboard
