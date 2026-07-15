# School Heating Asset Health & Fuel Optimisation Service

**Service 4 — Technical Description**
Municipality of Athens

---

## 1. Service Description

Service 4 addresses a critical but frequently overlooked dimension of school building management: the ability to continuously verify that classroom and common space thermal conditions meet the standards required for healthy and productive learning environments.

Most municipal schools in Athens operate diesel-fired heating systems that are often ageing, imprecisely controlled and difficult to monitor without dedicated sensor infrastructure. This service exploits the thermostat sensor data available across a subset of the school portfolio, combined with outdoor weather observations and electricity consumption telemetry, to build a continuous picture of indoor thermal conditions at each instrumented school.

The output is a **compliance monitoring capability** that identifies schools where thermal conditions are systematically falling outside acceptable bounds, quantifies the duration and severity of non-compliance and gives the central education and facilities management teams the evidence needed to prioritise corrective action.

---

## 2. Operational Context

The service covers the subset of municipal schools equipped with thermostat sensors, with a design that accommodates progressive expansion as sensor coverage is extended to additional buildings. Each school presents a distinct thermal behaviour shaped by its construction era, fabric quality, heating system type and capacity, spatial layout and occupancy schedule. Older school buildings in particular exhibit significant **thermal inertia**: indoor temperatures respond slowly to outdoor conditions and heating system adjustments, making simple threshold monitoring insufficient for capturing the true comfort experience of occupants across the school day.

Thermal comfort assessment is grounded in established temperature standards for occupied educational spaces, which specify minimum and maximum operative temperature bounds during teaching hours and distinguish between requirements for classrooms, gymnasiums, administrative spaces and ancillary areas. The service treats these bounds as **hard compliance references** rather than advisory guidelines, classifying each monitored interval as compliant, borderline or non-compliant and accumulating compliance statistics across the school day, week and term.

Outdoor temperature data are integrated to contextualise indoor readings and separate heating system failures from weather-driven exceedances that no realistic heating capacity could prevent. Occupancy schedules derived from the Greek school calendar, term dates and published timetable structures define which intervals are subject to compliance assessment. Periods outside teaching hours are excluded from compliance scoring but are retained for heating system efficiency analysis carried out in conjunction with Service 5.

---

## 3. Data Inputs

| Input | Resolution | Role |
|---|---|---|
| Thermostat temperature readings (°C) | 15 min | Indoor thermal conditions per school or zone; primary compliance evidence |
| Outdoor weather observations | 15 min / hourly | Contextualisation of indoor readings; degree-day adjusted expectations |
| Electricity consumption telemetry (kWh) | 15 min | Corroborating evidence of heating and auxiliary system operation |
| Diesel consumption data (via Service 5) | 15 min | Cross-reference for fuel availability issues |
| Building metadata (Service 1 core datasets) | Static | Construction era, area and category informing the thermal model |
| Greek school calendar, term dates & timetables | Static / periodic | Definition of teaching-hour intervals subject to compliance assessment |

---

## 4. Methodology and Technical Approach

The service constructs a **thermal profile** for each instrumented school by combining thermostat sensor readings with outdoor temperature observations to compute the indoor-outdoor temperature differential across each monitored interval. This differential is compared against a heating degree-day adjusted expectation derived from a **building-specific thermal model** calibrated on historical sensor and weather data. This allows the service to distinguish between intervals where indoor temperatures are low because outdoor conditions are exceptionally severe and intervals where indoor temperatures are low because the heating system is underperforming relative to conditions it should be able to handle.

### Non-Compliance Classification

Non-compliance events are classified by **severity** and **cause**:

- **Severity** is assessed on the magnitude and duration of the temperature exceedance relative to the applicable standard.
- **Cause** classification distinguishes between:
  - *Heating system capacity failures* — the system is operating but cannot maintain the required temperature
  - *Fuel availability issues* — flagged through cross-reference with the diesel consumption data monitored by Service 5
  - *Sensor or data quality anomalies* — spurious readings that do not reflect genuine indoor conditions

The cause classification guides the recommended corrective action associated with each detected event.

### Portfolio-Level Analysis

The service aggregates compliance statistics across all instrumented schools to produce a **ranked compliance league table**, updated weekly, identifying the schools with the most persistent or severe thermal comfort deficiencies. Trend analysis tracks whether individual schools are improving or deteriorating across successive terms, supporting accountability conversations between the central facilities team and individual school administrations.

---

## 5. Service Outputs

Per instrumented school and monitoring interval:

- Thermal compliance status (compliant / borderline / non-compliant)
- Indoor temperature reading and prevailing outdoor temperature context
- Severity classification for any non-compliant interval
- Cause classification where sufficient evidence is available

Daily and weekly compliance summaries per school:

- Proportion of teaching-hour intervals in each compliance category
- Cumulative duration of non-compliant conditions
- Estimated heating system performance ratio relative to weather-adjusted expectations

Portfolio level:

- A weekly compliance report ranking all instrumented schools by overall compliance rate, flagging schools with deteriorating trends or acute non-compliance episodes requiring immediate attention

All outputs reference the specific sensor, timestamp and weather data used, supporting traceability and enabling operators to cross-check flagged events against their own observations.

---

## 6. Evaluation and Validation

| Indicator | Description |
|---|---|
| **Detection accuracy** | Agreement between service compliance classifications and independent spot-check temperature measurements conducted by facilities staff at a sample of schools. |
| **Thermal model accuracy** | Mean absolute error between predicted indoor temperature under normal heating operation and actual thermostat readings across a held-out validation period, stratified by outdoor temperature range and school construction category to identify systematic biases. |
| **Cause classification false positive rate** | Proportion of heating system failure alerts that, on investigation, are attributed to sensor faults or data transmission issues rather than genuine equipment problems. |

---

## 7. Implementation and Availability

The service is accessible through an **authenticated REST API** delivering per-school compliance status updates, daily and weekly summary reports and portfolio-level tables in structured JSON format.

The processing pipeline is structured as independent versioned modules:

1. Thermostat sensor ingestion
2. Weather data integration
3. Thermal model computation
4. Compliance classification
5. Report generation

Each module can be updated without disrupting the live monitoring stream. **Sensor coverage metadata** is maintained as a first-class configuration parameter, enabling the service to adapt automatically as new schools are instrumented and added to the monitored fleet.
