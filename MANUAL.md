# The Fire, BEHAVE! manual

_For Unreal Engine 5.8, on Windows and Mac. C++ plugin, usable from Blueprints or C++. Copyright 2026 AIGameDev.olution._

Fire, BEHAVE! is wildfire that behaves the way real fire does, in weather that is worked out just as carefully. Light a fire and it runs with the wind and up hills, backs slowly into the wind, crosses a road by its embers, climbs into the tree tops on a hot day and dies in the rain. The weather gives each part of the fire its own air (wind at the height of the flames, temperature, humidity, sun, rain and snow), and the fire gives back smoke, its own wind and, when it is big enough, a thunderstorm of its own.

The fire's spread is the Rothermel model the US Forest Service uses, with the forty fuel models of today's fuel maps, and the weather's numbers are checked against published tables. All of that maths is plain C++ with no engine in it, and it ships with its tests.

**Contents**

1. [Install](#1-install)
2. [The looks, sounds, plants and demo level](#2-the-looks-sounds-plants-and-demo-level)
3. [Your first fire, in five minutes](#3-your-first-fire-in-five-minutes)
4. [The fire looks](#4-the-fire-looks)
5. [Grass, bushes, fallen wood and trees](#5-grass-bushes-fallen-wood-and-trees)
6. [The fire's sound](#6-the-fires-sound)
7. [The weather](#7-the-weather)
8. [Temperatures: Celsius, Fahrenheit or Kelvin](#8-temperatures-celsius-fahrenheit-or-kelvin)
9. [Using it from your game](#9-using-it-from-your-game)
10. [Settings worth knowing](#10-settings-worth-knowing)
11. [Packaging your game](#11-packaging-your-game)
12. [Limits](#12-limits)
13. [Credits and licence](#13-credits-and-licence)

## 1. Install

1. Close the Unreal editor.
2. Put the `WeatherDirector` folder into your project's `Plugins` folder (make the folder if there isn't one), so that you have `Plugins\WeatherDirector\WeatherDirector.uplugin`. If you installed it from Fab into the engine, skip this step and turn it on under **Edit > Plugins** instead.
3. Open the project. If Unreal asks to rebuild modules, click **Yes**. This needs Visual Studio with the "Game development with C++" tools on Windows, or Xcode on a Mac. A project with no C++ of its own yet needs one class first: **Tools > New C++ Class**, pick **None**, **Create Class**.

If the build fails, open `Saved\Logs\<YourProject>.log` and search for `error C` (on a Mac, `error:`). Each line found has the file and line number. Send them to support.

## 2. The looks, sounds, plants and demo level

The plugin comes with everything it draws and plays already made, in its own Content folder. To see it in the Content Browser, tick **Show Plugin Content** in the Content Browser's settings; its folder there carries the plugin's name, **Fire, BEHAVE!**. It holds:

- **FireLooks**: the two materials that draw the eight fire looks, `M_FirePuff` and `M_SmokePuff`, with the noise texture `T_FireNoise` and the card `SM_FireCard` they draw on.
- **FireSounds**: the fire's two loops, `S_FireBacking` and `S_FireHead`.
- **FuelArt**: the plants, `SM_FuelGrass`, three bushes, three logs and a branch, three each of conifers, broadleaf trees and dead snags, and their two materials, `M_FuelGrass` and `M_FuelPlant`.
- **Demo**: the demo level, `FireDemo`, and its four plain materials.

Every Fire Field picks them up by itself.

**The demo level.** Open **Demo > FireDemo**: a late-afternoon sky over dry grass, a Weather Director and a Fire Field with their try-it keys on, a road 40 metres ahead that a fire on a mild day won't cross, a stand of young conifers that torch, a patch of head-high brush and one of clearcut slash, and a ridge. The field draws the grass, the brush, the slash and the trees itself (section 5). Press Play, click in the game view and press **F**.

**Making them again.** The Python scripts that made them are in the plugin's `Content/Python` folder, with the texture, card, models and recordings they are made from. You need them only to make again something you deleted or changed. The plugin's folder is `Plugins/WeatherDirector` in your project, or the plugin's folder under the engine's `Engine/Plugins/Marketplace` if you installed it from Fab.

1. **Edit > Plugins**, search for **Python Editor Script Plugin**, tick it and restart the editor.
2. **Tools > Execute Python Script**, and pick `Content/Python/FireLooks/make_fire_looks.py` in the plugin's folder.
3. Wait a few minutes. The Output Log ends with **FIRE_LOOKS done, no problems**.
4. Restart the editor once, so every Fire Field picks them up.

Running it keeps the card, the meshes and the four materials that are already there, and imports the texture and sounds again. If you update the plugin and a material it keeps was made by an older copy of the script, the Output Log says so in a **PROBLEM** line. To make everything afresh, close the editor, delete the plugin's `Content/FireLooks` and `Content/FuelArt` folders, open the editor and run it. Without them, a Fire Field draws its fire as plain orange cones (and says so once in the Output Log), plays no sound and grows nothing.

`Content/Python/Demo/make_demo_map.py`, run the same way, makes the demo level again.

**The look test levels.** `Content/Python/Demo/make_look_test.py`, run the same way, makes **Demo > LookTest_Evening** and **LookTest_Day**: rows of small fields that light themselves, one panel for each of the eight looks, for steps of Flame Glow, Smoke Glow and Flame Stretch, for seven fuels, and under two kinds of trees, each with a sign. Fly along the rows to compare them. The director's number keys change the weather for all of them at once.

To run a script without opening the editor: `UnrealEditor-Cmd.exe <YourProject>.uproject -ExecutePythonScript="<full path to make_fire_looks.py>" -unattended`. Don't add `-nullrhi`, because the materials need a renderer to compile.

## 3. Your first fire, in five minutes

1. **File > New Level > Basic**.
2. **Window > Place Actors**, search for **Weather Director** and drag one onto the floor. In its **Details**, under **Weather > Try It**, tick **Number Keys Change Weather** and **Show Readout**.
3. Search Place Actors for **Weather Fire Field** and drag one onto the floor. A box 256 metres square appears: the ground that can burn, tall grass unless you say otherwise. Set its **Location** X and Y to **0**. Under **Weather > Fire > Try It**, tick **Keys Light Fires**.
4. Select **Player Start** and set its Location X and Y to **0** and Rotation Z to **0**, so that you start in the middle, looking downwind.
5. Press **Play** and click in the game view. Press **F**. A fire starts 30 metres ahead of you. It runs away from you with the wind, spreads slowly at its sides and creeps back toward you into the wind. Seen from above it becomes a long egg pointing downwind.
6. Press **J** for a hot, dry, windy afternoon (32 °C, 15% humidity, 6 m/s), wait half a minute and press **F** again. This fire runs about three times as fast, throws embers that start spot fires ahead of it, and may raise a fire whirl.
7. Press **4** for rain and watch the fire die within seconds. **H** puts every fire out at once.

The other keys: **G** lands a lightning stroke 30 metres ahead, **K** swings the wind a sixth of a turn (the wind shift fire crews fear), **T** walks a drip torch along a 40 metre line, and **L** wets a line 6 metres wide. The number keys change the weather (section 7), and **0** hides or shows the readout.

The orange readout at the top left is the fire's numbers: cells alight, hectares burned, how fast the head runs, flame length, power, spot fires, then the fire danger and the fire's air where you stand. The same keys work from the console: `Weather.Fire Light`, `Strike`, `Out`, `Torch`, `Strips 3`, `Hose 2`, `HotDay 6`, `WindShift`.

## 4. The fire looks

Select the Fire Field and change **Look**, under **Weather > Fire > Drawing**. It changes while the game runs, so you can flip through them in play:

| Look | What it is |
| --- | --- |
| Plain | Orange cones that lean as the flames lean, dark tiles for burned ground and grey balls for smoke. For working on the fire's behaviour |
| Soft | Soft, glowing puffs of flame |
| Cel Shaded | Flat bands of colour with a dark edge |
| Painterly (default) | Brush-stroke flame |
| Shards | Faceted crystal flame |
| Illustrative | Inked shapes with warm shadows |
| Rim Glow | Dark flame with a bright rim |
| Illustrative Cool | Inked shapes with cool shadows |
| Sunset Shards | Faceted flame shaded from gold at the ground to deep red at the tips; the look of the game Burn Director |

Every look draws flame, smoke, rising ash over fresh black ground, wisps from embers smouldering in the litter, and a spinning column of flame and ash for each fire whirl. The puffs take their shape from the fire maths: flames lean by the wind and the slope, stand as tall as the flame height says, and a head fire looks different from one backing into the wind.

Three dials beside it tune any look while the game runs. **Flame Glow** sets how brightly the flames glow, times the look's own glow: turn it down if the flames read white in a bright level. **Smoke Glow** does the same for the smoke, dust and ash, and **Flame Stretch** sets how far the flames stretch along their motion. **Light At Start**, under **Try It**, lights a fire in the middle of the field when the level starts.

`Content/Python/Demo/make_look_test.py` builds a test level of panels: small fields side by side, each burning from the start, one for each look, for steps of the three dials, for different fuels, and under trees. See [section 2](#2-the-looks-sounds-plants-and-demo-level).

To use your own art, set **Look Card Mesh**, **Look Flame Material** and **Look Smoke Material**. The material reads its look from a scalar parameter called **Style**, plus **Stretch** and **Glow**, and each puff's numbers from 11 per-instance custom floats (seed, size, age, heat, velocity, centre, height above ground). `WeatherFireLooks.h` documents them.

## 5. Grass, bushes, fallen wood and trees

A Fire Field draws what grows on it, as thick and as tall as each fuel carries, from the same numbers the fire burns. GR1 is a thin cover of ankle-high tufts, short grass stands a foot high and tall grass waist high; chaparral is head-high brush; timber litter has fallen branches, and TL7 big fallen logs; slash is a clutter of logs and branches. A Fire Fuel box with **Trees** ticked grows a stand of them, as tall as its **Tree Height Metres** and as close as its **Tree Cover**. Conifers stand over most fuels and broadleaf trees over the broadleaf litters (Hardwood Litter, TL2, TL6 and TL9); about one tree in twenty-five is a dead snag. The same field is planted the same way every time.

As the fire comes, grass and bushes glow under the flames and burn to black, the grass down to stubble tipped with grey ash and the bushes to bare branches, all standing on a skin of ash with a ragged edge. Trees go a dull dark brown as high as the heat reaches (a fire beside a stand scorches it too), blacken from the ground up, go black with glowing patches of embers while the fire is in their tops, and stand as black poles once a crown fire has run through them. Each tree burns with the ground under the whole of its crown, not only where its trunk stands. Leaves let the light through, so a stand with the sun behind it glows green at its edges. Where rain or water puts a fire out, the ground it burned stays black, though the fuel it had not yet burned there can still catch again. **Mow** cuts the grass and bushes down, and **Set Fuel At** grows what the new fuel carries, on burned ground too. The grass is straw when cured and green when lush, by the field's **Greenness** or its season, and bends with the wind at the field.

The settings are under **Weather > Fire > Fuel Art**, and are read when the level starts:

- **Draw Fuel**: on by default. Turn it off to draw your own ground cover.
- **Fuel Art Density**: how much to draw, as a share of what the fuel carries, 0.05 to 2. Lower it for a slower machine.
- **Fuel Art Distance Metres**: how far from the camera grass, bushes and fallen wood are drawn, 150 by default. Trees are drawn as far as they can be seen, with simpler versions far away.
- **Grass Meshes**, **Shrub Meshes**, **Log Meshes**, **Limb Meshes**, **Conifer Meshes**, **Broadleaf Meshes** and **Snag Meshes**: the meshes each kind is drawn with, one picked for each plant. The plugin's own are set.

To make the plants lighter or darker, open `M_FuelPlant` and change the defaults of **LeafLift** (1.5: how much the darkest leaves are lifted), **WoodLift** (1.2, the same for wood and bark) and **LightThrough** (0.6: how much light the leaves let through). **Charcoal** is the black that burned wood and leaves turn. **EmberGlow** (2, and 1.5 on `M_FuelGrass`) sets how brightly burning plants glow: raise it in a bright midday level where the embers don't show, and lower it if they read white. Running the script again keeps your change.

**Your own meshes.** A mesh stands on its origin with its up along Z; a log or a branch lies along its Z, centred on its origin, with its ground side toward its X. It is sized to the fuel by its bounds. Its material needs **Used with Instanced Static Meshes** ticked. To burn with the fire, it reads four per-instance custom data floats: 0 how black it is, 1 how hot it glows now, 2 how high a tree's needles are browned (a share of its height), and 3 the share of its leaves burned away (for grass, its height in centimetres). `WeatherFireFuelArt.h` documents them, and a material that ignores them simply doesn't burn. To use the plugin's own `M_FuelPlant` or `M_FuelGrass` on your mesh, give its vertices their colour (linear, as glTF stores it) and a first UV whose V runs from 1 at the ground to 0 at the top in Unreal (0 at the ground and 1 at the top in Blender's UV editor, since export turns it over): that is how high up the plant each point is, which the scorch and the char climb. Its U is not read.

**What it costs.** The plants are a few hundred triangles each. A field 256 metres square of tall grass draws about 29,000 clumps of grass, and the demo's stand of young conifers a tree every 7 or 8 metres. A field too big to draw all of is drawn thinner, and says so in the Output Log: at most 400,000 grass clumps, 150,000 bushes, 30,000 logs, 80,000 branches and 100,000 trees. The field is drawn in up to 16 tiles, so a fire redraws only the tiles it is in, ten times a second.

## 6. The fire's sound

The Fire Field plays five looping sounds, each heard from where the player's ears are, by the same rules the fire uses:

| Slot | When you hear it | Ships with |
| --- | --- | --- |
| Backing Fire Sound | Low flames close by: a slow fire creeping into the wind | A recording of a real grass fire backing through a meadow |
| Head Fire Sound | Tall flames close by: the fire running with the wind. The two fade into each other as the near flames grow past about a metre | A recording of a real fire running |
| Roar Sound | A lot of tall flame anywhere near. Carries much farther than the crackle | Empty: set your own |
| Whirl Sound | The nearest fire whirl, heard from where it spins | Empty: set your own |
| Smoulder Sound | Standing on or beside burned ground | Empty: set your own |

They are under **Weather > Fire > Sound**, with **Play Sound** to turn it all off and **Sound Volume** for the whole fire. Any looping Sound Wave or Sound Cue works in a slot. The fire's own maths fades each loop with distance and places it, so give the sounds no attenuation of their own.

## 7. The weather

One **Weather Director** per level runs the weather. Pick its **Start Preset** in Details, or change it while the game runs:

| Key | Preset | Wind | Rain or snow | Visibility | Temperature |
| --- | --- | --- | --- | --- | --- |
| 1 | Clear | 3 m/s | none | 20 km | 15 °C |
| 2 | Light Snow | 4 m/s | light snow | 2 km | -1 °C |
| 3 | Heavy Snow | 9 m/s | heavy snow | 400 m | -4 °C |
| 4 | Rain | 6 m/s | rain | 4 km | 12 °C |
| 5 | Thunderstorm | 14 m/s | heavy rain, lightning | 1 km | 22 °C |

From a Blueprint: **Find Weather Director**, then **Set Preset** (with how many seconds the change takes) or **Set Weather** with your own numbers. From the console: `Weather.Preset Thunderstorm 30`.

The director moves the level's Exponential Height Fog (and makes one if there is none), turns the level's Wind Directional Source to the wind, and draws snow and rain around the camera. It keeps count of the ground: snow depth, wetness, ice and grip, for your game to read with **Get Ground**.

**Quality**, under **Weather > Full**, sets how deep the weather goes. **Low**, the default, is the weather above, cheap enough for any machine. **Medium** and up add the full fire weather: wind that changes with height and terrain (town or open field), the ground's own temperature in the sun, rain and snow sorted by the air they fall through (with freezing rain and ice pellets), floods on low ground, and a day with its own high and low. **High** and **Full** draw four and sixteen times the flakes and drops, and **Full** turns on Unreal's volumetric fog. Full also lets a big fire pull in its own wind, shade the fuel with its smoke, and grow a thunderstorm whose lightning starts more fires.

At any Quality you can also place weather that moves across the level: a **Weather Front** (or **Weather Warm Front**) that changes the air behind it, with a wind shift a fire feels, and a **Weather Storm Cell**, a travelling thunderstorm with its own rain, lightning and gust front. The director's **Dust Devils** setting adds dust devils on hot afternoons.

For anything the wind should push, add a **Weather Wind Push** component. A physics body needs **Simulate Physics** ticked; a character needs nothing more.

For lightning, bind the director's **On Lightning Strike** and **On Thunder** events and play your own flash and thunder. Thunder arrives about three seconds after the strike for every kilometre. A Fire Field listens for strikes and may start a fire where one lands in dry fuel.

## 8. Temperatures: Celsius, Fahrenheit or Kelvin

Every temperature setting is stored in Celsius and tagged as a temperature, so the Details panel shows it in the unit you pick under **Editor Preferences > General > Appearance > Units > Temperature**. You can type a value in any unit.

The on-screen readouts follow the director's **Readout Temperature Units** (Celsius, Fahrenheit or Kelvin). Your own UI can use **Convert Temperature** and **Temperature Text**, which take Celsius and the unit you want: `Temperature Text(15, Fahrenheit)` gives "59 F". The functions your game calls take and return Celsius.

## 9. Using it from your game

**The fire.** Get the field with **Get Actor Of Class** (Weather Fire Field), then:

- **Change the fire:** **Ignite** (a match, or a patch), **Light Along** (a drip torch walking a path; call it each tick with the two places a torch was and is), **Strip Fire** (rows lit one behind another), **Douse** and **Douse Line** (water: a lasting wet line), **Mow**, **Set Fuel At**, **Strike At** (lightning), **Roll Log**, **Put Out**.
- **Read the fire:** **Get Fire At** (flame length, lean, height, depth, tree tops left, water on the ground), **Get Fire Danger** (what a fire would do here before one is lit), **Get Smoke At**, **Get Heat At** (the heat on a person; **Advance Burn** keeps count of the harm), **Get Fire Wind At**, **Get Ground Wind At**, **Get Totals**.
- **The season and the drought:** **Set Season Index**, **Set Drought**, **Get Season**, **Get Drought**.
- **On Fire Event** tells you when something happens: a fire lit, flames across a break, a spot fire, trees torching, a whirl, an ember gone to ground, the last fire out.

Call these after the field's Begin Play, which is where the field is made. **Set Drought** and **Set Season Index** can be called earlier.

**What grows where.** Drop a **Weather Fire Fuel** box inside the field to change the fuel there: a road (nothing to burn), a mown strip, a stand of trees, or any of the forty fuel models. A field reads the height of the ground under each cell when the level starts, so fire runs faster uphill.

**The weather.** **Find Weather Director**, then **Set Preset**, **Set Weather**, **Get Local Weather** (the weather where something stands, with "feels like"), **Get Wind At Location**, **Get Ground**, **Get Felt Weather**, **Get Day** and **Get Level Report**.

**In C++**, add `"WeatherDirector"` to your module's `Build.cs` dependencies and include `WeatherFireFieldActor.h` or `WeatherDirectorActor.h`:

```cpp
AWeatherFireField* Fire = Cast<AWeatherFireField>(UGameplayStatics::GetActorOfClass(this, AWeatherFireField::StaticClass()));
if (Fire != nullptr)
{
	const FWeatherFireOutlook Danger = Fire->GetFireDanger(GetActorLocation());
	UE_LOG(LogTemp, Display, TEXT("A fire here would run %.0f m a minute"), Danger.RateMetresPerMinute);
	Fire->Ignite(GetActorLocation() + GetActorForwardVector() * 1000.0, 2.0f);
}
```

The maths needs no engine at all: `Public/WeatherFire.h` (`wx::fire`) and `Public/WeatherFull.h` (`wx`) build in any C++17 compiler, so a server, a tool or another engine can run the same fire.

## 10. Settings worth knowing

On the **Fire Field** (under **Weather > Fire**):

- **Fuel**: what grows on the field where no Fire Fuel box says otherwise.
- **Cell Metres**: the size of one burning cell. 2 by default; 4 is a quarter of the cost.
- **Seed**: the same number gives the same embers, whirls and lightning every time.
- **Fire Time Scale** and **Fuel Time Scale**: the fire burns at real speed and the fuel dries 60 times faster, so that rain and sun change things while you watch. Set either to what your game needs.
- **Lightning Starts Fires**, **Read Ground Height**, and the switches under **Hills** (**Air By Height**, **Slope Winds**, **Terrain Wind**) turn each rule on or off.
- **Draw Fuel**, **Fuel Art Density** and **Fuel Art Distance Metres**, under **Fuel Art**: the plants (section 5).

On the **Weather Director**: **Start Preset**, **Quality**, **Ground Time Scale**, **Fires Change The Weather** (smoke in the fog and the fire's pull in the wind), **Sun Light** (point it at the level's sun so the day and the night follow it), and **Readout Temperature Units**.

Every setting has a tooltip that says what it does and in what unit.

## 11. Packaging your game

- Set your own materials for the weather's flakes, drops and the plain look's shapes (**Snow Material**, **Rain Material**, **Flame Material**, **Burned Material**, **Smoke Material**), with **Used with Instanced Static Meshes** ticked. The engine's plain shape material, which these use by default, is fine in the editor.
- The fire looks' own materials and sounds, and the plants' meshes and materials, are referenced from the plugin's code, so they are packed with your game.
- The orange readout, the try-it keys and the `Weather.` console commands are for the editor and development builds. A shipping build has none of them.

## 12. Limits

- **One Weather Director per level**, and keep it in the main level, not a streamed sublevel.
- **One air for the whole level.** The weather has one temperature and dew point at a given height; storms and fronts change the wind and rain from place to place, and the fire's own air changes over its hills.
- **The ground is numbers.** Snow cover, puddles and ice are counted, not drawn. Read **Get Ground** and drive your own materials.
- **No shelter.** Wind, rain and snow pass through walls and roofs.
- **Lightning is an event.** It draws a plain debug line, left out of shipping builds; your game plays the flash and the thunder.
- **Roads and wet lines are not drawn.** The fire knows them; your level shows them. The field draws what grows on it (section 5) and darkens burned ground with plain tiles; the ground's own material is yours.
- **Plants are drawn, not solid.** Nothing collides with the grass, bushes, logs or trees.
- **Not replicated.** A multiplayer game should run the fire on the server and send what it needs.
- **Unreal 5.8, on Windows and Mac.** Built and tested with Unreal 5.8.4: with Visual Studio's MSVC compiler on Windows, and Apple's clang on a Mac. The same code builds on the 5.9 preview.

## 13. Credits and licence

The plugin, its code, its shaders and its documentation are copyright 2026 AIGameDev.olution, all rights reserved, and are licensed to you under the Fab end user licence that came with your purchase.

The plants' models were made in code for the game Burn Director (Epic MegaJam 2026) by the plugin's author, and are drawn here with the plugin's own materials, without that game's cartoon outlines. `Content/Python/FuelArt/CREDITS.md` has the details.

The fire's two sounds are recordings of real fires: the Maple Fire of 14 August 2016, from the Yellowstone National Park sound library of the US National Park Service (public domain; credit: National Park Service), and "Big Branching Fire #1" by Joseph Sardin, BigSoundBank (CC0). `Content/Python/FireSounds/CREDITS.md` has the details.

The fire's spread follows Rothermel (1972) and Andrews (2018), with the forty fuel models of Scott and Burgan (2005), crown fire after Van Wagner (1977) and Rothermel (1991), and spotting after Albini (1979). Its numbers are checked against BehavePlus.

Fire, BEHAVE! is not made by, affiliated with or endorsed by the USDA Forest Service or the makers of BehavePlus.
