# OSCLeash

A "simple" system to make a functional "Leash" in VRchat using OSC. <br/>I swear I'm not a disappointment to my parents. This is **kinda** user friendly now. <br/>This can be adapted to any pullable physbone; EG: A tail. 

<br/>

## Options for running OSCLeash

1. **Via an executable**
   - Download latest zip [from releases](https://github.com/ZenithVal/OSCLeash/releases)
   - Extract wherever.
   - Run Executable
2. **From the source**
   - Clone the github
   - Run `pip install -r requirements.txt` in the directory to install libraries
   - Run the python script

## Setup

**Requires VRC3 Avatar SDK.**

1. Download the program via one of the above methods.
2. Define config.json settings if needed.
3. Grab the prefab [from releases](https://github.com/ZenithVal/OSCLeash/releases)
4. Place the prefab on the ROOT of the model. (Next to Meshes/Armature)
5. Set the source of `Leash Physbone` to your leash, and adjust it if needed. 
   - Only one physbone can control the bones, delete your pre-existing physbone if needed.
6. The position constrain source on `Aim Target` should be assigned to the last bone of your leash.
7. If your phybone is off center, copy the constraint from above and paste it on the root of the prefab. 
   - The source should be the origin of your leash.
8. Enable OSC in the radial menu (Options->OSC->Enable)
   - Also click Reset OSC in the radial menu if this is an update to a pre-existing model.
9. Run program.

<br/>

There will be a setup video later.

For setup questions/support feel free to shoot me a DM or ask in #OSC-Talkin in [my Discord](https://discord.gg/7VAm3twDyy)

<br/>

## Known issue & Xbox input Workaround.

As with RavenBuilds's take on the OSCLeash, using OSC as an input for movement causes your arms to be locked into desktop pose, please slap some support onto [this canny](https://feedback.vrchat.com/feature-requests/p/osc-locks-arms)! 

**TEMPORARY WORKAROUND**: Set "XboxJoystickMovement" to true in the config file. Instead of outputting movement with OSC, this will emulate an Xbox controller joystick! Skipping over the above issue entirely. This will probably be removed when VRC fixes the issue.<u> Check the extra steps in setup for this. <br/></u>

<br/>

**For Xbox input**

- You need [ViGEm](https://github.com/ViGEm/ViGEmBus/releases) installed.
- The VRC window must be focused for inputs to be recieved.

<br/>

# Config

| Value                 | Info                                                                      | Default   |
|:--------------------- | ------------------------------------------------------------------------- |:---------:|
| IP                    | Address to send OSC data to                                               | 127.0.0.1 |
| ListeningPort         | Port to listen for OSC data on                                            | 9001      |
| Sending port          | Port to send OSC data to                                                  | 9000      |
|                       |                                                                           |           |
| Logging               | Logging values in the terminal                                            | true      |
| GUIEnabled            | Troggles the GUI visibility                                               | True      |
| GUITheme              | Theme for the small UI box, try "random"                                  | blank     |
| StartWithSteamVR      | Starts the app when SteamVR starts                                        | false     |
|                       |                                                                           |           |
| ActiveDelay           | Delay between OSC messages while the leash is being grabbed. (In seconds) | 0.05      |
| InactiveDelay         | Delay between non-essential OSC messages (In seconds)                     | 0.5       |
|                       |                                                                           |           |
| RunDeadzone           | Stretch value above this will cause running                               | 0.70      |
| WalkDeadzone          | Stretch value above this will cause walking                               | 0.15      |
| StrengthMultiplier    | Multiplies speed values but they can't go above (1.0)                     | 1.2       |
|                       |                                                                           |           |
| TurningEnabled        | Enable turning functionality                                              | false     |
| TurningMultiplier     | Adjust turning speed                                                      | 0.75      |
| TurningDeadzone       | Stretch value above this will begin turning                               | 0.15      |
| TurningGoal           | Goal degree range for turning. (0° to 144°)                               | 90        |
| TurningKp             |                                                                           | 0.5       |
|                       |                                                                           |           |
| XboxJoystickMovement  | Esoteric workaround for VRC breaking animations upon OSC input            | false     |
| BringGameToFront      | Focuses the selected window so Xinput works correctly                     | false     |
| GameTitle             | The name of the window                                                    | VRChat    |
|                       |                                                                           |           |
| PhysboneParameters    | A list of Physbones to use as leashes                                     | see below |
| DirectionalParameters | A dictionary of contacts to use for direction calculation                 | see below |
| DisableParameter      | Parameter that will pause the program                                     | blank     |
|                       |                                                                           |           |

ᴹᵃⁿ ᵗʰᵉʳᵉ'ˢ ᵃ ˡᵒᵗ ᵒᶠ ˢᵉᵗᵗᶦⁿᵍˢ

---------

### Physbone Parameters

The list of the parameters your leashes are using. <br/>If you had three leashes, your list might look like this:

```json
        "PhysboneParameters":
        [
            "Leash",
            "Leash2",
            "Leash3"
        ],
```

The script will automatically read from the _IsGrabbed and _Stretch parameters correlating with the above.

<br/>

---

### Multiple Leashes

I wouldn't reccomend attempting this at the moment unless you understand constraints and animations. <br/>

- Depending on which is grabbed, you'll need to animate two position constraints. 
  - We only care about which one was grabbed first. 
- The Source of `Aim Target` should alernate the ends of the leashes
- The source of a psotion constraint on the OSCLeash Prefab should move between the origins of the leashes.

This will be included in a setup video eventually. <br/>

---

### Turning Functionality

WOAAAH! **Motion sickness warning!** This gets a bit funky but you really don't need to worry about the math.<br/>
If you want to enable the feature make sure to set **TurningEnabled to True**.<br/>
**This does not work well with the controller workaround**<br/>
`Currently Supports North, East, South, & West`<br/>

If you had a leash up front and you want to turn to match the direction it's pulled from (EG: a Collar with the leash on the front) Set set the parameter on your Leash Physbone and config to `Leash_North`.<br/>

```json
        "PhysboneParameters":
        [
            "Leash_North",
            "Leash_South",
            "Leash_East",
            "Leash_West"
        ],
```

We'll parse the name of your leath for North, South, ect based on underscores. So `"Tail_South"` would work.

Whenever this leash is grabbed and pulled past the deadzone it will begin to turn. <br/>It will continue to turn until it is greater than the TurningDeadzone value. <br/>

<br/>

Here's the simplified logic of the system.

```python
if LeashDirection == "North" and Z_Positive < TurningGoal:
    TurningSpeed = ((X_Negative - X_Positive) * LeashStretch * TurningMultiplier)
```

<br/>

---

### Directional Parameters

If you wish to change the contacts to used for direction calculations, you can do so here. 

```json
        "DirectionalParameters":
        {
                "Z_Positive_Param": "Leash_Z+",
                "Z_Negative_Param": "Leash_Z-",
                "X_Positive_Param": "Leash_X+",
                "X_Negative_Param": "Leash_X-"
        }
```

<br/>

---

### Default Config.json

```json
{
        "IP": "127.0.0.1",
        "ListeningPort": 9001,
        "SendingPort": 9000,

        "Logging": true,
        "GUIEnabled": true,
        "GUITheme": "",
        "StartWithSteamVR": false,

        "ActiveDelay": 0.05,
        "InactiveDelay": 0.5,

        "RunDeadzone": 0.70,
        "WalkDeadzone": 0.15,
        "StrengthMultiplier": 1.2,

        "TurningEnabled": false,
        "TurningMultiplier": 0.75,
        "TurningDeadzone": 0.15,
        "TurningGoal": 90,
        "TurningKp": 0.5,

        "XboxJoystickMovement": true,
        "BringGameToFront": false,
        "GameTitle": "VRChat",

        "PhysboneParameters":
        [
                "Leash"
        ],

        "DirectionalParameters":
        {
                "Z_Positive_Param": "Leash_Z+",
                "Z_Negative_Param": "Leash_Z-",
                "X_Positive_Param": "Leash_X+",
                "X_Negative_Param": "Leash_X-"
        },

        "DisableParameter": "",

        "ScaleSlowdownEnabled": true,
        "ScaleParameter": "Go/ScaleFloat",
        "ScaleDefault": 0.25
}

```

---

## How does this work??

**Step 1: Gather Leash Physbone Values**

This one is simple. We receive the Leash_Stretch and Leash_Grabbed parameters.  
If Leash_Grabbed becomes true, we begin reading Leash_Stretch 

We'll use these values in this example:  

> Leash_IsGrabbed = True<br/>Leash_Stretch = $0.95$

<br/>

**Step 2: Gather Directional Contact values**

<img src="https://cdn.discordapp.com/attachments/606734710328000532/1011420984303165500/Example_Gif.gif" title="" alt="Function Example" width="502"> 
4 Contacts (Blue) surround a object with an aim constraint and a contact at the tip. (Yellow) <br/>

Based on where the constraint is aimed, it will give us 4 values. <br/>

If it was pointing South-South-West, we would get:

> Leash_Z+ = $0.0$<br/>Leash_Z- = $0.75$<br/>Leash_X+ = $0.0$<br/>Leash_X- = $0.25$ 

<br/>

**Step 3: MAAAATH!**

Math is fun. Add the negative and positive contacts & multiply by the stretch value.

> (Z_Positive - Z_Negative) * Leash_Stretch = Vertical<br/>(X_Positive - X_Negative) * Leash_Stretch = Horizontal 

So our calculation for speed output would look like:

> $(0.0 - 0.75) * 0.95 = -0.7125$ = Veritcal<br/>$(0.0 - 0.25) * 0.95 = -0.2375$ = Horizontal

<br/>

**Step 4: Outputs**

If either value is above the walking deadzone (default 0.15) we start outputting them instead of 0. <br/>If either value is above the running deadzone (0.7) we tell the player to run (x2 speed)

All movement values are relative to the VRC world's movement speed limits. <br/>So we'd be moving at $142.5$% speed south and $47.5$% speed to the West. 

If the values are below the deadzones or _IsGrabbed is false, send 0s for the OSC values once to stop movement. 

<br/>

# Credits

- @Nullstalgia & @Spectrshiv are responsible for the rewrite of v3
- @ALeonic is responsible for a majority of v2
- @FrostbyteVR babied me through 90% of the process of making v1
- @I5UCC I stared at the code of their ThumbParamsOSC tool for a long time.
- Someone else did this but it was a closed source executable.
