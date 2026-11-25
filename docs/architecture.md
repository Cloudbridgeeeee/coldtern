# ColdTern — proposed AWS architecture

ColdTern uses AWS to collect readings reliably through unstable power, detect anomalies quickly and keep complete temperature records for every room.

## Request flow

1. Sensors
2. AWS IoT Core
3. AWS Lambda
4. SageMaker AI
5. DynamoDB
6. Operator review
7. Maintenance

## Services

### AWS IoT Core

**Role:** Collect sensor readings

Receives temperature, door, defrost and compressor readings from gateways over secure MQTT, with buffered delivery after power or network outages.

### Amazon SageMaker AI

**Role:** Detect unusual behaviour

Trains per-unit anomaly models that learn normal defrost and loading patterns, so alerts flag real drift rather than routine events.

### Amazon S3

**Role:** Keep temperature records

Stores the complete history of readings and exported logs, encrypted and retained to meet client and regulatory record-keeping needs.

### Amazon DynamoDB

**Role:** Serve live state and alerts

Holds current readings, alerts, acknowledgements and maintenance histories with fast reads for live dashboards.

### AWS Lambda

**Role:** Score readings and notify

Scores incoming readings against models, raises and escalates alerts, and generates daily temperature reports.

## Principles

- **Built for power cuts** — Gateways buffer readings and catch up automatically.
- **Read-only** — We never control compressors or controllers.
- **Fewer false alarms** — Models understand defrosts and loading.

## Safeguards

- **Operators decide** — Alerts are reviewed by your operators before maintenance is raised.
- **Keep your safety alarms** — ColdTern complements, never replaces, your existing temperature alarms and safety systems.
- **No product release decisions** — Whether stock is safe after an excursion is decided by your QA process.
- **Client data separated** — Each client’s rooms and records are access-controlled.

## MVP scope

We import historical readings from a set of cold rooms, train unit-level models, and run operator-reviewed alerts and a maintenance history for each unit over the pilot period.

- Historical reading import
- Unit-level anomaly models
- Operator-reviewed alerts
- Maintenance history per unit
- Weekly performance report
