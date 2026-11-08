# Developer Cookbook — K_PX4
**Stack:** Python 3.11, mavsdk-python, PX4 1.14+, PAX 27B (mission planner), AIOSS_FORMAT
**Domain:** PX4: autopilot integration for Anticloud autonomous drones and vehicles

## PAX-guided mission
```python
from k_px4 import AnticloudPX4Mission

mission = AnticloudPX4Mission(
    connection="udp://:14540",  # PX4 SITL
    pax_model="./pax-27b-q4.gguf",
    aioss_chain="./px4.aioss"
)

# PAX generates mission plan
plan = mission.pax_plan(
    objective="Survey 200m x 200m field for crop damage",
    altitude_m=30
)
print(f"Waypoints: {len(plan.waypoints)}, Est. duration: {plan.duration_s:.0f}s")

# Upload and execute
await mission.arm_and_execute(plan)
print(f"Mission complete: {mission.result}")
print(f"Chain: {mission.chain_hash}")
```

## AIOSS Chain Append
```python
import hashlib, time

def aioss_append(chain_path, payload: bytes, module_id: str):
    entry_hash = hashlib.sha3_256(payload).digest()
    ts = int(time.time_ns()).to_bytes(8, 'big')
    with open(chain_path, 'rb') as f:
        f.seek(-32, 2); prev_hash = f.read(32)
    new_hash = hashlib.sha3_256(prev_hash + entry_hash + ts).digest()
    with open(chain_path, 'ab') as f:
        f.write(ts + entry_hash + new_hash)
    return new_hash.hex()
```
