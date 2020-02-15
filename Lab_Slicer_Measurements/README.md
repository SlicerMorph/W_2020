# Lab Slicer #4: Measurements and Visualization 
## Overview of markups
* Stable version of Slicer only supports fiducial markups 
* Slicer Nightly and SlicerMorph versions add: lines, angles, ROIs, open and closed curves, and improved handling of large numbers of markups. 
<img src="./images/MarkupWidgets.png">

* Updates to Markups module are ongoing so check back for updates
## Markup Types
**ROI:**
Place two points sequentially that specify corners of a rectangular cube defining the region of interest. The size and shape of the rectangle can be adjusted after placement.

**Fiducials:**
Place a single landmark point.

**Lines:**
Sequentially place two points, creating a line between them.

**Angles:**
Place three points sequentially. This forms two vectors where the second point placed is the vertex. The angle beween the two vectors is displayed.

**Open and closed curves:**
Sequentially place points. A curve will be fit to the points and updated as additional points are added. If the closed curve is selected, the first and last points placed will be connected.

<img src="./images/MarkupTypes.png">

## Markup Placement
  * Slicer has two mouse modes: Transform, and Place. 
  * Transform mode is the default interaction mode. This mode allows interaction with loaded data (pan, zoom, rotate)
  * The icons in the mouse mode toolbar at the top of the main GUI allow to switch between these modes
  * Place mode allows to place one object then switches modes back to Transform mode. Fiducial is the default object.
  * If there is no active Markup node, one will be created with the first placement. Curve points and fiducials will be added to the active Markup node, if one exists. 
  * Place mode can be made persistent by clicking the checkbox in the mouse mode toolbar.
<img src="./images/FiducialPersistence.png">

## Markup Management
Fiducial points and anchor points of lines, curves, and angles can be accessed and manipulated using the `Markups` module. 
<img src="./images/markupsModule1.png">
* In the Create menu, a new node Markups node can be created for fiducials, lines, angles, and curves.
* In the Display menu, set the visibility, opacity, glyph and text size of a markup node. Expand the Advanced tab for additional options.
* In the Control Points menu, use the table to adjust visibility, labels, and position of individual fiducials or anchor points
<img src="./images/markupsModule2.png">

## Example 1: Using Markups for Measurement
In this example, we will place a closed curve on one slice of a CT scan, measure the area of the curve, and visualize the region. For more detail and discussion, see the Slicer discourse thread [here](https://discourse.slicer.org/t/how-can-i-calculate-an-area-on-a-ct-image-i-can-calculate-volumes-mm-3-but-not-areas-mm-2/1549/7).

1. Select the `Sample Data` module and load the MRIHead volume. 
 <img src="./images/sampleData.png">

2. Select the closed curve markup mode and place a curve around the brain tissue in the red view window (axial slice). You can change the Slicer layout to red window only for better detail.
<img src="./images/CurveOnRed.png">

3. Open the Python Interactor and paste the following snippet of code to calculate the area of the curve:
```
curve=slicer.util.getNodesByClass("vtkMRMLMarkupsClosedCurveNode")[0]
crossSectionSurface = vtk.vtkPolyData()
areaMm2 = slicer.modules.markups.logic().GetClosedCurveSurfaceArea(curve, crossSectionSurface)
print("Curve {0}: surface area = {1:.2f} mm2".format(curve.GetName(), areaMm2))
```
<img src="./images/pythonInteract.png">


4. If you switched to the red slice view layout in the Slicer application, switch back to the conventional layout. To visualize the area of the curve, type the following code snippet in the Python Interactor:
```
crossSectionSurfaceModel = slicer.modules.models.logic().AddModel(crossSectionSurface)
crossSectionSurfaceModel.SetName("{0} surface".format(curve.GetName()))
crossSectionSurfaceModel.CreateDefaultDisplayNodes()
crossSectionSurfaceModel.GetDisplayNode().BackfaceCullingOff()
crossSectionSurfaceModel.GetDisplayNode().SetColor(curve.GetDisplayNode().GetColor())
crossSectionSurfaceModel.GetDisplayNode().SetOpacity(0.5)
crossSectionSurfaceModel.SetDescription("Area[mm2] = {0:.2f}".format(areaMm2))
```
 <img src="./images/VisualizingCurveArea.png">
 
## Example 2: Using the `Line Profile` module
In this example, we will use the `Line profile` module to place a line and examine the intensities of a volume along the line.

1. Check that the MRHead volume from example 1 is loaded in the scene.

2. Select the line markup mode and place a line along an area of interest in one of the slice views. This could also be done using an open or closed curve.

3. Select the `Line Profile` module. Choose MRHead as the input volume, the line you created (by default named **L**) as the input line and choose the options to create a new output table and plot series. If needed, you can adjust the number of samples along the line using the Line resolution slider. Select the **Compute intensity profile** button and a line plot of the intensity volume intensity values sampled along the line. From the `Data` module you can also view the results as a table by clicking the eyeball next to the name of the table node created.

<img src="./images/lineProfile.png">

## Visualization: Displaying Mesh Data
Mesh data in Slicer is displayed using the `Models` Module. It can not be rendered using the `Volume Render` Module. Fiducial points are automatically placed on the surface of the a loaded mesh and will be constrained to the surface when they are moved along it. The control points for other markups are also constrained to mesh surfaces when present, but the interpolated . 

## Example 3: Displaying a Mesh and resampling a curve on the surface
1. Load the Gorilla Skull Reference Model under the SlicerMorph tab of the `Sample Data` module (you will need SLicerMorph installed to see this option in the menu).

<img src="./images/sampleDataGorilla.png">

2.Center the dataset in the 3D viewing window using the button at the top left of the window. Optionally, change to the 3D only layout.

<img src="./images/centerGorilla.png">

3. Open the `Models` module. Experiment with changing the color and opacity of the skull.
<img src="./images/Models.png">

4. Select open curve placement mode from the upper menu bar and place a curve between landmarks **35** and **42** using approximately 10 points. Note that the control points are snapped to the mesh, but the curve itself may lie above or below the mesh surface. In the nem=
<img src="./images/curveOnMesh.png">

5. Open the `Markups` module. Expand the Resample Menu. Select **Create a new markups curve** from the Output node selector and set the Number of resampled points to 50. In the **Constrain points to surface** menu, select the loaded gorilla mesh. Before resampling, confirm that the curve being resampled is selected as the active node in the `Markups` table. 
<img src="./images/resampleOptions.png">

Click the **Resample curve** button to generate a new open curve with 50 points constrained to the mesh surface. This results in a curve that is closer to the actual surface curvature. Note any difference in length between the original and resampled curves in the `Markups` table.
<img src="./images/newCurve.png">

## Visualization: Volume Rendering
The `Volume Rendering` module provides interactive visualization of 3D image data. For full documentation of the panel and functions, see [here](https://www.slicer.org/wiki/Documentation/Nightly/Modules/VolumeRendering#Panels_and_their_use).
* Only scalar volumes can be used for volume rendering. Vector volumes (eg jpg, png, bmp, or other classic 2D formats) can be converted to scalar volumes using the [VectorToScalarVolume module](https://www.slicer.org/wiki/Documentation/Nightly/Modules/VectorToScalarVolume).
* 3D Slicer uses volume ray casting to computes 2D images from 3D volumetric data sets. Unlike surface reconstruction, there is no estimation of object surfaces or segmentation.
* The values displayed are calculated using a transfer function that incorporates voxel intensities, material properties, and illumination.
* The opacity and color of the image can be adjusted by modifying their transfer functions in the `Volume Rendering` module.

 <img src="./images/volumeRenderTF.png">
 
* Slicer supports both CPU and GPU volume rendering. CPU based will always work, whether you are on a computer without a dedicated graphics card, or on a remote connection (which may not support hardware accelerated graphics), but it is slow. GPU requires you have a dedicated graphics card with 1GB or more videoRAM, but it is much faster. 
* If you have a dedicated graphics card, you may want to set the default visualization method to GPU rendering using the menu option in: Edit->Preferences 
* Always set the rendering quality to normal 
* The physical limits to the size of the volumes that can be rendered are determined by the graphics card RAM and MAX_3D_TEXTURE_SIZE. Every dimension of the image must be less than the value of the MAX_3D_TEXTURE_SIZE and the full dataset must fit into GPU’s RAM. For the full discussion on these limits, see the Slicer discourse thread [here](https://discourse.slicer.org/t/what-spec-gpu-is-required-for-gpu-volumentric-rendering/1596).
* Driver issues: To configure laptops with two GPUS see [this discussion](https://discourse.slicer.org/t/can-i-choose-which-gpu-to-use/3149)
* Crop 3D view vs Crop Volume confusion


## Example: Volume Rendering 
1. Load the MRIHead volume from the `Sample Data` module.
2. Open the `Volume Rendering` module. In the **Volume** field, make sure the volume MRHead is selected. Click the eyeball next to the **Volume** field to display the image. You can change the 3D Slicer layout to 3D only.

<img src="./images/initialDisplay.png">

3. Expand the **Advanced** tab to view the opacity and color transfer functions. You can click on these functions to move or add additional control points.
<img src="./images/initialTF.png">

4. Under the **Display** tab, click on the **Select a Preset** menu. This menu contains saved transfer functions that work well for common data types. Select **MRI Default** (row 4, column 5). Try adjusting the color and opacity functions of this suggested display setting.
<img src="./images/colorPreset.png">

## Example: SlicerAnimator 
The `Animator` module helps create and export animations in mp4 or GIF format. The animations are created by visualizing a volume and adjusting the rotation, ROI cropping, and rendering properties. A demo video of this module is also available [here](https://youtu.be/9GBekYcJR4E) .

1. Load the MRIHead volume from the `Sample Data` module.

2. Open the `Volume Rendering` module. In the **Volume** field, make sure the volume MRHead is selected. Click the eyeball next to the **Volume** field to display the image. Under the Display Menu, adjust the Shift sliderbar to optimize 3D visibility.

2. Open the `Animator`  module. In the Animation Parameters dropdown menu, select the option to create a new animation. 
<img src="./images/animatorModule.png">

3. Select the **Add Action** button and choose **CameraRotationAction** from the menu. A CameraRotation action will be added to the Action menu. The properties of the rotation can be adjusted using the **Edit** button. To preview the animation, select the play button. 
<img src="./images/addCamera.png">

4. Select the **Add Action** button and choose **ROIAction** from the menu. Two ROI markups will be placed in the scene. The first ROI will be used to crop the region displayed at the start of the animation and the second will be used to crop the region displayed at the end of the animation. To adjust the placement of the ROIs, switch to the `Data` module and turn the visibility of the ROI markups on. Place the Start ROI around the whole head. Place the second ROI inside the brain. Return to the `Animator` module to preview the effect of this action. 
<img src="./images/selectROI.png">

5. Select the **Add Action** button and choose **VolumePropertyAction**. The volume property action allows your animation to transition from one set of rendering properties to a second set. To use this effect, you will need to create three sets of volume properties. The first will control the rendering at the beginning of the animation, the second will control the rendering at the end of the animation, and the third is an arbitrary set of properties that will be used to store the volume's display properties as they are updated. To create the properties, click the **Edit** button next to the volume property action that you added to the Actions list. Note that three property sets are created  named **Start VolumeProperty**, **End VolumeProperty**, and **VolumeProperty**. 

6. Open the `Volume Rendering` module and expand the **Inputs** menu. In the Property drop-down menu, select **Start VolumeProperty**. The current rendering will be displayed at the start of the animation. Adjust the color mapping and opacity. 
<img src="./images/startProperty.png">

You can repeat this process for the **End VolumeProperty**, but in this example we will use preset display properties for the final animation rendering. In the display menu, click the **Select a Preset** button and choose **MR-default**. This will display the volume as it will appear at the end of the aniamtion. Before leaving  the `Volume Rendering` module, return to the property menu and select **VolumeProperty**. It is critical that this is the final active property selected when leaving the `Volume Rendering` module.
<img src="./images/endProperty.png">

7. Open the `Animator` module. Click the **Edit** button next to the Volume Property action in the Actions menu. Select the Start VolumeProperty as **Start VolumeProperty**, End VolumeProperty as **MR-default** and the Animated VolumeProperty as **VolumeProperty**. Click the **OK** button and preview the animation using the play button.
<img src="./images/volumeProperty.png">

8. When you are done adjusting the animation parameters, Select an output file location in the Export menu and click the **Export** button to save your animation.
