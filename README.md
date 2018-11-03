Hello all, this is my first post here so I figure I'll make it count.

Note: Throughout the post I will refer to OpenIV installers and ```.oiv``` installers - [Info can be found here](https://fileinfo.com/extension/oiv)

In this tutorial, I'll show you how to make a ```.oiv``` installer for your mod.
```.oiv``` installers make it easier for the users to install mods, rather than users attempting to follow the readme file which can involve editing handling files and replacing ```dlclist.xml``` constantly, OpenIV installers allow the user to click install and have everything done without them doing much of anything. As a result, users will more likely choose the easier .oiv route and get you more downloads because they don't want to do the work of manually installing another mod.

From a developers perspective, ```.oiv``` files appear to be more work than they're worth on the surface, however in my opinion the pros of OpenIV installers outweigh the cons. For starters, OpenIV installers are much cleaner than the manual installations, especially for add-on vehicles, the reason being most manual install mods come with ```dlclist.xml``` and instruct the user to replace the original ```dlclist.xml``` with the provided one in the mod. In doing so, the user also removes in game access to all their other add-on vehicles. OpenIV installers, on the other hand, append data to the end of the dlclist file and do not affect previous entries in the file. Additionally, OpenIV installers are very easy to update once you have them created, I will go more in depth later on in the post.

With that said, let's begin.

## Step One
### Creating the required files
Create a folder which will contain your installer files, I named mine "Install."
Inside the folder, make a file called ```assembly.xml```
If you want your installer to have an icon, get an image that is **exactly** 128x128px, name it ```icon.png``` and place it in the Install folder
Last but not least, create a folder called content within the main folder (Install in my case)
Your folder will (hopefully) end up looking like this:

![Files and Folders appropriately placed within Install folder](https://i.imgur.com/mllBtOG.png)

## Step Two
### Setting up your ```assembly.xml``` file
Open up ```assembly.xml``` in your favourite text editor (Notepad or Visual Studio will work)

Copy and paste the below: [(If you aren't familiar with XML, you can learn more here)](https://www.ibm.com/developerworks/library/x-newxml/index.html)

```
<?xml version="1.0" encoding="UTF-8"?>
<package version="2.1" id="{1234ABC5-6D7E-8F89-G873-11QG279GFF85}" target="Five">
<!-- Package version is the version of installer (MUST BE  2.1, NOT 2.0!), OpenIV uses this to determine how to install your mod. 2.0 is standard. id is the **unique** identifier for your installer. You can easily generate a GUID with any of the following sites: http://www.guidgen.com/, https://www.uuidgenerator.net/ or https://www.guidgenerator.com/-->
	<metadata>
		<name>Example Installer</name> <!-- The name of the mod -->
		<version>
			<major>1</major> <!-- The version of your mod, current settings are 1.0 = [major].[minor]-->
			<minor>0</minor>
			<tag>Beta</tag> <!--The tag on the mod, can be Beta, Alpha, Test etc;-->
		</version>
		<author>
			<displayName>Ckacmaster</displayName> <!-- Insert your name here -->
		</author>
		<description><![CDATA[A Bugatti Veyron 16.4]]></description> <!-- Mod description which goes between the CDATA[] brackets-->
	</metadata>
	<colors>
		<headerBackground useBlackTextColor="False">$FF272727</headerBackground>
<!-- useBlackText is if you want the mod name and such to be black rather than white. "$FF272727" is the background color of the header shown to the user while they're installing the mod. It is also the backdrop for the mod name in OpenIV. -->
		<iconBackground>$FF2E2E2E</iconBackground> <!-- The background color of the icon area -->
	</colors>
	<content>
<!-- This is where we will be adding all of our mod files and installation info for OpenIV's eyes only -->
	</content>
</package>
```

## Step Three
### Populating the content folder
Find your mod files, for the sake of this tutorial, I will be providing examples of a replacement adder (Bugatti) and an add on ninef (Audi R8 PPI).
I choose to format my files in this manner because I find it easiest to navigate through and manage. To each his own.
* ModName (folder)
* * Vehicles (folder)
* * * YFT and YTD files
* * Extras (folder)
* * * All extra files (vehicle modification yfts for example)

When you've got your mod files sorted to your liking, place the mod folder inside the content folder you created earlier. Note: you can place mod files such as ```.yft``` and ```.ytd``` files directly in the content directory if you like

You should end up with something along the lines of:
![Content folder](https://i.imgur.com/L8OH4O1.png)
The adder folder is the replacement Bugatti folder, and the r8ppi folder is for the add on R8 which will be used later on
![adder folder](https://i.imgur.com/12wgonF.png?1)
![vehicles folder](https://i.imgur.com/KHhr8Zb.png?1)
Good beans :thumbsup: 
## Step Four
### Completing the ```assembly.xml```
Navigate back to your ```assembly.xml``` file and open it with your favourite text editor. (I prefer VS2017)
Find this line and make some space below it like I did;
```
	<content>
<!-- This is where we will be adding all of our mod files and installation info for OpenIV's eyes only -->
    
    
	</content>
```
Now we can tell OpenIV what to do to install the mod. Following the readme provided by the mod developer Aitgamers - Mister Brooks, this mod needs to be installed in ```x64e.rpf```. The mod also needs to modify the ```vehicles.meta``` and ```handling.meta``` files - we will cover doing that as well. To start, we need to tell OpenIV to open ```x64e.rpf``` for us so we can install to it. We also need to advise OpenIV to create the ```x64e.rpf``` file if it doesn't already exist provided the user chooses the OpenIV option to install the mod to their mods folder. This can be accomplished through the following line
 
    <archive path="x64e.rpf" createIfNotExist="True" type="RPF7"> </archive>
The  ```type="RPF7"```  in the above is the version of RPF we are extracting. It will always be ```RPF7``` for GTAV.
The full path where the files will be installed is x64e.rpf\levels\gta5\vehicles.rpf, therefore we need to specify the vehicles.rpf to archive as well, format your code like this:
```
<content>
<!-- This is where we will be adding all of our mod files and installation info for OpenIV's eyes only -->

    <archive path="x64e.rpf" createIfNotExist="True" type="RPF7">
      <archive path="levels\gta5\vehicles.rpf" createIfNotExist="True" type="RPF7">


      </archive>
    </archive>
    
</content>
```
Now we can add our files in the appropriate manner.

```
<content>
<!-- This is where we will be adding all of our mod files and installation info for OpenIV's eyes only -->

    <archive path="x64e.rpf" createIfNotExist="True" type="RPF7">
      <archive path="levels\gta5\vehicles.rpf" createIfNotExist="True" type="RPF7">
        <add source="adder/vehicles/adder.yft">adder.yft</add>
        <add source="adder/vehicles/adder.ytd">adder.ytd</add>
        <add source="adder/vehicles/adder_hi.yft">adder_hi.yft</add>

<!-- Add a file (replace the stock file). source will be the location of your mod files in relation to the content directory, where the file name between the "> <" will be the target file you are replacing in the current directory relative to the archive you are in (x64e.rpf\levels\gta5\vehcicles.rpf) is the location that i'm saving the above mod files as per the readme. -->

      </archive>
    </archive>    

</content>
```
If all you need or want to do than that's all folks. If you need to install an addon vehicle, and edit ```.meta``` + ```.xml``` files, the ride is just beginning (just kidding, we're almost done)

## Step Five
### Editing ```.xml``` and ```.meta``` files
For my first mod, I will need to edit two files; ```vehicles.meta``` and ```handling.meta``` [If you are looking specifically to add your add on car to the ```dlclist.xml```, then feel free to click here for a shortcut.](#dlclist)

Back to ```assembly.xml```; we will need to open up ```update.rpf``` and write the code to open and edit the ```vehicles.meta``` file as mentioned in the mod's readme file. 
To quote Mister Brooks in his mod's readme file; "now search for ```<PovCameraOffset x="0.000000" y="-0.190000" z="0.630000" />```
replace it with this ``` <PovCameraOffset x="0.000000" y="-0.190000" z="0.570000" />```"
We will accomplish this by writing:
```
	<content>
<!-- This is where we will be adding all of our mod files and installation info for OpenIV's eyes only -->

    <archive path="x64e.rpf" createIfNotExist="True" type="RPF7">
      <archive path="levels\gta5\vehicles.rpf" createIfNotExist="True" type="RPF7">
        <add source="adder/vehicles/adder.yft">adder.yft</add>
        <add source="adder/vehicles/adder.ytd">adder.ytd</add>
        <add source="adder/vehicles/adder_hi.yft">adder_hi.yft</add>
      </archive>
    </archive>
    
<!-- STEP FIVE BELOW-->
    <archive path="update\update.rpf" createIfNotExist="True" type="RPF7">
      <xml path="common/data/levels/gta5/vehicles.meta">
        <!--VEHICLES.META-->
        <replace xpath='/CVehicleModelInfo__InitDataList/InitDatas/Item[modelName="adder"]/PovCameraOffset'>
          <PovCameraOffset x="0.000000" y="-0.190000" z="0.570000" />
        </replace>
        <!--END VEHICLES.META-->
      </xml>
    </archive>
	</content>
```

You may notice my usage of ```[modelName="adder"]``` in the ```xpath``` property. I am selecting the adder item using ```xpath```, [more information is here](https://devhints.io/xpath)


After we've changed the ```PovCameraOffset```, all that's left to do is modify the ```handling.meta``` file. This time, the readme is telling me to replace the entire adder entry in ```handling.meta```. I can accomplish this by writing the following:
```
<content>

    <!-- This is where we will be adding all of our mod files and installation info for OpenIV's eyes only -->

    <archive path="x64e.rpf" createIfNotExist="True" type="RPF7">
      <archive path="levels\gta5\vehicles.rpf" createIfNotExist="True" type="RPF7">
        <add source="adder/vehicles/adder.yft">adder.yft</add>
        <add source="adder/vehicles/adder.ytd">adder.ytd</add>
        <add source="adder/vehicles/adder_hi.yft">adder_hi.yft</add>
      </archive>
    </archive>


    <!-- STEP FIVE BELOW-->

    <archive path="update/update.rpf" createIfNotExist="True" type="RPF7">
      <!--UPDATE.RPF-->
      
      <xml path="common/data/levels/gta5/handling.meta">
        <!--VEHICLES.META-->
        <replace xpath='//Item[handlingName="ADDER"]'>
          <Item type="CHandlingData">
          <handlingName>ADDER</handlingName>
          <fMass value="1800.000000" />
          <fInitialDragCoeff value="7.800000" />
          <fPercentSubmerged value="85.000000" />
          <vecCentreOfMassOffset x="0.000000" y="0.000000" z="0.000000" />
          <vecInertiaMultiplier x="1.000000" y="1.300000" z="1.500000" />
          <fDriveBiasFront value="0.200000" />
          <nInitialDriveGears value="6" />
          <fInitialDriveForce value="0.700000" />
          <fDriveInertia value="0.650000" />
          <fClutchChangeRateScaleUpShift value="6.000000" />
          <fClutchChangeRateScaleDownShift value="6.000000" />
          <fInitialDriveMaxFlatVel value="300.000000" />
          <fBrakeForce value="2.500000" />
          <fBrakeBiasFront value="0.450000" />
          <fHandBrakeForce value="0.700000" />
          <fSteeringLock value="42.000000" />
          <fTractionCurveMax value="2.500000" />
          <fTractionCurveMin value="2.380000" />
          <fTractionCurveLateral value="22.500000" />
          <fTractionSpringDeltaMax value="0.150000" />
          <fLowSpeedTractionLossMult value="1.500000" />
          <fCamberStiffnesss value="0.000000" />
          <fTractionBiasFront value="0.485000" />
          <fTractionLossMult value="1.000000" />
          <fSuspensionForce value="2.860000" />
          <fSuspensionCompDamp value="1.400000" />
          <fSuspensionReboundDamp value="2.100000" />
          <fSuspensionUpperLimit value="0.120000" />
          <fSuspensionLowerLimit value="-0.100000" />
          <fSuspensionRaise value="0.000000" />
          <fSuspensionBiasFront value="0.500000" />
          <fAntiRollBarForce value="0.900000" />
          <fAntiRollBarBiasFront value="0.600000" />
          <fRollCentreHeightFront value="0.410000" />
          <fRollCentreHeightRear value="0.410000" />
          <fCollisionDamageMult value="0.700000" />
          <fWeaponDamageMult value="1.000000" />
          <fDeformationDamageMult value="0.700000" />
          <fEngineDamageMult value="1.500000" />
          <fPetrolTankVolume value="65.000000" />
          <fOilVolume value="5.000000" />
          <fSeatOffsetDistX value="0.000000" />
          <fSeatOffsetDistY value="0.000000" />
          <fSeatOffsetDistZ value="0.000000" />
          <nMonetaryValue value="80000" />
          <strModelFlags>440010</strModelFlags>
          <strHandlingFlags>0</strHandlingFlags>
          <strDamageFlags>0</strDamageFlags>
          <AIHandling>AVERAGE</AIHandling>
          <SubHandlingData>
            <Item type="CCarHandlingData">
              <fBackEndPopUpCarImpulseMult value="0.075000" />
              <fBackEndPopUpBuildingImpulseMult value="0.030000" />
              <fBackEndPopUpMaxDeltaSpeed value="0.250000" />
            </Item>
            <Item type="NULL" />
            <Item type="NULL" />
          </SubHandlingData>
          </Item>
        </replace>
        <!--END VEHICLES-->
      </xml>
      
      <xml path="common/data/levels/gta5/vehicles.meta">
        <!--VEHICLES.META-->
        <replace xpath='/CVehicleModelInfo__InitDataList/InitDatas/Item[modelName="adder"]/PovCameraOffset'>
          <PovCameraOffset x="0.000000" y="-0.190000" z="0.570000" />
        </replace>
        <!--END VEHICLES-->
      </xml>
      <!--END UPDATE.RPF-->
    </archive>
</content>
```

# [Adding on vehicles](#dlclist)
## ```dlclist.xml``` and files

Let's start with the files for installation. In this mod, I need to copy my ```dlc.rpf``` file from ```Install>content>r8ppi>``` to ```GTAV>mods>update>x64>dlcpacks>r8ppi```

This can be done simply:
```
<content>
<add source="r8ppi/rpf">update/x64/dlcpacks/r8ppi/dlc.rpf</add>
</content>
```

Now we need to add the required data to ```dlclist.xml``` and ```extratitleupdatedata.meta```:
First, for ```dlclist.xml```
```
<content>

<add source="r8ppi/dlc.rpf">update/x64/dlcpacks/r8ppi/dlc.rpf</add>

    <archive path="update/update.rpf" createIfNotExist="True" type="RPF7">
      <xml path="common/data/dlclist.xml">
        <add xpath="SMandatoryPacksData/Paths">
          <item>dlcpacks:\r8ppi\</item>
        </add>
      </xml>
    </archive>
</content>
```
Now, for ```extratitleupdatedata.meta```
```
<content>

<add source="r8ppi/dlc.rpf">update/x64/dlcpacks/r8ppi/dlc.rpf</add>

    <archive path="update/update.rpf" createIfNotExist="True" type="RPF7">
      <xml path="common/data/dlclist.xml">
        <add xpath="SMandatoryPacksData/Paths">
          <item>dlcpacks:\r8ppi\</item>
        </add>
      </xml>
      <xml path ="common/data/extratitleupdatedata.meta">
        <add xpath="SExtraTitleUpdateData/Mounts">
          <Item type="SExtraTitleUpdateMount">
            <deviceName>dlc_r8ppi:/</deviceName>
            <path>update:/dlc_patch/r8ppi/</path>
          </Item>
        </add>
      </xml>
    </archive>
</content>
```
# Making the actual ```.oiv``` file
It's simple: Compress your main folder (containing ```assembly.xml```, icon and content) into ```.zip``` format and rename the ```.zip``` to ```.oiv```


# That's it!
If you would like more information, I've included some links below, including the entire tutorial on GitHub along with links the the mods used in the example.
## If you want me to create a ```.oiv``` for your mod
Please PM me, or email me [ckacmaster@protonmail.com](mailto:ckacmaster@protonmail.com)

## Links
[Tutorial on GitHub](https://github.com/Ckacmaster/OIV-Tutorial)
[Audi R8 Mod by le_AK](https://www.gta5-mods.com/vehicles/2013-audi-r8-v10-ppi-razor-tuning-add-on)
[Bugatti Veyron by Mister Brooks](https://www.gta5-mods.com/vehicles/bugatti-veyron-grand-sport)
