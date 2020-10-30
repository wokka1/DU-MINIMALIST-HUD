# DU-MINIMALIST-HUD
Hud project for Dual Universe with a fuel tank monitor and a damage report system

![hudimage](https://raw.githubusercontent.com/Catharius/DU-MINIMALIST-HUD/main/images/top_all.jpg)
![hudimage2](https://raw.githubusercontent.com/Catharius/DU-MINIMALIST-HUD/main/images/top_av.jpg)
![hudimage3](https://raw.githubusercontent.com/Catharius/DU-MINIMALIST-HUD/main/images/top_front.jpg)
![hudimage3](https://raw.githubusercontent.com/Catharius/DU-MINIMALIST-HUD/main/images/side_av.jpg)


Features :
* Hide default fuel widgets if script is installed via autoconf
* Add a fuel tank monitor in the top left corner of the hud
* Add a ship layout based on the real position of your elements on your dynamic core, switch views by using option 2 key (ALT+2 by default)
* Available views are TOP, FRONT and SIDE
* Elements can be filtered (ALL,WEAPONS AND AVIONICS,AVIONICS ONLY,WEAPONS ONLY) by using option 1 key (ALT+1 by default)
* Add a damage report list on the left of the hud when something is damaged

## Patch note
30/10/2020 : 
* Easier hud positioning
* You can now switch between top view, front view and side view !
* A label has been added under the ship layout
* Better transparency management, damaged elements will be more visible
* More explicit lua parameters
* Hud rotation gone for now
* For advanced users, renderHTML() will now return an array with all hud elements separated, default css can be obtained via renderCSS()

How to use this script :
* You can use the autoconf to make a clean installation of the hud on a piloting seat (Cockpit not supported yet)
* Download minimalistic_hud.conf and copy paste it in your \Dual Universe\Game\data\lua\autoconf\custom
* Ingame, just right click on your piloting seat then go to advanced/run custom autoconfigure and select "minimalistic hud"

My todo list for the next versions:
* Show the ship layout outside of the hud  on a screen for example
* Cockpit version of the hud
* Full screen mode with elements names on the ship layout
* Repair Station mode for the repair crew

Disclaimer : This is an early build, you may encounter some hud positionning problems (Large core not tested yet). if so, you can adjust all parameters from the lua parameter menu on your piloting seat

## Fuel module
### List of lua parameters
* **MINHUD_show_fuel** : enable the fuel module
* **MINHUD_fuel_left_position** : fuel module position from the left side of the HUD
* **MINHUD_fuel_top_position** : fuel module position from the top side of the HUD
* **MINHUD_fuel_refresh_rate** : fuel module refresh rate every x seconds (useful if you have performance issues) 
* **MINHUD_fuel_show_remaining_time** : if fuel is lasting more than x hours, do not show remaining time, 0 to always show remaining time. It shows 10 hours by default.

## Damage Report module
### List of lua parameters
#### Functionalities
* **MINHUD_show_labels** : show/hide view labels
* **MINHUD_defaultFilter** : Sset the default filter when you start the script (1 for all,2 for avionics and weapons,3 for avionics only, 4 for weapons only)
* **MINHUD_defaultView** : set the default view when you start the script (1 for top,2 for side and 3 for front)
* **MINHUD_show_txt_module** : enable the ship damage text report
* **MINHUD_dmg_priority** : show damaged components (3) Below 100%, (2) Below 75%, (1) Below 50%
#### Customization
* **MINHUD_size_ratio** : change the size of the ship layout, use positive or negative numbers
* **MINHUD_left_position** : change the left position of the ship layout (Increase to move right)
* **MINHUD_top_position** : change the top position of the ship layout (Increase to move down)
* **MINHUD_label_position** : move the view label left or right (useful for centering)
* **MINHUD_txt_module_left_pos** : change the left position of the ship layout (Increase to move right)   
* **MINHUD_txt_module_top_pos** : change the top position of the ship layout (Increase to move down)
* **MINHUD_dmg_priority** : Change the size of the ship layout, use positive or negative numbers to scale up or down (Please note that you will need to adjust the x,y position
#### Script performances
**MINHUD_dmg_refresh_rate** : damage report refresh rate every x seconds, increase the value if you have performances issues.

### Filters
You can use alt+1 (option1) to switch between filter modes
* **ALL** : Show all elements below 75% by default (You can adjust using the damagereport_txt_priority parameter)
* **WP & AV** : Show only weapons & avionics elements
* **AVIONICS**  : Show only avionics components (Wings, adjustors, vertical boosters, engines, fuel tanks, etc..)
* **WEAPONS** : Show only weapons

You can use alt+2 (option2) to switch between views
* **TOP** : show the top view of your ship
* **FRONT** : show the front view of your ship
* **SIDE** : show the side view of your ship (The front of the ship is on the right)

The dynamic core unit and resurection nodes will always be visible no matter what filter you have selected since they are really important.

If you do not have elements of one view, like no weapons for example the view will be hidden until you switch to another view.

## Lua scripting

### Fuel module script
Create a new fuel module :
```lua
fuel_module = FuelModule.new()
```
Get the html code of the module
```lua
fuel_html=fuel_module:renderHTML()
```

### Damage report module script
Create a new damage report module :
```lua
damage_rep = DamageModule.new()
```
Get an array filled with all html hud parts :
```lua
damage_html=damage_rep:renderHTML()
top_view_html = damage_html[1] -- The top view of the ship layout
front_view_html = damage_html[2] -- The front view of the ship layout
side_view_html = damage_html[3] -- The side view of the ship layout
table_view_html =  damage_html[4] -- The text damage report
```
Get default css for all that :
```lua
damage_css = damage_rep:renderCSS()
```
Get the current seleted view (1,2 or 3 for top, front, side)
```lua
  damage_rep:getActiveView()
```
Change the view
```lua
damage_rep:nextView()
```

Change the filter
```lua
damage_rep:nextFilter()
```

