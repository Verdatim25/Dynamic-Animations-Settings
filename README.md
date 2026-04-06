# Dynamic-Animations-Settings
Dynamic Animations Settings for FDDA redone

Dynamic animations settings

DYNAMIC ANIMATION SETTINGS: Major Rework by Verdatim

Base script by Archangel

Description:
Dynamically changes FDDA item use animation settings via scripting
Requires FDDA Redone (dont want to include fdda redone script here, its not mine)

CREDITS:
Archangel - For original script, enhanced for better functionality
Lizzardman – original FDDA mutator logic, for the function `get_template_action_play_animation_fdda_config(item_section)`

Below is a guide assuming you know next to nothing about how to set regular FDDA anims
It does assume you know how to set up regular anims

SET UP GUIDE:
Quick Note: if the script cannot find the new section, it just uses the base section for the item
- USES GUIDE
This script allows for there to be different animations and settings for different uses, heres how you set it up
Create a new mod_animations_settings_*.ltx file
Create a new section for the altered anim that is in the format [item_section]_use_[number]; where [item_section] is the item_section anim you want to change, and number is the uses in the item.
For example, if you want to change the animation of the flask use when its its last use then create a new section called "flask_use_1". This anim will then play when the item is left with one use
If you want to change the animation when the uses left in the flask is 2, just create a new section called "flask_use_2".
For 3, create a new section called "flask_use_3"

- VARIANTS GUIDE
This script also allows variants to exist, heres how to set it up.
Under the animations settings section you want to edit, simply add a "variants = [animation_section_name1], [animation_section_name2], ... , [animation_section_name]" where your animation_section is another set of animation settings, the script will then randomly choose between the animations equally. If you want one animation to play more than another, simply add its entry more than once.
For example, if you want [flask] to sometimes play the [water_quality] anim or the [tea] anim, (doesnt change the effect applied btw), then do this in your mod_animations_settings_*.ltx
![flask]
variants = water_quality, tea
This will make the [flask] item play the [flask] anim a third of the time, [water_quality] another third of the time, and [tea] the last third of the time.
Note that this can be stacked with the uses modifier, for example if [flask_use_1] has variants it will play those variants by themselves only when the flask is at its last use.

Order Of Operations:
The script first checks if the section with [item_use_(current_item_uses)] exists, and THEN checks for animation variants under the NEW section, not the other way around.
Let's say for example your using a flask with 1 use left and you have a [flask] section like this
[flask]
...
variants = tea

BUT you also have a [flask_use_1] section added like this:
[flask_use_1]
...
variants = 

Then the variants of [flask] WILL NOT BE CONSIDERED. If you want the [flask_use_1] section or other use_number sections to not be considered, put "uses_allowed = false" under the [flask] animation settings.
So it will look like this:
[flask]
...
variants = tea
uses_allowed = false

This will make the script NEVER consider the uses for this specific animation settings section. Ideally you could delete the sections but this is a fallback just in case. 
NOTE: "uses_allowed" is true by default


SAMPLES

Here's a sample section:
	[flask]
	snd = interface\item_usage\new_drink_flask
	anm = item_ga_flask
	cam = itemuse_anm_effects\sexo_flask_drink.anm
	tm = 7250
	helm = false

Explanation:

snd: its the sound played when using the item, just the direct path to the sound starting from the “sounds” folder

anm: its the animation section but with "_hud" removed, needs to be created under mod_system_*.ltx, sample below. In this case the anm will point to "item_ga_flask_hud" and NOT "item_ga_flask", keep this in mind!

cam: The camera animation; i.e. the animation added to the camera when the animation is playing. Just the direct path to the .anm starting from the “anims” folder

tm: timing, i.e. the time taken for the effect application. The og script is pretty weird. to get the correct timing here you have to take the timing (in seconds) that you want the effect to apply, then multiply it by a thousand and divide it by 0.367, this final timing will be the number you put in tm

For example if you want the effect to apply after 2 seconds, then your tm value needs to be (2*1000) / 0.367 which is roughly 5450, set your tm = 5450 and it should be good to go!

helm: stands for helmet. Determines whether or not in strict fdda mode if the item can be used when a helmet is equipped. Here it is false because you cannot drink from a flask with a helmet on. If set to true, then the item will be able to be used if in strict FDDA mode AND wearing a helmet. Does nothing if strict FDDA mode is off.

Sample anm section:

	[item_ga_flask_hud]
	
	hud_fov                     = 0.8
	
	hands_position				= 0.0,-0.02,0.14
	
	hands_orientation			= 0.0,0.0,0.0
	
	hands_position_16x9			= 0.0,-0.02,0.14
	
	hands_orientation_16x9		= 0.0,0.0,0.0
	
	item_visual             	= dynamics\flask\wpn_water_flask_hud.ogf
	
	anm_ea_show		= flask_rework_hand_drink, flask_rework_drink
	
its the same as any hud section, one thing to note however is that your anm NEEDS to be anm_ea_show or else it wont play.
Yes theoretically you could point to any hud section and it should work as long as it includes anm_ea_show.
This means if you give a weapon an anm_ea_show then it should theoretically work, but dont quote me on that
Just in case you dont know, first string in anm_ea_show is the hand animation, the second is the item animation.
