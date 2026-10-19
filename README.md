# IoT-Based Smart Energy Monitoring System

**Project title:** Design and Implementation of an IoT-Based Smart Energy Monitoring System  
**Prototype name:** WattWatch

## Project overview
This ZIP contains a responsive browser-based prototype for monitoring energy consumption. It demonstrates a dashboard, smart-meter status, usage analytics, threshold alerts, report filtering, and CSV export.

## Run it
1. Extract `iot-smart-energy-monitoring-system.zip`.
2. Open the extracted folder.
3. Double-click `index.html` or open it in Chrome, Firefox, Edge, or another modern browser.
4. Use **Refresh readings**, **Simulate test alert**, **Generate report**, and **Export CSV** to test the features.

No additional installation is required.

## Included files
- `index.html` — dashboard layout
- `style.css` — responsive visual styling
- `script.js` — demo readings, chart rendering, alerts, report generation, and CSV export
- `README.md` — project setup and technical notes

## Features demonstrated
- Energy consumption, estimated cost, current demand, and meter status
- Four example meter locations
- Today / this week consumption chart
- Simulated high-usage and connection alerts
- Report filters by period and location
- CSV export for spreadsheet use
- Responsive layout for desktop and mobile

## Proposed full IoT architecture
1. **Sensing layer:** Smart meter or safe, suitably rated energy-measurement module collects voltage, current, power, and cumulative energy.
2. **Edge layer:** A controller such as ESP32 reads supported meter data and validates basic values.
3. **Connectivity layer:** Wi-Fi, Ethernet, or a suitable gateway carries telemetry.
4. **Messaging layer:** An authenticated MQTT broker receives telemetry from devices.
5. **Processing layer:** A backend validates payloads, timestamps readings, handles duplicates, computes aggregates, and detects threshold events.
6. **Data layer:** A database stores meter metadata, readings, and alerts.
7. **Application layer:** The dashboard displays current readings, trends, alerts, and reports.
8. **Notification layer:** Optional email or SMS delivery can be added for important alerts.

## Example telemetry payload
```json
{
  "meter_id": "SM-001",
  "timestamp": "2026-10-17T10:42:00Z",
  "voltage_v": 228,
  "current_a": 4.9,
  "power_kw": 1.12,
  "energy_kwh": 18.4
}
```

## Suggested database tables
- `meters(meter_id, name, location, status, created_at)`
- `readings(id, meter_id, timestamp, voltage_v, current_a, power_kw, energy_kwh)`
- `alerts(id, meter_id, alert_type, message, severity, created_at, acknowledged)`
- `users(id, name, email, password_hash, role)`

## Event-processing steps
1. Authenticate the device or gateway.
2. Validate meter ID, timestamp, and numeric ranges.
3. Reject or quarantine malformed readings.
4. Prevent duplicate events using a unique meter/timestamp key or event ID.
5. Save accepted readings and update consumption summaries.
6. Compare readings with configured thresholds.
7. Create alerts for high usage or missing device updates.
8. Log processing errors and retry temporary failures safely.

## Security recommendations
- Use TLS and unique device credentials for real deployments.
- Do not expose MQTT secrets in browser-side JavaScript.
- Apply role-based access control to dashboards and reports.
- Use parameterized database queries and validate all inputs.
- Back up records and log administrative actions.
- Do not connect mains electricity or sensors without qualified supervision and appropriate isolation.

## Important limitation
This ZIP is a **frontend demonstration**. Meter readings are simulated in the browser. It does not currently connect to real smart-meter hardware, an MQTT broker, a backend API, or a persistent database. Those integrations are proposed next steps and would require additional implementation and testing.
