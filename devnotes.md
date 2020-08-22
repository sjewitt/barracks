
door tags:
5, 0
36, 256, 
37, 256
38, 256
39, 256


DSDOROPN
DSDORCLS

3D FLOORS AND ACTIONS:
----------------------

TAG	DESCRIPTION		OTHER (is it a 3d door etc.)
---	-----------		----------------------------
1	water
2	bridges over water
3	
4	main gate
5	gate control sector	Scripted open/close
6	sentry platform
7	
8	
9	
10	
11	
12	
13	
14	
15	
16	
17	
18	
19	
20	
21	
22	
23	
24	
25	
26	
27	
28	
29	
30	
31	
32	
33	
34	
35	
36	
37	
38	
39	
40	


CEILINGS:
---------

8
9
10
30
39
40




3D floor control sectors:
-------------------------
Starting at one immediately to the right of the canal, in void. Going clockwise and ttb/ltr at stairs block  

sector	ceiling	floor	ceiltex		floortex	walltex		tag	notes
-------------------------------------------------------------------------------------
 #2	-8	-128	fwater1		fwater1		gray1		2	water
 #5	0	16	flat1		flat1		door1		2	water
 #9	64	-8	flat20		flat20		 gray1		5	main gate moving control sector
 #23	256	240	flat1		flat1		gray1		-
 #170	392	320	ceil5_1		flat1		door1		-
 #256	392	384	ceil5_1		ceil5_1		doortrak	-	roof (multi)
 #46	252	244	flat23		flat23		gray1		2	walkway
 #192	252	244	flat23		flat23		shawn2		2	walkway
 #86	152	64	flat1		flat1		gray1		-	window wall, gnd floor
 #87	281	192	flat1		flat1		gray1		-	window wall, 1st floor
 #89	385	320	flat1		flat1		gray1		-	window wall, 2nd floor
 #231	320	256	flat1		flat1		SPCDOOR3	36	NE sentry door control
 #90	128	112	flat20		flat20		doortrak	-	1st floor
 #91	256	248	flat20		flat20		 doortrak	-	2nd floor
 #92	392	64	ceil5_1		flat1		gray1		-	wall above main door
 #263	128	64	flat20		flat1		gray1		-	gnd floor internal door upper wall
 #264	256	192	flat20		flat1		gray1		-	1st floor internal door upper wall
 #263	384	320	flat20		flat1		gray1		-	2nd floor internal door upper wall

NOTE: central doors (do not have a 2nd floor set, so need separate tag? can I do multi-linedef here?):
 #283	128	64	flat20		flat1		gray1		-	gnd floor door upper
 #284	256	192	flat20		flat1		gray1		-	1st floor internal door upper wall
 #232	320	256	flat1		flat1		SPCDOOR3	37	SE sentry door control
 #149	392	384	ceil5_1		ceil5_1		doortrak	-	roof (multi) above stairs

NOTE: flight 1 is normal sector floor heights:
 #148	256	224	flat1		flat1		gray1		-	step, flight 2
 #150	248	216	flat1		flat1		gray1		-	step, flight 2
 #151	240	208	flat1		flat1		gray1		-	step, flight 2
 #152	232	200	flat1		flat1		gray1		-	step, flight 2
 #153	224	192	flat1		flat1		gray1		-	step, flight 2
 #154	216	184	flat1		flat1		gray1		-	step, flight 2
 #155	208	176	flat1		flat1		gray1		-	step, flight 2
 #156	200	168	flat1		flat1		gray1		-	step, flight 2
 #157	192	160	flat1		flat1		gray1		-	step, flight 2
 #158	184	152	flat1		flat1		gray1		-	step, flight 2
 #159	176	144	flat1		flat1		gray1		-	step, flight 2
 #160	168	136	flat1		flat1		gray1		-	step, flight 2
 #161	160	128	flat1		flat1		gray1		-	step, flight 2
 #162	152	120	flat1		flat1		gray1		-	step, flight 2
 #163	144	112	flat1		flat1		gray1		-	step, flight 2
 #164	136	104	flat1		flat1		gray1		-	step, flight 2
 #165	128	96	flat1		flat1		gray1		-	step, flight 2	
 #233	384	248	flat1		flat20		gray1		-	wall at end of flight 1

 #230	320	256	flat1		flat1		SPCDOOR3	38	SW sentry door control
 #229	320	256	flat1		flat1		SPCDOOR3	35	NW sentry door control

Control sectors for internal doors:
-----------------------------------
Need to be independent AND be controlled by different scripts. Are to the SOUTH of the stair control sectors.

Because |I have 3d doors over 3d doors, I need to independently control them. Therefore, I need a script that will
detect player height and do a switch/case to move the relevant control sector. Can I do this?...

First control sector row: 	gnd floor
second control sector row:	1st floor
third control sector row:	2nd floor

proceed CLOCKWISE from main gate. i.e each set of doors for a floor starts at the SE corner room,


 #39	64	0	flat1		flat1		SPCDOOR3	42	NE, gnd, office door
 #39	192	128	flat1		flat1		SPCDOOR3	43	NE, gnd, office door
 #39	320	256	flat1		flat1		SPCDOOR3	44	NE, gnd, office door


logic for multilevel doors:

script n void(playerHeight){
	int[3] = {n,n,n};	//where n are control sectors for each floor
	//map playerHeight to appropriate door - may not be relevant for single player
	//it is an exercize im switching control sector to move though...




}



int switchHeightMap[8][5] = {
	{5, 0, "Main gate",false,1},
	/* sentry doors */
	{35, 256, "Northwest sentry",false,2},
	{36, 256, "Northeast sentry",false,3},
	{37, 256, "Southeast sentry",false,4},
	{38, 256, "Southwest sentry",false,5},
	
	/* NE office doors, all floors */
	{42, 0, "Northeast sentry",false,6},
	{43, 128, "Southeast sentry",false,7},
	{44, 256, "Southwest sentry",false,8},
};

script 7 (int switchHeightMapLookupIndex ){
	log(s:"stacked door script");
	int controlSectors[3] = {42,43,44};
	/* check whether player is same height as current door */ 
	for(int x=0;x<3;x++){
		log(s:"looping ", i:x);
		if(checkPlayerDoorProximity(controlSectors[x], switchHeightMapLookupIndex )){
			log(s:"opening door at ",i:controlSectors[x]);
			FloorAndCeiling_RaiseByValue (controlSectors[x], 12, 64);
			//see https://github.com/rheit/zdoom/blob/master/wadsrc/static/filter/game-doomchex/sndinfo.txt
			PlaySound(switchHeightMap[switchHeightMapLookupIndex][4],"doors/dr1_open");
			Delay(144);
			FloorAndCeiling_LowerByValue (controlSectors[x], 12, 64);
			PlaySound(switchHeightMap[switchHeightMapLookupIndex][4],"doors/dr1_clos");
			TagWait(controlSectors[x]);
		}
		
	}
}

 
/*
test if player is at the height for door activation:
*/
function bool checkPlayerDoorProximity(int sectorTag, int switchHeightMapLookupIndex){
	int switchHeight = switchHeightMap[switchHeightMapLookupIndex][1];
	bool trigger = false;
	int zpos = (GetActorFloorZ(0))/65536; //see https://zdoom.org/wiki/Definitions#Fixed_point_numbers
	if(switchHeight >= zpos-24 && switchHeight <= zpos+24){
		trigger = true;
	}
	return(trigger);
}





#include "zcommon.acs"

/*
test of stacked doors
*/
script 1 (int index){	//CONTROL SECTOR INDEX!!!
	FloorAndCeiling_RaiseByValue (index, 12, 64);
 	//see https://github.com/rheit/zdoom/blob/master/wadsrc/static/filter/game-doomchex/sndinfo.txt
 	PlaySound(index,"doors/dr1_open");
	Delay(144);
	FloorAndCeiling_LowerByValue (index, 12, 64);
	PlaySound(index,"doors/dr1_clos");
	TagWait(index);
}




#include "zcommon.acs"
int switchHeightMap[2][3] = {
	//	controlsectortag,	triggerHeight,	soundThingId
	{	3, 					0,				1},
	{	2, 					128 ,			2},
};

/*
test of stacked doors with independent switches:
*/
script 1 (int index){
	FloorAndCeiling_RaiseByValue (index, 12, 64);
 	//see https://github.com/rheit/zdoom/blob/master/wadsrc/static/filter/game-doomchex/sndinfo.txt
 	PlaySound(index,"doors/dr1_open");
	Delay(144);
	FloorAndCeiling_LowerByValue (index, 12, 64);
	PlaySound(index,"doors/dr1_clos");
	TagWait(index);
}


/*
test of stacked doors switched by vertically stacked switches. This is to simulate multi-3Dfloor sectors
behaving as doors - i.e. they should trigger themselves but NOT the dorrs directly above or below.
*/
script 2(void){
	/*
	These refer to the indicies of the vertically stacked 3D floors (that are doors) defined 
	in the lookup data attay 'switchHeightMap[]':
	*/
	int controlSectorLookup[2] = {0, 1};
	
	/*
	because we are on a single map line, we cannot differentiate between the door lines ('cos they are the 
	SAME line...) so when we activate it, we need to determine the correct code path to take by determining
	the player height first and then activating the correct eD door object.
	*/
	for(int x=0; x<2; x++){
		//test if the player is correct height for current door (i.e. is player same height as control sector):
 		if(checkPlayerDoorProximity(controlSectorLookup[x] )){
			
			log(s:"opening door at ",i:switchHeightMap[controlSectorLookup[x]][0]);
			log(s:"via control sector ",i:switchHeightMap[controlSectorLookup[x]][0]);
			
			//now move the relevant control sector:
 			FloorAndCeiling_RaiseByValue (switchHeightMap[controlSectorLookup[x]][0], 12, 64);
 			//see https://github.com/rheit/zdoom/blob/master/wadsrc/static/filter/game-doomchex/sndinfo.txt
 			PlaySound(switchHeightMap[controlSectorLookup[x]][2],"doors/dr1_open");
 			Delay(144);
 			FloorAndCeiling_LowerByValue (switchHeightMap[controlSectorLookup[x]][0], 12, 64);
 			PlaySound(switchHeightMap[controlSectorLookup[x]][2],"doors/dr1_clos");
 			TagWait(switchHeightMap[controlSectorLookup[x]][0]);
			//break out of loop
			break;
 		}
		
 	}
}

/*
Check the player z-index against the height of the current switch. When we use >1 3D floor in the same
location, we get stacked floors, doors etc. We need to ensure that the player is in fact at or nearly at
the height of the supplied 3D floor, as defined in the lookup array.

Return true or false:
*/
function bool checkPlayerDoorProximity(int switchHeightMapLookupIndex ){
	int switchHeight = switchHeightMap[switchHeightMapLookupIndex][1];
	log(s:"in check height func - switch: ", i:switchHeight);
	bool trigger = false;
	int zpos = (GetActorFloorZ(0))/65536; //see https://zdoom.org/wiki/Definitions#Fixed_point_numbers
	log(s:"in check height func - player: ", i:zpos);
	if(switchHeight >= zpos-24 && switchHeight <= zpos+24){
		trigger = true;
	}
	return(trigger);
} 



stairwell door:
---------------
TAG: 62
Control sectors:
TAG	sound thing TID
---	---------------
63	21
64	22
65	23

LH central door
---------------
TAG: 66

Control sectors:
TAG	sound thing TID
---	---------------
67	24
68	25
// 69	26


Script as per https://zdoom.org/wiki/ACS_LockedExecute - access to each floor in turn?
 - add doors to the stairwell.



sector tag 78: light block plus standard floor def





Need to add height test to monster activate code - move reom line special to actual code...

secret teleport - block monsters

secret teleport - return direction!!

platform switches - ensure that trigger does not happen until slave switches have risen.
MAKE THE RED KEY HARD TO GET!!!!

barrier width:
2832 -> 3328 = 496 + pillar = 544

### 13/08/20
  - Split wall 3D sectors lengthways to do textures
  - Add windowsill 3D sectors above trap window shutters
  - ~~ make window shutters openable from inside (switch? or just a window action? ) ~~
 
#### consolidate gnd-1st floor window 3D sectors def:
  - FLAT1
  - FLAT1
  - ceil 152
  - floor 64
  - light 160
 
 
#### Tags to split into inner and outer sectors:
 8 (windows)
 29 (platform doors)
 11 (main door wall)
 
entry slope over slime sea stack y: -580


### EXIT LOGIC:
 - Level 3 SE door to stairs cannot be opened from inside
    - switch (s1) needs to be added to NE room. This CANNOT be triggered until the switch on SE platform is activated. But as this already activated the arachnos, that is a challenge. 
 
        - This CANNOT be triggered until the switch on SE platform is activated. But as this already activated the arachnos, that is a challenge. 
 - Red key to open the SW door
    - activate arachnos
 - SE switch 

###BEDS

#### Tags to replace
Need new tags (below) as they already define something, so I need to copy the def 
 
 
#### New tags
These are  bed defs AND must be added to floors/roof control sector:
 108 - pillows, 1st only 
 110, 1st, 2nd
 
 109 - bed, 1st only
111 - bed 1st, 2nd


FUCKNUTS!!!!
------------
Combine stff done on 'IslandFortress.wad' (steps, additional monster placements - and extra 3D floors in comp rooms/beds) with 'islndfts.wad' (pillar decorations, bridge to pillars). Use islndfts.wad as master!!!
 - check each, make list of diffs!
 - lights 
 - beds
 - cliffs
 - sea pillar etc...


16:24 17/08/2020

brutal doom issues:
-------------------
https://forum.zdoom.org/viewtopic.php?f=3&t=55437 (dormant actors)

beautiful doom issues:
----------------------
key!!!



 - lights in stair corridors
 - reduce lighting in corridors
 - why the fuck is the hall and the room JOINED???
 - shorten computers in 1sf floorWest room or split textures (use silver or compspan)
 - demons in 2nd floor bedroom
 - demons in 2nd floor corridors
 - WTF do I do about the exit logic?
	need to decide what to do with arachnos and NW room - let's have a secret, protected by a vertical forcefield? Use same logic as per the doors and your z-pos.
 - make exit VISIBLE from the red platform?
 - Add secret item to tele dest (can I make the tele plat a secret?)
 - change building lighting to control sector lighting?
 - re-fix the superimposed doortrak texture!!
	- it's just the five doors
 - fix repeated exit switch triggering (bars will descend tooo far...)
	- also, slow them




new lights - need ceiling adjust:
112	x=-16	y=32 
113	x=16	y=8



22/08/20:
---------
TABLE for top 1st floor!


 - COMPUTER ENDS! Bad texture aligmnet (alter 8Mu at each end) 		- OK
 - men in 2nd floor							- OK
 - men in corridors?							
 - men on opening stair doors						- OK
 - trench texture at back						- OK
 - split bak chaingunners into alcoves					- OK
 - move bullet box within reach gnd floor				- OK
 - gnd floor computer texture align					- OK
 - 2x chaingunners on stair - one at top w. libe blocking		- ok
 - 2nd floor monsters in bedroom					- OK
 - platform secret needs to be rocket launcher				- OK
 - revenants failed to trigger						- OK
 - misaligned crate texture						- OK
 - health in gnd floor W room?
 - more goodies outside? NEED TO PLAYTEST THIS

 - Add the security droid custom monster to the computer rooms as security thing!!!

