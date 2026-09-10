# ⚡ Flash MVP: Incident Management & Automated Regression Pipeline

> **Core Objective:** Rapid anomaly isolation, automatic Markdown "Anomaly Passport" generation, and zero-day BDD regression test synthesizing from OpenTelemetry microservice traces.



## 🏗 System Architecture (Lean Setup)

[cURL / Client] --(gRPC)--> [checkoutservice :50050]
|
(Unhandled Payload)
v
[paymentservice :50051] --> (CRASH / 500)
|
(OTel)
v
[OTel Collector :4317] --> [Jaeger API :16686]
|
(passport_generator.py)
v
+----------------------------+
| PASSPORT_INC-01.md         |
| tests/reproduce_issue.feature|
+----------------------------+


## 🚀 60-Second Demo Execution Flow

### 1. Launch Lightweight Infrastructure
```bash
docker compose -f docker-compose.lean.yaml up -d

2. Inject Anomaly (Ammo Payload)
Inject an invalid currency parameter to trigger an unhandled crash in paymentservice:
Bash
grpcurl -plaintext -d '{"user_id": "user-flash-mvp", "user_currency": "INVALID_CURRENCY"}' localhost:50050 oteldemo.CheckoutService/PlaceOrder

3. Generate Incident Artifacts 
Execute the automated trace analyzer to extract telemetry and synthesize response assets:
Bash
python3 scripts/passport_generator.py

### 📄 Produced Artifacts
Anomaly Passport: PASSPORT_INC-01.md
Contains Trace ID, Isolation status ($P_{isolated}$), entry point, and evidence vector.

BDD Regression Guard: tests/reproduce_issue.feature
Auto-generated Gherkin scenario ready for CI/CD pipeline integration.
