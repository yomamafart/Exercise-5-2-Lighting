# Exercise 5.2-Lighting

Exercise for MSCH-C220

The purpose of this exercise is for you to explore 3D lighting and camera movement in Godot. There are many steps, and some of the concepts are a little complicated, so I would recommend that you stop and test frequently. I will try to be as clear as possible.

This exercise is loosely based on the How to light a 3d scene in Godot (3d tutorial) by GDQuest. The YouTube video is [available here](https://www.youtube.com/watch?v=iamttSmxA2I).

This exercise is different from the GDQuest tutorial in distinct ways, so please follow the directions I provide. The video may help you to visualize anything you might find confusing, however.

--- 

Fork this repository. When that process has completed, make sure that the top of the repository reads [your username]/Exercise-5-2-Lighting. Edit the LICENSE and replace BL-MSCH-C220 with your full name. Commit your changes.

Clone the repository to a Local Path on your computer.

Open Godot. Import the project.godot file and open the "Lighting" project.

In World.tscn, I have provided a starting place for the exercise: the scene contains a parent Spatial node (named World), a Marker3D node, and a Node3D node (named Pivot). I have also included several files in the Assets folder, which we will use during this exercise.

res://Cabin.tscn contains a [3D model of a small cabin](https://sketchfab.com/3d-models/gr-5df64141235040749103749123e43010) created by luyssport and released under a CC Attribution-NonCommerical-ShareAlike license. The original model is in the Assets folder (scene.gltf), but I have gone through the tedious process of preparing the materials to respond to lighting. Feel free to explore the original model if you wish.

Right-click on the Marker3D node in the Scene Panel, and Instance Child Scene. Select res://Cabin.tscn.

Select the Marker3D node, and in the Inspector, change Node3D->Transform->Position.y = 6

Right click on the Pivot node and Add Child Node. Choose Camera3D. Rename it Camera. Select the Camera node and in the Inspector, check the box next to Current. In Node3D->Transform. Change Position.y = 6 and Position.z = 5

With the Camera selected, you should now see a checkbox (labeled Preview) in the top left corner of the Viewport. If you check that box, you should see a fairly-close view of the right side of the house. Test it, and then leave the preview unchecked.

Godot, by default, creates a procedurally-generated skybox for 3D games with a hazy, cloudy sky (which reflects on the meshes in the scene. The next step will be to replace this with a custom skybox and allow Godot to illuminate the cabin with indirect lighting.

Right click on the World node and Add Child Node. Select VoxelGI. This will provide some constraints to Godot for precomputing the global illumination. Select the VoxelGI node, and in the inspector, set the Size to x=30, y=30, z=30. Select VoxelGI->Data and choose New VoxelGIData. In the Viewport, you should see a button labeled Bake VoxelGI. Press that now. Now, we would prefer to not have to look at the green cube, so we will hide it in the editor. To the left of the Bake VoxelGI button is one labeled View. If you press that, you should see a Gizmos submenu. Select the eye next to VoxelGI (so the eye is now closed).

Right click on the World node and Add Child Node. Select WorldEnvironment. Drag it to the top of the Scene panel, so it appears immediately under World. In the Inspector, select Environment and choose New Environment. After the new Environment has been created, select Environment again and Save it as res://Environment.tres. Then select Environment once more and choose Edit.

Under Background->Mode, select Sky, and under Background->Sky, select New Sky. Edit the Sky and under Sky Material, select new PanoramaSkyMaterial. Edit the PanoramaSky, and drag the res://Assets/venice_sunset_1k.hd file (acquired [from HDRIHaven](https://hdrihaven.com/hdri/?h=venice_sunset)) into its Panorama. The skybox should now appear as a boardwalk at sunset in Venice. Go back to editing the Environment. 

In Ambient Light->Source select Sky, and in Ambient Light->Color, type in R=95, G=33, B=217, A=255, set Ambient Light->Energy=0.2, and set Ambient Light->Sky Contribution=0.5

We actually only want the colors of the skybox reflecting on our model, so in Background->Mode, instead select Custom Color. In Background->Color, type in R=68, G=68, B=68, A=255.

Set ToneMap->Mode to Aces and Ss Reflections->Enabled to On. There is a thorough discussion of ToneMap in the GDQuest video—it adjusts how the exposure and white balance are calculated. SS reflections allows metallic surfaces to be reflective.

We will now experiment with two different types of light sources. Back in the Scene panel, right-click on the World node and Add Child Node. Select DirectionalLight3D. In the Inspector, change Node3D->Transform->Position: x=-15, y=10, z=20. Node3D->Transform->Rotation Degrees: x=-10, y=-40, z=-25. The arrow should be aligned with where the sun was in the Venice Sunset panorama (indicating where the directional light is coming from). To make it look more like a sunset, change Light->Light->Color: R=235, G=104, B=8, and change Light->Light->Energy=0.6

Right-click on the World node and Add Child Node. Select OmniLight3D. In the Inspector, change Node3D->Transform->Position: x=1.711, y=4.333, z=0.268. Change Light->Light->Color: R=197, G=188, B=109. Change OmniLight3D->Omni->Attenuation=2 (drag down the line)

Finally, we will add a little dynamism to the camera. Right-click on the Pivot node, and Attach Script. Set the path to res://Pivot.gd with an Empty Template.

In the resulting Pivot.gd script, type the following:

```
extends Node3D

@export var speed = 15
@export var zoom_speed = 1.5
@export var zoom_min = 5
@export var zoom_max = 15

func _physics_process(delta):
	rotation_degrees.y += delta*speed
	$Camera.position.z += delta*zoom_speed
	if $Camera.position.z > zoom_max or $Camera.position.z < zoom_min:
		zoom_speed *= -1
```

Run the scene. The result should look something like this: 

![Cabin example](https://github.com/BL-MSCH-C220/Exercise-5-2-Lighting/blob/main/cabin.gif)

Quit Godot. In GitHub desktop, add a summary message, commit your changes and push them back to GitHub. If you return to and refresh your GitHub repository page, you should now see your updated files with the time when they were changed.

Now edit the README.md file. When you have finished editing, commit your changes, and then turn in the URL of the main repository page (https://github.com/[username]/Exercise-5-2-Lighting) on Canvas.

The final state of the file should be as follows (replacing the "Created by" information with your name):
```
# Exercise 5.2-Lighting

Exercise for MSCH-C220

An exploration of 3D lighting and camera movement in Godot

## Implementation

Built using Godot 4.1.1

## References

[How to light a 3d scene in Godot (3d tutorial)](https://www.youtube.com/watch?v=iamttSmxA2I)

The model is [gr](https://sketchfab.com/3d-models/gr-5df64141235040749103749123e43010) created by luyssport and released under a CC Attribution-NonCommercial-ShareAlike license.

The HDRI Sky Box (Venice Sunset) was downloaded from [HDRIHaven](https://hdrihaven.com/hdri/?h=venice_sunset).

## Future Development

None

## Extra Credit

None

## Created by 

Jason Francis
```
