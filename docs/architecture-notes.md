# Architecture Notes

## Design Style
The platform follows a layered real-time streaming architecture:
- source layer
- ingestion layer
- processing layer
- storage layer
- monitoring and visualization layer

---

## Main Data Path
Transactions / Events → Kafka → Spark Structured Streaming → Validation → Anomaly Detection → Storage → Dashboard

---

## Design Priorities
- reliability
- data quality
- visibility
- scalability
- modularity

---

## Core Components
### Data Sources
Simulated user transactions or business events.

### Kafka
Streaming ingestion layer used to collect and buffer incoming records.

### Spark Structured Streaming
Processing engine used for:
- parsing
- validation
- transformation
- anomaly detection

### Storage
Holds processed outputs:
- valid records
- invalid records
- anomalies
- metrics

### Dashboard
Displays:
- quality score
- failure rate
- throughput
- latency
- anomaly trends

---

## Key Design Principle
This project treats reliability as a first-class concern, not just data movement.

---

## Expected Outcome
The platform should make it easier to:
- trust streaming data
- detect bad records early
- monitor pipeline health
- identify anomalies before they affect analytics# Architecture Notes

## Design Style
The platform follows a layered real-time streaming architecture:
- source layer
- ingestion layer
- processing layer
- storage layer
- monitoring and visualization layer

---

## Main Data Path
Transactions / Events → Kafka → Spark Structured Streaming → Validation → Anomaly Detection → Storage → Dashboard

---

## Design Priorities
- reliability
- data quality
- visibility
- scalability
- modularity

---

## Core Components
### Data Sources
Simulated user transactions or business events.

### Kafka
Streaming ingestion layer used to collect and buffer incoming records.

### Spark Structured Streaming
Processing engine used for:
- parsing
- validation
- transformation
- anomaly detection

### Storage
Holds processed outputs:
- valid records
- invalid records
- anomalies
- metrics

### Dashboard
Displays:
- quality score
- failure rate
- throughput
- latency
- anomaly trends

---

## Key Design Principle
This project treats reliability as a first-class concern, not just data movement.

---

## Expected Outcome
The platform should make it easier to:
- trust streaming data
- detect bad records early
- monitor pipeline health
- identify anomalies before they affect analytics
