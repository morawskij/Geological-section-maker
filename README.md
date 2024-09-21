# Geological-section-maker

### (To start using the app, scroll down past the tutorial and run the cell below)

#### If the images in this user guide do not display correctly, check if you have downloaded the *App_screenshots* folder and placed it in the same directory as this Notebook

This application was inspired by a Planetary Mapping course at the Chieti-Pescara University, with an intention to create a tool that would provide an alternative to a more manual approach to drawing interpretative geological sections, in which the user would create them by means of pressing buttons and dragging some sliders only. Any bugs discovered while using the app can be reported to jm.morawski@gmail.com. An overview of the main functionalities of the app can be found below:

## Summary of available hotkeys 

#### Placed here for ease of referring to when interacting with the application. To read the actual full user guide, start reading from "Welcome screen"

(see section **About hotkeys** in chapter **Adding and customizing layers** for an explanation)

**E** - add a control point to an active layer, right of the active control point 

**W** - add a control point to an active layer, left of the active control point 

**Arrow keys** - move a control point up, down, to the left or to the right

**A** - change the active control point one to the left

**D** - change the active control point one to the right

**X** - delete the active control point

**Q** - toggle linear/quadratic interpolation mode for the active layer

**C** - change the color of the active layer

**[** - change the active layer by one down

**]** - change the active layer by one up

**H** - hide/reveal the delineation curves of layers, including the highlighted curve of the active layer

**J** - hide/reveal the highlighted curve of the active layer

**K** - hide/reveal the delineation curves of layers, except the highlighted curve of the active layer

--------------------- **EXPERIMENTAL** ------------------------------------

**-** - move active layer down

**=** move active layer up

These last two functions have been implemented as hotkey-triggered only and there are no buttons to call them as their behavior is currently somewhat unstable.

The idea is to change the place of the current layer in the list of layers (puch it below, or abve it's neighbour, in terms of stratigraphic relationships), without changing it's delineating curve. This can obviously change the appearence of the plot significantly, and can be helpful for someone who had already meticulously created their layers, but then realized that one of them is actually more likely, from the geological perspective, to be younger than the one above it (or older than the one below it). This is a desirable feature of the application, but at the moment leads to some erratic behavior, with the plots not looking quite as expected after moving layers around. Use with caution. However, some experimentation seems to suggest that if you go up and down along the whole list of layers using the **]** and **[** hotkeys, after moving your layers around, the plot should again look as expected.

## Welcome screen

<img src="./App_screenshots/Welcome_screen_line1.png" alt="Welcome screen" width=600px>

Before starting to work within the ***Geological section maker***, you need to have a text file with some data extracted from a linear section along a Digital Elevation Model (DEM) of the site of interest. In the uppermost part of the welcome screen you can provide the application with essential information on how to interpret the data file(s) which you will feed to it:

**x data column** - a number of a column within the file in which the abscissa (measured in meters along the section line) values are stored

**y data column** - a number of a column within the file in which the corresponding DEM values are stored

**Header lines** - Number of header lines to be dismissed when parsing the file content

Default values of $6$, $7$ and $1$ correspond to the data format as outputted by the ***qProf*** QGIS plugin. If using a differrent tool, you might need to change them using the dropdown menus.

<img src="./App_screenshots/Welcome_screen_line3.png" alt="Welcome screen" width=700px>

In the third line, there are additional settings determining how your section will appear:

**Section plot width (px)** - Width of your plot in pixels, must be an integer. Default value is such that the second application window would occupy most of a regular HD resolution screen.

**Vertical exagerration** - The factor of vertical exagerration (VE), must be a positive real number. Each time a new data file is loaded, this field will be overwritten by a value computed so as to optimize the space occupied by the plot in the app screen. If a different  value is preferred, you need to type it manually before proceeding to **Draw**. Increasing VE the automatically computed may lead to undesireable appearance of the second application window.

**Section line name** - how it will appear in the title of the plot. Should correspond to the way you name the two end points of the section line on your map.  

<img src="./App_screenshots/Welcome_screen_line2.png" alt="Welcome screen" width=700px>

To load the section topography data, press the **Load profile data file** button. You will be able to pick a file of your choice, which, provided that it is in the correct format, will be read by the program and saved into relevant variables. 

<img src="./App_screenshots/Welcome_screen_draw.png" alt="Welcome screen" width=700px>

Once at least one file is uploaded, you will be able to proceed to the second window by pressing **Draw**. However, there is also a possibility to upload more data files, corresponding to a continuation of the same section. It may be necessary if your section line passed through multiple DEMs. The suggested value of vertical exagerration will be updated each time, adapting to the size of the abscissa range of the section. 

<img src="./App_screenshots/Welcome_screen_load.png" alt="Welcome screen" width=700px>

The **Load saved progress** button can be used to resume working on a section by calling a text file in which all the necessary information was saved when previously working with ***Geological section maker***. When loading saved progress, do not change any of the other settings, but rather press this button directly - all the relevant information about your plot will be read directly from the file.  

## Drawing window


<img src="./App_screenshots/Second_window.png" alt="Drawing window" width=1000px>

Once some data is uploaded and the button **Draw** is pressed, you will be moved to another workspace which should look more or less like this. A plot of the topography along the section is provided, together with a dashed line indicating a linear interpolation which fills the gaps in places where the data is missing. Notice that in this example a section came from two separate data files probing two different DEMs and there was a gap between them - hemce a long dashed segment around $20000$ m. 

From here, you can start adding layers into your section with the **Add layer** button. However, if you realise that you forgot to change the VE, plot width, section name or upload an additional data file, you can press the **↵** button instead. It will bring you back to the welcome screen, where you can make the changes necessary, then press **Draw** again. In it's current form the application does not have the ability to remove data files already loaded, therefore, if your plot looks wrong becuase of incorrect input file selection, you will have to start over by shutting down and relaunching the app. Be aware that using the **↵** button is only possible before adding the first layer into your section. If you realize a fundamental setting mistake later, you will also have to start over from the beginning.

### Adding and customizing layers

<img src="./App_screenshots/Second_window_add_layer.png" alt="Drawing window" width=1000px>

The concept of the workflow is to create a stack of layers defined by their delineating upper lines (curves), which are adjusted by means of tweaking their control points. Each layer will have it's own name, color, and optional hatch (e. g. dashed or dotted filling, valid options are |,/\,O,X,o,x,*,+,-,. or any collection of these characters). For any $n>0$, the area shown in the color and hatch of the $n^{th}$ layer on the plot will be the set of points $(x,y)$ such that $l_{n-1}(x)<y<l_n(x)$, where $l_i$ is the upper delineating curve of the $i^{th}$ layer, with $l_0$ referring to the bottom of the plot. Notice that the delineating curve of a layer can go below the base of the plot or the curve delineating the previous layer - in the intervals of the $x$ range where this will be the case, this layer simply won't be shown.     

Any new $n^{th} $layer is added with a single control point, which defines the height of the delineating curve (defaults to halfway between the uppermost point of $l_{n-1}$ and the highest point of the topography along the section). Keeping all layers like this will correspond to an interpretation of horizontal strata. Moving the initial control point to the right corresponds to absecnce of a given fascies left of that point in the section. If finer shapes are desired, more control points can be added.

#### About hotkeys

Some of the workflow is relatively slow when performed with pressing buttons, and an alternative of using **hotkeys** is provided for those functions which need to be used extensively. For example, the addition of a control point to an active layer can be achieved both with an appropriate button and by pressing **L** on the keyboard. Whenever available, the hotkeys will be provided next to the functionalities described below. A full list of hotkeys is also provided in the beginning of this tutorial.

The use of hotkeys requires for all the text input fields to be deactivated, otherwise the keyboard input could be interpreted in abmiguous ways by the computer. If you need to access those fields, press **Change legend name**, **Change hatch**, **Change export file name** or **Change save file name**, depending on the field you want to modify. You will get access to modify the text within the field, but the hotkeys will in turn become deactivated. Disable all text input fields after making desired changes to them in order to be able to use hotkeys again.

#### Adding control points (E/W)

<img src="./App_screenshots/Second_window_add_point.png" alt="Drawing window" width=1000px>

The **+** button in the application creates a new control point:
- If it is a second point created, it will be positioned on the very right of the plot, at the same height as the first point. Modifying it's position will result in a representation of the layer as an inclined stratum of fixed dip angle. The same can be achieved with the hotkey **E**.
- If there are already $2$ points or more, and the current active point is not the rightmost one, a new point will be created halfway between the currently active point and it's nearest neighbour to the right. The same can be achieved with the hotkey **E**.
- If there are already $2$ points or more, and the current active point is the rightmost one, a new point will be created halfway between the currently active point and it's nearest neighbour to the left. The same can be achieved with the hotkey **W**.

Generally, the hotkey **E** creates a control point to the right, and the hotkey **W** to the left of the currenty active point, provided that the active point is not the rightmost, or leftmost, respectively (in which case the pressing of the key is ignored). The button **+** has, except for the one special case, the same effect as the hotkey **E**, whereas there is no general equivalent to the hotkey **W** in the button-based interface.

The newly created point becomes the new active point, highlighted with a red plus (or black, if it is the first or last control point of $l_n$)

#### Moving the control points (arrow keys)

<img src="./App_screenshots/Second_window_move_point.png" alt="Drawing window" width=1000px>

Let us denominate the control points of a curve $l_n$ as $p_{n,1},p_{n,2},..,p_{n,m_n}$, where $m_n$ is the number of currently defined control points of the $n^{th}$ layer's curve. If a position of any such point $p_{a,b}$ is $\left(x_{p_{a,b}},y_{p_{a,b}}\right)$, then a condition is set that $\forall_{n\in\mathbb{N}^+}\forall_{k\in\{1,2,..,m_n\}}x_{p_{n,k-1}}<x_{p_{n,k}}<x_{p_{n,k+1}}$, where $x_p{n,0}$ and $x_p{n,n_m+1}$ correspond to the leftmost and rightmost ends of the section's abscissa range. In other words, the control points remain segragated in terms of increasing abscissa. There are no constraints on the ordinate, other then fitting within the range of the plot. The movement of the points within the constraints can be achieved in two ways:
- For large movement, it is recommended to use the horizontal and vertical sliders highlighted on the screenshot shown above. The setting of the sliders will always be interpreted as a percentage of the range of the given coordinate for the given control point (see contraint on abscissa described above). When the active point changes, the setting of the sliders will always jump to accurately reflect their position within the currently availabe range.
- For fine tuning, use arrow keys to move the point around. It is not advised for larger movements as it will cause significant lags. The arrow key input will be ignored if trying to push a given point outside of it's currently permitted abscissa range.

#### Changing active point (A/D)

<img src="./App_screenshots/Second_window_change_point.png" alt="Drawing window" width=1000px>

The navigation between the control points is done by changing the active point by one to the right or to the left (in the notation from above, it would mean activating the point $p_{n,m'}=p_{n,m\pm1}$, where $n$ is the number of the active layer, and $m$ is the index of the previousl active point (assuming the operation is possible, that is $m'\in\left\{1,2,...,m_n\right\}$. This can be schieved with the two buttons highlighted on the screenshot above, but it is far more efficient to use the hotkeys, **A** to move one point to the left, and **D** to move one point to the right.

#### Removing a point (X)

<img src="./App_screenshots/Second_window_remove_point.png" alt="Drawing window" width=1000px>

By pressing the **-** button or by using the hotkey **X**, you can remove the currently active control point. Cannot be done for the leftmost point of the active layer's control point ($p_{n,1}$).

#### Interpolation (Q)

<img src="./App_screenshots/Second_window_interpolation.png" alt="Drawing window" width=1000px>

For $m_n\geqslant3$, it is possible to apply quadratic interpolation between the control points to achieve a smoother looking curve. To switch between linear and quadratic interpolation, use the radio buttons indicated on the screenshot above, or the hotkey **Q**. If control points are subsequently removed so that only $2$ remain, the application will automatically revert to linear interpolation for this layer.

#### Changing the color of the active layer (C)

<img src="./App_screenshots/Second_window_color.png" alt="Drawing window" width=1000px>

Use the hotkey **C** or press the button showing the current color (see screenshot above), in order to change the color of the layer. The Tkinter Color Chooser window will open, where you can adjust the color by using the sliders or by manually typing the #RRGGBB hex code (you will likely want to copy paste it from your map coloring in QGIS). When modifying the string, make sure it has been properly changed in the input field, sometimes if the string is not modified correctly the change does not take place and you need to start over.

#### Changing the hatch of the active layer

<img src="./App_screenshots/Second_window_hatch.png" alt="Drawing window" width=1000px>

If, instead of compete filling, you want to use a hatch of any sort, such as in the example above, press the **Change hatch** button, which will then turn into a **Fix hatch** button. You can type in any combination of |,/\,O,X,o,x,*,+,-,. characters, such as in the example above. The appearance of the layer will update live as you manipulate the string. Once satisfied, press **Fix hatch** to settle on a choice, deactivate the text input window, and reactivate the availabiltiy of hotkeys.

#### Changing the name of the active layer

<img src="./App_screenshots/Second_window_layer_name.png" alt="Drawing window" width=1000px>

The change of the name with which the layer will appear in the plot, can be achieved analogously to changing the hatch, as shown in the example above.

### Removing layers

<img src="./App_screenshots/Second_window_delete_layer.png" alt="Drawing window" width=1000px>

Press the **Delete** button of a given layer to permanently delete it. Only available when there are multiple layers.

### Changing the active layer ([/])

<img src="./App_screenshots/Second_window_change_active_layer.png" alt="Drawing window" width=1000px>

The active layer will be highlighted in the column indicated on the screenshot above with an inactive **┈➤** button. To change the active layer, press the button with the pencil icon next to the layer you wish to activate.

The application can only display the controls for up to $5$ layers at the same time. If more layers have been created, such as in the example above, you can use the arrow butons shown with the red ellipse on the screenshot to navigate up and down along the layer list.

You can also use the hoteys: **]** to change the active layer by one up, and **[** to change the active layer by one down  

### Layer delineation line appearance on the plot (H/J/K)

<img src="./App_screenshots/Second_window_hide.png" alt="Drawing window" width=1000px>

The delineation curves help you design your section, but you do not want them to be shown on the final output plot. 

To hide/show the delineation curves of all layers (except the highlighted curve of the active layer, which is red and contains the control points), as in the example above, uncheck/check the **Show layer boundaries** box, or use the hotkey **K**.

To hide/show the highlighted curve of the active layer, uncheck/check the **Highlight active layer boundary** box, or use the hotkey **J**.

The hotkey **H** will hide both if either is currently visible, or reveal both if they were both hidden.

### Exporting the plot

<img src="./App_screenshots/Second_window_save_fig.png" alt="Drawing window" width=1000px>

Once happy with the geological section plot, hide layer boundaries and export the plot using the button **Export plot**. Change the name for the output file according to your needs (if always left to default, you will keep overwriting the same file ***test_profile.png***). If you do not provide the extension in the file name input box, *.png* will be assumed by default. 

Sometimes you might want to hide some layers in your plot. For example, maybe you want to create an animation or separate slides in your presentation with different layers highlighted, or sequentially added, so you need dedicated figures for that. To generate such intentionally incomplete plots, uncheck the **Show** boxes next to the layers you want to hide, before exporting the plot, as done for the uppermost $3$ layers in the example above (see the dark arrow on the screenshot). Be sure to change the export file name each time. In principle, you can also hide some layers in this manner during your work on the plot earlier on, but there is probably no reason to ever do so.  

### Saving progress to resume later

<img src="./App_screenshots/Second_window_save_progress.png" alt="Drawing window" width=1000px>

A standard **Ctrl+S** will **NOT** save your progress, but an equivalent can be achieved by saving all the information about the section in it's current shape in a text file, which can be done in the space indicated on the screenshot above in a manner analogous to the export of the plot itself. The generated file can then be read by the application, as explained at the end of the **Welcome screen** section of this manual.
