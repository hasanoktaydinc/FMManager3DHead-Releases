# FM Manager 3D Head

A BepInEx 6 (IL2CPP) plugin for Football Manager 27 that replaces your manager's 3D face with a 3D head scan (.glb), on macOS and Windows.

## Install

You need BepInEx 6 (IL2CPP) installed for the game. Then:

1. Download the latest `FMManager3DHead-v….zip` from [Releases](https://github.com/hasanoktaydinc/FMManager3DHead-Releases/releases) and unzip it.
   - **Mac:** double-click **Install on Mac.command**. The first time macOS may refuse to open it because it was downloaded: right-click it and choose **Open**, or allow it in **System Settings → Privacy & Security → Open Anyway**.
   - **Windows:** double-click **Install on Windows.bat**.

   The installer finds the game's `BepInEx/plugins` folder in your Steam libraries (it asks when there are several, or none) and copies the plugin to `BepInEx/plugins/FMManager3DHead/`. Your faces and settings there are kept, so you can run it again for every update.

   **By hand:** copy `FMManager3DHead/FMManager3DHead.dll` and the `FMManager3DHead/lang` folder to `BepInEx/plugins/FMManager3DHead/`.
2. Get a 3D scan of your head as a **GLB** file (a `.zip` that contains one also works). See [Getting your head scan](#getting-your-head-scan-glb) below.
3. Start the game and go to the screen where you create your manager or change its appearance. Click **Edit 3D Head**, next to **Confirm** at the bottom right, to open the settings panel. Click **Load Manager Face** and pick the file.
   You can also drop the `.glb` into the `faces` folder, `BepInEx/plugins/FMManager3DHead/faces/`.

The head shows up on your manager's 3D character, for example in the dressing room and on the touchline, and (with **Also use for 2D portrait** on the panel's **Look** tab) on the 2D manager portrait.

When a newer version is published, the panel shows a **New version … - download** link at the bottom (nothing is downloaded by itself; turn the check off with `CheckForUpdates`).

## Getting your head scan (GLB)

You need a textured 3D model of your own head, saved as a single binary glTF file (`.glb`). The plugin does not create it; you make it once with a 3D head-scanning app or service and then load it in the game.

### 1. Create your head

**Recommended: [KeenFace](https://keenface.ai)** – this is the service the plugin was developed and tested with.

1. Go to [keenface.ai](https://keenface.ai) and create a free account. New accounts get **450 free credits**; one head costs **150 credits**, and a head that fails is not charged (see [pricing](https://keenface.ai/pricing); figures at the time of writing).
2. Open the **Studio** and upload your photos (front, left and right side of your head).
3. Wait a few minutes until your head is ready, then download it as **GLB**.

You can also use any other phone app or online service that creates a **realistic 3D head / digital human** from your photos or a video and can **export to GLB** (glTF 2.0). Look for one that:

- produces a **textured** model (a colour texture; a normal map for skin detail is a bonus)
- models the **eyes as separate parts** (most "realistic head" or "digital human" scanners do)
- includes your **neck and upper chest / shoulders**, not just the face

For the best result while capturing:

- use soft, even light (daylight from a window or an overcast sky); avoid strong light from one side or from above, which bakes shadows into the skin, especially on the neck
- take off your glasses, keep a neutral face with your eyes open, look straight ahead
- keep your hair off your forehead and ears; the game's own hairstyle is used on top of the scan
- wear a top with a low or open neckline so the scan captures your neck and upper chest

### 2. Export as GLB

Export or download the model as **GLB**. If you get a `.zip`, you can load it as it is, as long as it contains a `.glb`.

### 3. Check the model (only if it does not load)

The plugin recognises the parts of the model by their **material names**:

| Part | Material name | Needed |
|---|---|---|
| Head and neck skin | contains `Skin` (for example `Head_Skin`) | yes |
| Left / right eye | starts with `Eye` (for example `Eye_L`, `Eye_R`) | yes (used to place and size the head) |
| Teeth, corneas | any name; transparent parts are skipped | no |

The head should face forward (+Z) with +Y up, which is the glTF standard. A slightly turned or tilted head is corrected automatically from the eyes.

If your app only exports another format (FBX, OBJ, USDZ), or its material names differ, convert it with the free [Blender](https://www.blender.org/):

1. **File > Import** your model.
2. In the **Material** tab, rename the skin material so it contains `Skin`, and the two eye materials to `Eye_L` and `Eye_R`.
3. **File > Export > glTF 2.0**, choose the **glTF Binary (.glb)** format, keep **+Y Up** on, and export.

The log file `BepInEx/plugins/FMManager3DHead/FMManager3DHead.log` tells you what was found (for example "Scan includes the body down to 25.2 cm below the eyes"), or why a model could not be used.

### 4. Load it

In the game, click **Edit 3D Head** on the manager appearance screen (or press **Ctrl+Shift+H**), click **Load Manager Face** and pick your `.glb` (or `.zip`). Then fine-tune the fit in the panel; see Settings and Troubleshooting below.

The plugin folder then looks like this:

```
BepInEx/plugins/FMManager3DHead/
├── FMManager3DHead.dll
├── FMManager3DHead.log     the plugin's log
├── faces/                  manager faces (.glb), profiles.json, saved 2D portraits (portraits/)
└── lang/                   panel languages (.json)
```

## In-game panel

Click **Edit 3D Head** to open the **FMManager3DHead** panel. The button is on the manager appearance screen (where you create your manager or change its looks), next to **Confirm**, where you also see your 3D manager. There is also a keyboard shortcut that opens and closes the panel anywhere in the game: **Ctrl+Shift+H** (on a Mac, **Cmd+Shift+H** also works). You can change it with `Hotkey` in the settings file.

At the top the panel shows which manager profile you are adjusting. **Load Manager Face** loads a face for it; **Remove face** (click twice) deletes this manager's face and settings, so it shows the game's own face again in all its careers. The settings are on six tabs:

| Tab | Settings |
|---|---|
| **Head** | Head size and width, head height and forward/back, back of the head in/out, how deep the head sits inside the game head; **Hairline**: hairline height, hair blend, scalp under game hair |
| **Face** | Details scans often soften, on four sub-tabs: **Forehead & nose** (forehead, brow ridge, nose tip and width), **Cheeks & ears** (cheekbones, cheek fullness, ears), **Mouth** (mouth, lip fullness), **Chin & jaw** (chin forward/back, chin tip, chin length, jaw width, chin tip width, double chin) |
| **Eyes** | Eye colour, eye brightness, iris size, eye whiteness, eye gloss, eye depth |
| **Neck** | Neck length (front and back), where the neck smoothing starts, neck tone, body depth; use the scan's body (shoulders), smooth neck skin |
| **Look** | Skin brightness and warmth, face sharpness, eyebrow darkness; show the face, hide the game's hair, beard and eyebrows, skin detail, use for the 2D portrait (and **Redraw 2D portraits**) |
| **Accessories** | The game's glasses (tilt, height, depth, width), earrings and hearing devices (height, depth, in/out), each separately |

Every number has **-** and **+** for small steps, and a bar between them: click or drag on the bar to set the value. The bar fills from the default value to the current one, the name of every changed setting turns purple, and the small arrow in front of it puts just that setting back to its default. While the manager on screen has no face yet, the panel shows a short how-to and a big **Load Manager Face** button instead of the settings.

Below the tabs: **Export settings**, **Import settings**, **undo** / **redo** (the two arrows; also **Ctrl+Z** / **Ctrl+Shift+Z**, on a Mac **Cmd+Z** / **Cmd+Shift+Z**, while the panel is open), **Reset to defaults** and **Close**. Undo goes back step by step, separately for each manager profile; quick changes in a row (several clicks, dragging a bar) are one step.

### Export and import settings

**Export settings** saves all panel settings of the manager profile in use (head, face, eyes, neck, look and accessories) to a `.json` file; it suggests your Downloads folder and a name like `Hasan Dinç - 3D head settings.json`. **Import settings** loads such a file into the manager profile in use, and the head updates a moment later. Use it for a backup before you experiment, to give another manager profile the same settings, or to share them with a friend. The face itself (`.glb`) is not part of the file.

A file from an older version of the plugin works too: settings it does not contain keep their current values.

Drag the panel by its title bar to move it. Changes are applied 1.5 seconds after your last click (so you can click a few steps quickly), and are saved for the manager profile. Most changes show within a second; the neck length, neck smoothing, neck tone, eye colour, eye brightness, face sharpness and eyebrow darkness settings reprocess the face file and take a few seconds.

## One face per manager profile

FM keeps your managers as profiles that you can use in any number of careers. Each profile gets its own face and its own panel settings, so a second profile (for example a female manager) can have a different face and different neck and fit settings.

1. Open the manager appearance screen of the profile (when you create the manager, or when you change its looks).
2. Click **Edit 3D Head** and then **Load Manager Face**. The face is saved for this profile.
3. Adjust the settings in the panel. They are saved for this profile too.

Repeat this for every profile that should have a face. From then on the plugin recognises the profile's manager in every career, in the dressing room, on the touchline and in its 2D portraits, and uses that profile's face and settings. A manager without a face keeps the game's own face. Loading a new face (from the appearance screen, or with **Ctrl+Shift+H** while your manager is on screen) replaces that profile's face in all its careers.

**What belongs to the profile and what to the career.** The face and the panel settings belong to the profile, so they are the same in all of the profile's careers. What you change on the game's own appearance screen inside a career (hair, beard, outfit, glasses, jewellery) stays in that career, as the game keeps it: you can give your manager a different hairstyle in each career and the face still follows. When you load a new face for a profile, every career of that profile shows it, and the hair and outfit you picked in each career stay as they are.

The plugin recognises a profile by its looks in the appearance editor: gender, skin tone and the face sliders. Changing the face sliders on the appearance screen is fine (the plugin follows the changes there). Two profiles with exactly the same face sliders cannot be told apart.

**Coming from an older version:** your existing face is not lost. Open your manager's appearance screen (or your career), open the panel and click **Use the existing face for this manager**.

The panel shows which profile you are adjusting by its manager's name. The plugin reads the name on the manager appearance screen, so a profile shows as "Profile 1" until you have opened its appearance screen once.

The faces are in the `faces` folder (`face-<id>.glb`), and the list of profiles with their settings is in `faces/profiles.json`.

## Languages

The panel and the Edit 3D Head button come in English, Türkçe, Deutsch, Português, Français, Español, Italiano and Polski. The plugin uses your game's language; if the game is set to any other language, it uses English. To choose a language yourself, set `Language` in the settings file (for example `Language = de`).

Each language is a file in the `lang` folder inside the plugin folder (`BepInEx/plugins/FMManager3DHead/lang/`): `en.json`, `tr.json`, `de.json` and so on. The plugin creates them the first time it runs.

**Adding your own language**

1. Copy `en.json` and name the copy after your language, for example `ru.json`.
2. Open it in a text editor and save it as UTF-8.
3. Set `"language"` to the language's name, for example `"Русский"`.
4. Set `"match"` to words that appear in the game's language name, for example `["рус", "russ", "ru"]`. Short words (3 letters or less) must be the first word of the name; longer ones can appear anywhere in it. The log shows the game's language name: look for "game language '…'".
5. In `"texts"`, translate only the text on the **right** of each line. Leave the English text on the left unchanged.

The plugin reloads the files every few seconds, so you can see your changes in the game while you work. A missing or empty line shows the English text. You can also edit the files that come with the plugin; an update does not overwrite them.

```json
{
  "language": "Русский",
  "match": ["рус", "russ", "ru"],
  "texts": {
    "Head size": "Размер головы",
    "Load Manager Face": "Загрузить лицо менеджера"
  }
}
```

## Settings

Most settings can be changed in the in-game panel (**Edit 3D Head** or **Ctrl+Shift+H**). The settings of the `Look` and `Fit` sections, and `ApplyToPortrait`, belong to the manager profile in use: the file always shows the settings of the profile that was on screen last, and editing them changes that profile. All settings are also in `BepInEx/config/FMManager3DHead.cfg`. Changes are applied as soon as you save the file; no restart needed.

| Setting | Default | What it does |
|---|---|---|
| `UseScanBody` / `FrontChestCm` | true / 24 | When the scan reaches below the neck, open collars (polo, V-neck) are filled with the scan's own upper chest down to `FrontChestCm` below the eyes (panel: "Body depth"), so the skin colour always matches. If a deep V-neck shows a dark gap at the bottom, raise it. Shoulders and the back are cut at the collar lines. |
| `KeepGameNeck` / `GameNeckFromCm` | false / 11 | (Experimental.) Keeps the game's own neck and chest under the scan, from this far below the eyes, so open collars (polo shirts, t-shirts) are filled. It takes the colour of your scan's skin (see `MatchNeckSkin`). |
| `ConformNeck` | true | Wraps the scanned neck (below the jaw) onto the game's neck, so it fits inside collars and has no step where it ends. |
| `NeckLengthCm` / `NeckLengthBackCm` | 18 / 16 | How far below the eyes the scanned neck is cut at the front and at the back, in cm. The cut follows the shape of a shirt collar. Collars sit at different heights per outfit: if skin peeks out over the back of a jacket collar while moving, raise `NeckLengthBackCm` (panel: "Neck length back"); if a crew-neck T-shirt shows a gap at the back, lower it (13–16 covers most outfits). |
| `NeckWidthCm` / `NeckSideTrimStartCm` | 14 / 12 | Below the jaw, the neck is trimmed to this width so it doesn't poke through the collar at the sides. Lower the width if skin still shows on the shoulders. |
| `NeckBackInsetCm` | 1 | Cuts the back of the neck below the jaw so it doesn't show through the jacket. Raise it if skin shows at the back; lower it if a gap appears at the nape. |
| `FitMode` | Shape | Settings file only. `Shape` matches the game's skull width, height and depth, so hairstyles, hats and glasses sit on the head. `HeadWidth` matches only the width. `Eyes` keeps the face true to life, but it can look big under game hair. `Average` is halfway between. |
| `StraightenScan` | true | Uses the eye line to undo a slightly turned or tilted scan, so it sits straight inside the game's hair. |
| `MatchCrownHeight` | false | With `Shape`, also stretches the head to the game's crown height. It is off by default because it can make the face look long. |
| `MaxShapeStretch` | 0.15 | With `Shape`, the most that height and depth may differ from the width (0.15 = 15%). |
| `SkinNeck` | true | The lower neck follows the game's neck bones, so it stays inside the collar when the head turns. |
| `NeckBlendStartCm` / `NeckBlendEndCm` | 9 / 14 | The neck moves only with the head down to the first depth below the eyes. Below the second it moves with the neck bones. In between it blends. |
| `ConformSkull` | true | Reshapes the back and sides of the scanned skull to the game's head, so the game's hairstyles cover it. The face is not changed. |
| `ConformInsetCm` | 0.3 | Keeps the scan this far inside the game's skull. Raise it if skin still shows through the hair. |
| `ConformSidesFromCm` | 2.5 | Starts reshaping the sides at this height above the eyes. Lower it if skin shows above the ear. |
| `ConformMaxChange` / `ConformFromCmBelowEyes` | 0.25 / 8 | The most the skull may be reshaped, and how far below the eyes the reshaping starts. |
| `ScaleMultiplier` / `ForwardOffset` / `UpOffset` | 1 / 0 / 0 | Fine-tune the size and position of the head. |
| `HeadWidthMultiplier` | 1.0 | Narrows or widens the head sideways only (panel: **Head width**), for example when the arms of the game's glasses sink into the temples. Height and depth stay as they are, so the game's hair still fits. |
| `HeadUpCm` | 0 | Moves the head up (+) or down (-) in cm (panel: **Head height**), for example to move the hairline. |
| `HeadForwardCm` | 0 | Moves the head forward (+) or back (-) in cm. |
| `ForeheadCm` | 0 | Pushes the forehead out (+) or in (-) in cm, from above the eyebrows to the hairline. |
| `BackOfHeadCm` | 0 | Pushes the back of the head out (+, rounder or more pointed) or in (-, flatter) in cm. With the game's hair on, large values poke through the hair. |
| `ChinCm` | 0 | Moves the chin forward (+) or back (-) in cm; the mouth and the sides of the jaw stay almost put. |
| `ChinTipCm` | 0 | Moves only the tip of the chin forward (+) or back (-), in cm. |
| `MouthCm` | 0 | Moves the lips and the area around the mouth forward (+) or back (-), in cm; the nose and the chin stay put. |
| `NoseTipCm` | 0 | Moves the tip of the nose forward (+) or back (-), in cm. Scans often round the nose off. |
| `NoseWidth` | 1.0 | Narrows (< 1) or widens (> 1) the nose. |
| `CheekbonesCm` | 0 | Brings the cheekbones out (+) or flattens them (-), in cm. |
| `BrowRidgeCm` | 0 | Brings the brow ridge above the eyes out (+) or flattens it (-), in cm. |
| `LipsCm` | 0 | Fuller (+) or thinner (-) lips, in cm. |
| `CheeksCm` | 0 | Fuller (+) or hollower (-) cheeks, in cm. |
| `DoubleChinCm` | 0 | Under the chin: fuller (+) or tighter (-), in cm. Stops above the collar line. |
| `ChinLengthCm` | 0 | Makes the chin longer (+, the bottom of the chin moves down) or shorter (-), in cm. The lips stay put; the neck and collar are not touched. |
| `ChinTipWidth` | 1.0 | Narrows (< 1, more pointed) or widens (> 1, squarer) only the tip of the chin; the jaw corners stay put. `JawWidth` changes the whole lower face. |
| `JawWidth` | 1.0 | Narrows (< 1) or widens (> 1) the lower face only, from the cheeks to the jaw corners. |
| `EarsCm` | 0 | Moves the scan's ears away from (+) or towards (-) the head, in cm, for example when they stick out through the game's hair. |
| `SkinBrightness` | 1.0 | Brightness of the scan's skin (face, neck and body), to match the game's lighting and hands. |
| `SkinWarmth` | 0 | Shifts the scan's skin colour warmer (+, redder) or cooler (-), to match the game's hands and lighting. |
| `FaceSharpness` | 0 | Sharpens the scan's skin texture (0 = off, 1 = strong): brings back pores, stubble, eyebrow hairs and lip lines in soft scans. |
| `EyebrowDarkness` | 0 | Darkens the scan's own eyebrow hairs (0 = off, 1 = strong); the skin between the hairs keeps its colour. |
| `GlassesWidth` | 1.0 | Widens (> 1) or narrows (< 1) the game's glasses, for example when their arms sink into the temples. |
| `HideGameHair` | false | Scanned heads usually have no hair, so the game's hairstyle is kept by default. |
| `HideFacialHair` / `HideEyebrows` | true | Hide the game's beard and eyebrows. |
| `EyeColour` | Original | Eye colour: Original, Brown, DarkBrown, Black, Hazel, Green, Blue or Grey. The scan's iris pattern is kept. |
| `IrisBrightness` | 1 | Darkens (below 1) or lightens (above 1) the scan's eye colour. For example, 0.6 gives darker brown eyes. |
| `IrisRadius` | 0.145 | Panel: **Iris size**. Size of the iris on the eye texture, used by the eye colour and brightness settings. Raise it if a ring of the scan's old colour stays around the iris; lower it if the colour spills onto the white of the eye. |
| `EyeWhiteness` | 0 | Cleans up the whites of the eyes (0 = off, 1 = strong): less grey or yellow, a little brighter. Dark corners and eyelid shadows are kept. |
| `EyeGloss` | 0 | Makes the eyes wetter and glossier, with more reflection (0 = as the game draws them, 1 = strong). |
| `EyeDepthMm` | 0 | Moves the eyeballs forward (+) or back (-) in their sockets, in mm, for example when they look sunken or bulge through the eyelids. Use small steps: the eyelids are part of the skin and stay put. |
| `UseNormalMap` | true | Uses the scan's skin detail map (pores, wrinkles). Turn it off if the face looks patchy. |
| `UseGameNeckWeights` | true | Bends the scanned neck with the game's own neck skinning, so it stays inside collars when the head turns or tilts. |
| `MatchNeckSkin` | true | With `KeepGameNeck` on, colours the kept game neck with your scan's skin so there is no colour seam at the collar. |
| `GameNeckShoulderInsetCm` | 0.6 | With `KeepGameNeck` on, sinks the kept game shoulders under the skin so they don't poke through high collars. Raise it if skin still shows at the shoulders. |
| `EarringUpCm` / `EarringForwardCm` / `EarringOutCm` | -0.8 / -0.1 / -0.3 | Moves the game's earrings onto your ears: up/down, forward/back, and away from/towards the head (both sides mirrored). |
| `HearingDeviceUpCm` / `HearingDeviceForwardCm` / `HearingDeviceOutCm` | -0.8 / -0.1 / -0.3 | The same for the game's hearing devices, separately from the earrings. |
| `SmoothNeckSkin` / `NeckSmoothFromCm` / `NeckSmoothToCm` | true / 11 / 14 | Fades the bottom of the scanned neck (usually blurry and blotchy in scans) into one even skin colour, from/to this far below the eyes. Beard hairs are left untouched, so the fade can start high without erasing the beard. |
| `NeckTone` | Brightness of the evened neck skin (panel: "Neck tone"). 1 = as sampled; lower = darker. |
| `GameNeckFrontOnly` | true | With `KeepGameNeck` on, leaves out the back of the game's neck, which pokes out above collars (open collars are at the front). |
| `GlassesTiltDeg` | 0 | Tilts the game's glasses around the bridge of the nose so the arms rest on your ears. Negative values lower the arms. |
| `GlassesUpCm` | 0 | Moves the glasses up or down (cm). |
| `GlassesForwardCm` | 0 | Moves the glasses forward or back (cm). |
| `HairlineCm` | 8 | Height of the hairline above the eyes, in cm (panel: "Hairline height"). `HairBlend` and `ScalpInsetCm` work above it. |
| `HairBlend` | 0 | Blends the scan's scalp above the hairline into the colour of the scan's own hair, a little darker like hair roots (0 = off, 1 = full). Where the scalp shows under or between the game's hair it then reads as hair, not as a bald patch or the scan's blotchy hair. Leave it at 0 if you hide the game's hair. |
| `ScalpInsetCm` | 0 | Sinks the scalp above the hairline into the head, in cm (panel: "Scalp under game hair"), so the scan's own hair stays under the game's hair instead of poking through it. Leave it at 0 if you hide the game's hair. |
| `ApplyToPortrait` | false | Also puts the head on the game's 2D manager portrait (panel: "Also use for 2D portrait"). The game keeps its portrait until it is rendered again: after switching this on, load the face again or restart the game. No visible effect if a photo pack replaces your portrait. |
| `Language` | Auto | Language of the panel and the Edit 3D Head button. `Auto` uses the game's language (English if there is no file for it). Or the name of a file in the `lang` folder: `en`, `tr`, `de`, `pt`, `fr`, `es`, `it`, `pl`, or one you added. |
| `EditorButton` | true | Shows an **Edit 3D Head** button on the manager appearance screen (creating your manager or changing its looks), next to **Confirm**. It opens the settings panel. |
| `FaceCache` | true | Saves each face's finished textures in `faces/cache`, so the face loads in a moment the next time instead of being processed again (seconds). Made again by itself when the face file or a setting that changes the textures changes. The folder can be deleted at any time. |
| `CheckForUpdates` | true | Looks once per game start whether a newer version is published on [GitHub](https://github.com/hasanoktaydinc/FMManager3DHead-Releases/releases); the panel then shows a link to it. Nothing is downloaded or installed by itself. |
| `SavePortraits` | true | Saves the 2D portraits drawn with your face (in `faces/portraits`) and shows the saved picture the next time, so screens with your portrait open without waiting. A new portrait is drawn by itself when the manager's looks in that career (hair, outfit…), the profile's settings or its face change. The panel's **Redraw 2D portraits** button deletes the saved ones. |
| `Hotkey` | Ctrl+Shift+H | Keyboard shortcut that opens and closes the settings panel anywhere in the game. Modifiers `Ctrl` (on a Mac, Command also counts as Ctrl), `Shift`, `Alt`, `Cmd`, then one key (`A`–`Z`, `F1`–`F12`, `0`–`9`…), for example `Ctrl+Shift+H` or `F8`. Pick a combination you have not given to anything in the game. Empty or `None` turns it off. |
| `ManagerIds` | (empty) | Only used until the first face profile exists (see [One face per manager profile](#one-face-per-manager-profile)): the manager IDs whose 2D portrait gets the head. With profiles, portraits are recognised by the profile. |

## Troubleshooting

The plugin writes its own log to `FMManager3DHead.log` in its folder, `BepInEx/plugins/FMManager3DHead/`. Check it first if the head does not appear.

**The head looks stretched front to back, or the back of the head looks wrong.** Your scan may not include the whole head (some apps only scan the face). Set `FitMode = HeadWidth` in the settings file. Leave it on `Shape` (the default) for full head scans: the other modes size the head evenly and can leave the chin, mouth or earrings off the game's head, for example a dark patch at the mouth in 2D portraits.


## Support and contact

If the plugin makes your career more fun, you can [buy me a coffee](https://buymeacoffee.com/codexx01). News, questions and ideas: [@codexx01 on X](https://x.com/codexx01). Both links are also in the panel (top right and bottom right).

Panel icons: [Lucide](https://lucide.dev) (ISC license).
