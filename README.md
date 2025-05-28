# VRCLens Controller (VRCLC)

This is how I capture photos using a game console controller to operate VRCLens.
It is a non-destructive avatar addition to existing VRCLens setups via OSC.

Example video:
https://m0b1.b-cdn.net/vrclc/vrclc-demo-v1.0.mp4

The world used in the example is [Japanese Alley - VRC Light Volumes Test](https://vrchat.com/home/world/wrld_af756ca8-30ee-41a4-b304-2207ebf79db9/info).
The creator is [RED_SIM](https://sim.red/), of recent light volumes fame.

## Assumptions / Preconditions

- An avatar with default VRCLens v1.9.2 setup with write defaults and drone controls enabled
- Some game console controller like XBox or PlayStation
- Some avatar edit experience with [VRCFury](https://vrcfury.com/)
- 40 free bits of parameter space as the custom controller uses 5 floats

## Known issues

- General
  - VRChat window must be active/focused in order for screenshot button to work (no Discord alt-tab!)
  - Left and right stick click in needs mapping to perform bumper functions so bumpers can control exposure
- Desktop
  - Sometimes the camera restarts off-level, normally resetting again fixes this
  - Swapping the right stick from rotation to panning resets the current rotation value
  - No easy way to move the focus point
- VR
  - Your hand orientation when VRCLens starts sets the default rotation

## Controls reference

| Button           | Operation                                                 | Development note            |
|------------------|-----------------------------------------------------------|-----------------------------|
| Start            | Start VRCLens, reset if already on, or stop if held       | VRCLFeatureToggle 254       |
| Select           | Take screenshot                                           | Steam input                 |
| Left stick       | Move drone forward/backward or strafe                     | VRCLControllerMove{H/V}     |
| Right stick      | Pan or rotate drone view depending on right bumper toggle | VRCLControllerRotate{H/V}   |
| Left trigger     | Vertical descent                                          | VRCLControllerClimb [0, -1] |
| Right trigger    | Vertical ascent                                           | VRCLControllerClimb [0, +1] |
| D-pad up/down    | Aperture F-stop increase/decrease to adjust DoF           | VRCLFeatureToggle 193/192   |
| D-pad left/right | Zoom level decrease/increase                              | VRCLFeatureToggle 101/105   |
| X button         | Cycle through auto-exposure modes                         | VRCLFeatureToggle 12        |
| Y button         | Cycle through heads-up display sizes                      | VRCLFeatureToggle 64        |
| A button         | Cycle through avatar auto-focus modes                     | VRCLFeatureToggle 13        |
| B button         | Cycle through depth of field modes                        | VRCLFeatureToggle 11        |
| Left bumper      | Cycle between portrait and landscape                      | VRCLFeatureToggle 222       |
| Right bumper     | Toggle right stick between pan and rotation               | VRCLFeatureToggle 65        |

## How to install

### Remap game console controller input on Steam for VRChat

We want to remove all default Steam controller bindings so we can use them to operate VRCLens.

- Connect your game console controller to your computer by cable, Bluetooth, etc.
- Click on the controller configuration launcher on VRChat in your library
![](./screenshots/launch-controller-configuration.png)
- Click on the controller configuration exploration launcher
![](./screenshots/launch-controller-profile-search.png)
- Search for and apply my most recent VRCLC controller layout.
Note that the controller configuration often needs to be restarted to show new layouts.
![](./screenshots/search-for-vrclc-configuration.png)
- (Optional) If you want to create your own layout, just make sure Screenshot is set to Select
![](./screenshots/controller-layout-example.png)

### Set up OSC bridge program to send controller input to VRChat

We need a program to bind our game console controller inputs to VRChat OSC port.
For this I use [TouchOSC](https://hexler.net/touchosc) as it is [recommended by VRChat](https://docs.vrchat.com/docs/osc-overview#standalone-programs).
It is a free program with prompts to buy the one-time purchase lifetime license.

- Install [TouchOSC](https://hexler.net/touchosc#get)
- Open the connections option via menu option "Edit - Connections"
- Create and activate an OSC UDP connection to IP address 127.0.0.1, port 9000
![](./screenshots/touchosc-connection.png)
- Download the [VRCLC TouchOSC configuration file](./vrclc-touchosc.tosc)
- Open the downloaded file via menu option "File - Open..."
- If your game console controller is connected you should see inputs reflected in TouchOSC after pressing the play icon
- (Optional) If you want to create your own layout, I suggest starting via menu option "Help - Examples - Gamepad"

### Updating avatar controller and parameters to use new bindings

We need some controller layers on a VRCLens avatar to translate OSC input to VRCLens instructions.
This install step assumes the presence and use of the common [VRCFury](https://vrcfury.com/).
If you build your avatars without it, you will have to copy some layers and parameters by hand.

- Download the [VRCLC animation controller](./vrclc-animation-controller.controller)
- Download the [VRCLC parameters](./vrclc-parameters.asset)
- Drag both into your Unity avatar project
- Add a VRCFury "Full Controller" component to your avatar pointing to the new files.
In advanced components, make sure you create one global parameter with the "*" character!
The yellow box warnings can be ignored as they will link to VRCLens during play.
![](./screenshots/vrcfury-controller.png)
- Upload your avatar for testing in either desktop or VR

## Testing

- Launch TouchOSC, load the configuration file, and press the play icon
- Launch VRChat and ensure [OSC is active](https://docs.vrchat.com/docs/osc-overview#enabling-it)
- Load an avatar with both VRCLens and VRCLC controller installed
- Press the Start button on your game console controller and VRCLens should start
- Press the Select button on your game console controller and a picture should be captured
- See the [controls reference](#controls-reference) above for handling information

## How to Remove

- Remove VRCFury component for VRCLC from your avatar and re-upload
- Launch the Steam controller configuration exploration launcher
- Apply one of the recommended controller layouts
![](./screenshots/controller-recommended-layouts.png)
