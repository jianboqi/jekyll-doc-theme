---
title: Documentation
permalink: /docs/documentation/
redirect_from: /docs/index.html
---

{{% toc %}}

# Installation

Download the installer from here [<a href="/projects/less/lessRelease/LESS-1.0.exe.zip" onClick="ga('send', 'event', { eventCategory: 'LESSDownload', eventAction: 'directdownload', eventLabel: 'LESSWin64bit'});">LESS1.0 (win64bit)</a>]. All you need is just to choose a place to install it.
(Usually, LESS can not be installed in c:\program files, since it does not have administrative rights.). After installation, LESS will be automatically started. If successful, it should be like this:

<center><img src="http://7xawu0.com1.z0.glb.clouddn.com/startscreen.png"/></center>

As you can see, there are mainly three panels in its main screen. Panel 1 is for displaying some information, such as view and sun azimuth angle, sensor footprint and tree positions. This will make it easier for user to set parameters, because this display is automatically updated when related parameters are changing. 

Panel 2 is the main place to set parameters. It has 7 subpages, each corresponds to different parameters (they will be explained later).

Panel3 is used to display some console information.

# Parameters


## Sensor

* **Type**: Up to now, we can set it as orthographic. In the near future, we can choose perspective.
* **Width/Height**: the size of the final image (pixels)
* **Samples**: sample count per squaremeter. Usually 128 is sufficient. If you increase to 256, 512, the quality of the simulated image will be better.
* **Spectral Bands**： It represents the which band you want to simulate. For example, RED and NIR: 660nm，900nm. Now, you must also input a bandwidth for each band with the format of `centerband:bandwith`, e.g. 660:10,900:10. These bandwidths will be used for determining the irradiance. If you have no special requirement, it can just set the bandwidth to zero. And also, we provide a tool to define the bands. <br/>
<center><img width="400" src="http://7xawu0.com1.z0.glb.clouddn.com/bandwidth.png"/></center>
* **Only First Order?**: If it is true, it means LESS only simulate the first-scattering event (sun * radiation reflected by objects only one time). If set to false, then both first-scattering and multi-scatteing will be simulated.
* **Width Extent/Height Extent**: The actual extend of the orthographic sensor can simulate. It is similar to FOV of perspective sensor.

## Observation
Currently, you can only input one direction at the same time：

* **View Zenith**: view zenith angle (angle between vertical direction)
* **View Azimuth**: view azimuth（0° is north， clock-wise to 360°). 

If you change this two values, the display panel will also changed according to the parameters you set.
<center><img src="http://7xawu0.com1.z0.glb.clouddn.com/obs.png"/></center>

(Note: the multiangle mode will also to integrated into the GUI in the near future.)

## Illumination
there are two groups of illuminations, one is from sun, other is from atmosphere:

Sun Zenith: zenith angle of sun
Sun Azimuth: azimuth angle of sun（0° is north， clock-wise to 360°）. This defines the position of sun. That means if you set it as 90° (East), then it will produce shadows in West.

Under Sky, there are two options:

* **Type**：It represents the type of atmophere radiation, now only one option can be used-"SKY_TO_TOTAL"，it means the ratio between diffuse radiaton and total incident radiation. Thus (1-SKYL)*T (T is sun radiation above atmosphere) is the sun radiation under atmosphere. Under this mode, atmosphere is isotropic diffuse radiation from upper hemisphere. 
* **Percentage**：it define the actual ratio between atmosphere radiation and total radiation. What should be noticed is that when you input the this value, the number of values should be equal to the the number of bands （under the sensor section). That is, for each band, it may have different values.


## Optical Database
Since objects in LESS are represented as triganles meshes, thus for each triangle, it can have at least three kinds of optical properties: reflectance of front side, reflectance of back side, and transimittance (we assume transmittance of both side is the same). 

<img src="http://7xawu0.com1.z0.glb.clouddn.com/opticaldb.png"/>

In Optical database, we should first define some optical model, and then they can be used in the following terrain or forest definition.
By default, there are three optical model defined, you can delete them, or modified them directly. you can also add new optical model. For each reflectance or transmittance, values of different bands are connected by comma.

## Terrain

there are mainly three types of terrain: `PLANE`, `RASTER`, `MESH`. `PLANE` just represents simple plane lies at altitude of 0. At the same time, the **XSize** and **YSize** should be given. `RASTER` means LESS uses a raster file to represent terrain. When select this, a **Terrain File** should be specified. The format of this raster file is "ENVI standard" or "Geotiff". It should be noted that you can also input "XSize" and "YSize", if they are not consistent with the actual extents of terrain image, the final terrain will be tretched. Thus, it is important to input correct Sizes.
The last option `MESH` means you can input a terrain which is already described by obj file. At this time, the you can not input the size. the extents is exacly the same with obj file.

For **Optical Property**, it lists a the available optical models which are defined in **Optical Database**. If you define a new one, it will automatically appear here. you simplely choose it.
<img src="http://7xawu0.com1.z0.glb.clouddn.com/plane.png"/>

<img src="http://7xawu0.com1.z0.glb.clouddn.com/terrain.png"/>

## Objects

Objects define what you want to put in the scene. For example, forest is formed by a number of single trees, which are describe by triangle mesh (obj file) in LESS. Usually, we can not input a obj for each single trees, since forest contains a lot of trees and  the tree itself contains numerous triangles, it maynot be possible even for large memory computers. The alternative is to use a "instance" technique. That means we define a single tree, and we can place it at different places, just using reference, thus the program only keeps one copy of the triangle mesh, but it represents trees at different places (they have exactly the same structures, but we can do rigid transformations). 

Thus, the first step is to define some objects (single tree). you can click `[Define Object]` button, it will show a second window, which allows you to define the objects.

<img src="http://7xawu0.com1.z0.glb.clouddn.com/object.png"/>

1. First of all, we give a name for our firt object, such as "birch", after click `[Add]` button, "birch" will appear in the Objects list, we select it, then the button `[Import OBJ]` is activated. This is used for selecting component of the object "birch". Component is a part of the object, e.g. a tree contains leaves and branches (usually they have different optical properties). click '[Import OBJ]', you can choose a obj file. then the name of the obj file will appear in the "Components" list. **You should be aware that the obj file itself will be copied to the simulation folder, so you can safely delete it from your original place.**. Then select one of the component, the `[Optical Property]` is activated, you can choose a optical model for the selected component. These optical models are also from "Optical Database". If you find you need to define a new optical model, you can go back to "Optical Database" page without closing the current window. When you finish, the optical models are automatically synchronized. **All the component must be given a optical property**. After all this, you can click "OK".
<img src="http://7xawu0.com1.z0.glb.clouddn.com/object1.png"/>

2. when object is ready, we come back to `Objects` page, now we can see that the defined objects are shown in Objects list, if we select it, we can define its positions, each time we input a coordinates for the object, its position will appear on the "Display" panel, which will help you to check whether it is correct.
<img src="http://7xawu0.com1.z0.glb.clouddn.com/forests.png"/>

3. (`New featrues`) Recently, we have implemented some new features to define positions of objects, because it is usually very difficult to input one by one for large areas. Thus, you can use the `Random` tool provided by LESS. Click the button [`Random`], it will open a new window, Now the only choice is poisson distributon (This is very normal for trees in forest). The only parameter you need to provide is the minimum distance between two objects. Click OK, LESS will automatically generate instances of defined objects. The number and position of each object in the Objects list is also ramdom. Thus, you can get a reasonable distribution of objects.
<img src="http://7xawu0.com1.z0.glb.clouddn.com/objectspos.png" />
4. If you just want to allocate objects in some particular areas, the solution is using `Polygon` tool. Check the option of Polygon under the display panel. This time when your mouse 
enters the display panel, it will become a hand. you can create a polygon by left click. If you want to remove the polygon, just use your right click of mouse. After the creation of polygon, you can delete or add object instances in this area. If you click `Add`, then all the generated instances will only apprear in the polygon, the position and number of each object are still random within the polygon. However, if you choose one of the objects in the Objects List. then the generated instances only contains this object. This tool can create some forest scene which contains one species of trees in some area and another species of tree in another areas.
<img src="http://7xawu0.com1.z0.glb.clouddn.com/polygon.png"/>




## Run simulation

When all is ready, click `[Run All]`, it will start to simulation. After that you can click '[Open Results Folder]' under `[Tools]`. it will show the simulated image. Now it is stored as ENVI Standard format. The following iamge shows a simulated image in NIR band. The altitude of trees
are automatically determined by LESS. This scene contains two single tree model, and totally 16 trees. The simulation is only about 5 seconds on a 8-cores labtop.

<img src="http://7xawu0.com1.z0.glb.clouddn.com/spectral__VZ=0_VA=180.jpg"/>

**It should be also noted that even though LESS is mainly for forest simulation, this tools can also be used to simulate other scenes, such as cities. Since the Object Definition is the same, each house may have different components which then have different optical properties. In conclusion, if the objects can be represented as triangle meshes, it may be possible.**

## Next Step
Some features, such as multiangle simulation at the same time and net work simulation, have not been integrated into this GUI version, we are continuing working on it, transforming it from command line to GUI.
