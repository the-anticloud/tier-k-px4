# L5 Narrow / L2 General Classification — K_PX4
**Platform:** Anticloud | **Tier:** TIER_9_ROBOTICS_IOT | **PAX:** 27B
**IP:** USPTO pending 2026, Anticloud FZ LLE, 0-1.gg | **License:** Apache-2.0

## L5 Narrow
K_PX4 integrates PX4 autopilot with Anticloud: MAVLink communication, mission planning, telemetry processing, and PAX 27B-guided autonomous mission execution. Narrow scope: Anticloud drone/vehicle autonomy — PX4-based platforms only.

## L2 General
L2 General: K_PX4 provides autopilot integration for any Anticloud TIER_9 drone or autonomous vehicle. L_BLACKBIRD drone controllers and custom rover platforms both connect via K_PX4.

## PAX 27B Integration
PAX 27B is the mission planner for K_PX4: given a high-level objective (survey area, deliver payload), PAX generates a structured mission plan that K_PX4 uploads to the autopilot.

## AIOSS Audit Chain
Every mission event (mission hash + MAVLink message hash + telemetry hash + PAX mission plan hash) is chained: H_n = SHA3-256(H_{n-1} || entry_hash_n || timestamp_n).
Offline-verifiable, tamper-evident, zero cloud dependency.

## Regulatory / Compliance
FAA Part 107 (UAV). DO-178C (airborne software). ISO 13482 (autonomous vehicle).
