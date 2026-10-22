Act as a Senior Enterprise Architect, Site Reliability Engineer, and Technology Risk Reviewer.

Objective:
Review the provided architecture documents, LLDs, source code (if available), runbooks, incident records, RCAs, PIRs, monitoring configurations, change records, and operational evidence. Identify architectural, operational, resilience, and implementation-level weaknesses that could contribute to outages, transaction failures, settlement issues, performance degradation, or regulatory incidents.

Focus specifically on:
- August 3, 2026 FX-Clear incident.
- Prior CCIL website unavailability incident.
- Similar incidents observed over the last 12 months.

Perform the review across the following areas:

1. Architecture Review
- Assess logical and physical architecture.
- Identify tight coupling, architectural bottlenecks, SPOFs, scalability constraints, and integration risks.
- Review API design, messaging patterns, dependencies, and failure propagation paths.
- Evaluate resilience, failover, DR mechanisms, and alignment with RTO/RPO objectives.
- Identify components where failure of a single transaction, service, queue, thread, process, or database operation could impact overall system availability.

2. Failure Handling & Code Review
- Review LLDs and source code for exception handling, retry logic, timeout handling, circuit breakers, queue management, concurrency controls, deadlock risks, transaction management, and failover behavior.
- Highlight areas where application failures could halt processing instead of gracefully isolating failed transactions.
- Recommend patterns where exceptions should be captured, logged, alerted, and processing should continue safely.
- Identify error-handling anti-patterns and implementation gaps.

3. Reconciliation & Position Monitoring
- Assess trade-versus-settlement reconciliation controls.
- Identify gaps in break detection, exception management, alerting, and manual fallback procedures.
- Review monitoring of unmatched positions and delayed settlements.

4. Monitoring & Observability
- Evaluate application, infrastructure, and business-process monitoring coverage.
- Identify monitoring gaps, alert fatigue, noisy alerts, missing KPIs, and lack of predictive indicators.
- Assess whether failures could have been detected earlier through better observability.

5. Incident & Problem Management
- Analyze 12 months of incidents for recurring themes and systemic weaknesses.
- Correlate incidents by failure mode, affected component, and root cause.
- Evaluate quality of RCA findings and remediation effectiveness.
- Identify recurring issues that indicate unresolved architectural or operational weaknesses.

6. Change & Release Management
- Review changes deployed before major incidents.
- Assess evidence of testing, rollback planning, CAB approvals, deployment controls, and release governance.
- Identify changes that may have introduced risk or insufficiently tested failure scenarios.

7. Environment & Configuration Management
- Identify configuration drift, undocumented settings, environment inconsistencies, and manual operational dependencies.
- Review use of Infrastructure as Code and centralized configuration management.
- Highlight risks arising from undocumented or manually managed configurations.

For every finding provide:
- Finding ID
- Severity (Critical/High/Medium/Low)
- Area (Architecture, Code, Operations, Monitoring, Change, DR, etc.)
- Evidence reviewed
- Observation
- Business/Operational impact
- Potential link to past incidents
- Recommended remediation
- Confidence level (High/Medium/Low)

Generate outputs as:
1. Executive Summary
2. Top Critical Risks
3. Incident Correlation Analysis
4. Architecture & Resilience Findings
5. Code-Level Failure Handling Findings
6. Operational Effectiveness Findings
7. Recommended Remediation Roadmap
8. Areas Requiring Human Validation

Important:
Treat all conclusions as potential review leads only. Do not fabricate evidence. Every finding must reference supporting source material and indicate whether the conclusion is directly evidenced or inferred.
