# Installing Your Forked Version in Home Assistant

This guide explains how to install your forked version of ha-ble-adv with the split light control feature.

## Option 1: Install via HACS (Recommended for Testing)

### Step 1: Add Your Fork as a Custom Repository

1. Open Home Assistant
2. Go to HACS → Integrations
3. Click the three dots menu (⋮) in the top right
4. Select "Custom repositories"
5. Add your repository:
   - **Repository URL**: `https://github.com/wizzyto12/ha-ble-adv`
   - **Category**: Integration
6. Click "Add"

### Step 2: Install the Integration

1. In HACS, search for "BLE ADV Ceiling Fan / Lamps"
2. You should see your forked version
3. Click on it and select "Download"
4. Restart Home Assistant

### Step 3: Configure the Integration

1. Go to Settings → Devices & Services
2. Click "Add Integration"
3. Search for "BLE ADV"
4. Follow the configuration flow
5. When selecting codec, choose `zhijia_v2_split`
6. Configure your lights with "Cold White Only" and "Warm White Only" types

## Option 2: Manual Installation

If you want to install directly without HACS:

### Step 1: Download Your Fork

```bash
cd /config  # or wherever your Home Assistant config is
git clone https://github.com/wizzyto12/ha-ble-adv.git
```

### Step 2: Copy Files

```bash
# If custom_components directory doesn't exist, create it
mkdir -p /config/custom_components

# Copy the integration
cp -r ha-ble-adv/custom_components/ble_adv_split /config/custom_components/
```

### Step 3: Restart Home Assistant

Restart Home Assistant to load the custom component.

### Step 4: Configure

Follow Step 3 from Option 1 above.

## Option 3: Install from PR Branch

To test this specific PR before merging:

### Via HACS:
1. Add custom repository: `https://github.com/wizzyto12/ha-ble-adv`
2. When installing, select the branch: `copilot/control-multiple-light-brightness`

### Manually:
```bash
cd /config
git clone -b copilot/control-multiple-light-brightness https://github.com/wizzyto12/ha-ble-adv.git ha-ble-adv-split
cp -r ha-ble-adv-split/custom_components/ble_adv_split /config/custom_components/
```

## Should You Rename It?

### For Personal Use (Keep Same Name)
If you're only using this for yourself and might merge back to the original:
- **Keep the domain as `ble_adv_split`**
- **Pros**: Easy to switch back to original, no conflicts with existing setup
- **Cons**: Can only have one version installed at a time

### For Distribution (Rename It)
If you want to distribute this as a separate integration or keep both versions:
- **Change the domain** (see instructions below)
- **Pros**: Can install alongside original, clear it's a fork
- **Cons**: More work, can't easily merge back

## How to Rename (Optional)

If you decide to rename it to make it distinct:

### 1. Update manifest.json
```json
{
  "domain": "ble_adv_split",
  "name": "BLE ADV Split Lights (Fork)",
  "codeowners": ["@wizzyto12"],
  "documentation": "https://github.com/wizzyto12/ha-ble-adv",
  "issue_tracker": "https://github.com/wizzyto12/ha-ble-adv/issues",
  ...
}
```

### 2. Rename Directory
```bash
mv custom_components/ble_adv_split custom_components/ble_adv_split
```

### 3. Update const.py
```python
DOMAIN = "ble_adv_split"
```

### 4. Update All References
Search and replace `ble_adv_split` with `ble_adv_split` in:
- `__init__.py`
- `config_flow.py`
- Any other files that reference the domain

## Recommendation

**For now, I recommend keeping the same name (`ble_adv_split`)** because:

1. **Easier testing**: You can test your changes without conflicts
2. **Easier to contribute back**: If the original author wants to merge your changes
3. **Less maintenance**: You don't need to maintain a separate fork long-term
4. **Your PR is isolated**: The changes are in a separate branch

Once your PR is tested and working, you have three options:
1. **Merge to your main branch** and use your fork permanently
2. **Submit PR to original repo** (if author is accepting contributions)
3. **Rename and distribute** as a separate integration

## Current Status

Your fork is at: `https://github.com/wizzyto12/ha-ble-adv`
Your PR branch is: `copilot/control-multiple-light-brightness`

You can install directly from this branch to test the split light functionality!

## Testing Your Installation

After installation:

1. Go to Settings → Devices & Services → Add Integration
2. Search for "BLE ADV" 
3. During setup:
   - Choose codec: `zhijia_v2_split`
   - For Light 0: Select "Cold White Only"
   - For Light 1: Select "Warm White Only"
   - Set min brightness to 3% (or your preference)
4. Complete setup and test your lights!

## Troubleshooting

### "Integration not found"
- Make sure you restarted Home Assistant after copying files
- Check that files are in `/config/custom_components/ble_adv_split/`

### "Can't find the new light types"
- Make sure you're using the `zhijia_v2_split` codec
- The new types only appear when that codec is selected

### Want to switch between versions?
If testing between original and fork:
1. Remove the integration (Settings → Integrations → BLE ADV → Delete)
2. Delete `/config/custom_components/ble_adv_split/`
3. Install the version you want
4. Restart Home Assistant
5. Add the integration again
