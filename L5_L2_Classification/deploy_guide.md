# Deploy Guide — K_PX4
**Tier:** TIER_9_ROBOTICS_IOT | **Stack:** Python 3.11, mavsdk-python, PX4 1.14+, PAX 27B (mission planner), AIOSS_FORMAT
**Air-gap capable after initial setup.**

## Prerequisites
Python 3.11+, mavsdk-python 1.4+, PX4 SITL simulator or real autopilot. MAVSDK server.

## Environment
PX4 SITL (sim) or Pixhawk hardware. MAVSDK server. T4 GPU for PAX mission planning.

## AIOSS Integration
```bash
aioss init --module K_PX4 --output ./k_px4.aioss
aioss append --chain ./k_px4.aioss --payload ./output.bin --module K_PX4
aioss verify --chain ./k_px4.aioss
```

## Air-Gap Setup
```bash
pip download -r requirements.txt -d ./wheels/
pip install --no-index --find-links ./wheels/ -r requirements.txt
```

## PAX 27B Harness Wiring
```python
from anticloud_pax import PAXHarness
harness = PAXHarness(
    model_path="./pax-27b-q4.gguf",
    module="K_PX4",
    aioss_chain="./K_PX4.aioss",
    classification="L5_NARROW_L2_GENERAL"
)
result = harness.process(input_data)
```

## Verification
```bash
aioss verify --chain ./K_PX4.aioss --verbose
python -m K_PX4.tests.smoke
```
