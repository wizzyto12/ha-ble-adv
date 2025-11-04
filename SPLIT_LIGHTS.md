# Split Cold/Warm Light Control

## Overview

This feature allows you to control two separate white lights independently using the cold and warm channels of your Zhi Jia controller. Instead of using color temperature control, each channel becomes a separate brightness-only light entity.

## Use Case

If you have a Zhi Jia controller (with brightness + color temperature features) connected to two regular white lights (not a CCT light), you can now control each light's brightness independently without color temperature adjustment.

## Configuration

To use this feature with your Zhi Jia v2 controller:

1. During device setup, when asked to select a codec, choose `zhijia_v2_split`
2. In the configuration flow, when configuring light entities, you'll see new options:
   - **Cold White Only** - Controls only the cold channel
   - **Warm White Only** - Controls only the warm channel
3. Configure two light entities:
   - **Light 0**: Select type `Cold White Only` - This controls the first white light (cold channel)
   - **Light 1**: Select type `Warm White Only` - This controls the second white light (warm channel)
4. Set the minimum brightness for each light (as low as your app allows, typically 3%)

## Features

Each light entity supports:
- **On/Off control**: Turn each light on or off independently
- **Brightness control**: Adjust brightness from minimum (configured via `min_brightness` setting) to 100%
- **No color temperature**: Pure brightness control without CT adjustments

## Example Configuration

When setting up your integration through the Home Assistant UI:

**Step 1: Select Codec**
```
Codec: zhijia_v2_split
```

**Step 2: Configure Light Entities**

Light 0 Configuration:
```
Type: Cold White Only
Min Brightness: 3% (or as low as your app allows)
Refresh on Start: Optional (check to refresh state on HA restart)
```

Light 1 Configuration:
```
Type: Warm White Only
Min Brightness: 3% (or as low as your app allows)
Refresh on Start: Optional (check to refresh state on HA restart)
```

The configuration flow will show these as dropdown options in your selected language.

## Technical Details

The `zhijia_v2_split` codec uses the 0xA8 command which accepts two arguments:
- `arg0`: Controls the cold channel (0-250)
- `arg1`: Controls the warm channel (0-250)

When you control:
- **Cold light entity**: Sets arg0 to brightness value, arg1 to 0
- **Warm light entity**: Sets arg0 to 0, arg1 to brightness value

This allows independent control of each channel without interference.

## Migration from Color Temperature Mode

If you were previously using color temperature to control your lights:
- Old method: Adjusting CT slider affected both lights
- New method: Each light has its own brightness slider
- Both lights can now be controlled independently
- You can set different brightness levels for each light simultaneously
