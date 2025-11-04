# Split Cold/Warm Light Control

## Overview

This feature allows you to control two separate white lights independently using the cold and warm channels of your Zhi Jia controller. Instead of using color temperature control, each channel becomes a separate brightness-only light entity.

## Use Case

If you have a Zhi Jia controller (with brightness + color temperature features) connected to two regular white lights (not a CCT light), you can now control each light's brightness independently without color temperature adjustment.

## Configuration

To use this feature with your Zhi Jia v2 controller:

1. Set the codec to `zhijia_v2_split` during device configuration
2. Configure two light entities:
   - **Light 0**: Set type to `cold` - This controls the first white light (cold channel)
   - **Light 1**: Set type to `warm` - This controls the second white light (warm channel)

## Features

Each light entity supports:
- **On/Off control**: Turn each light on or off independently
- **Brightness control**: Adjust brightness from minimum (configured via `min_brightness` setting) to 100%
- **No color temperature**: Pure brightness control without CT adjustments

## Example Configuration

When setting up your integration:
```
Codec: zhijia_v2_split

Light Entities:
- Entity 0:
  - Type: cold
  - Min Brightness: 3% (or as low as your app allows)
  
- Entity 1:
  - Type: warm
  - Min Brightness: 3% (or as low as your app allows)
```

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
