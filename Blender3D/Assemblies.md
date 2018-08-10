# Steps to aling servos with plastic parts for assembly

### Align pivot on plastic part with joint cylinder

1) Select the joint cylinder with (RMB), go into edit mode (Tab) and select the mesh mode (Ctrl-Tab) to be "Faces". Select the top surface using RMB.

2) Press (Shift-S) to open the snap window and select "Cursor to Selected", then press "A" to deselect face and Tab to exit edit mode.

3)Make the center of the top surface of the joint cylinder the origin for that geometry by pressing (Shift-Ctrl-Alt-C) and selecting "Origin to 3D Cursor". Then press A to de-select all object.

4) Select the top surface of the plastic part pivot by clicking (RMB), going into edit mode (Tab) and Selecting the MEsh mode to be "Face" (Ctrl-Tab) then select "Faces" from Window. Then select the top surface (RMB) and press Shift-S to open the snap window menu and select "Origin to selected". Press A to de-select

````python3
bpy.ops.view3d.snap_cursor_to_selected()
bpy.ops.object.editmode_toggle()
bpy.ops.object.select_all(action='TOGGLE')
````

3) Unhide joint cylinder and snap selected geometry to cursor

````python3
bpy.context.object.hide = False
bpy.ops.view3d.snap_selected_to_cursor(use_offset=False)
bpy.ops.object.select_all(action='TOGGLE')
````

4) Select the opposite top surface of the joint cylinder and place 3D cursor there


5) Select servo, make sure the origin is at the top surface of the pivot, then snap selection to 3D cursor

6) Enable snap tool and snap to surface

7) Select the bottom surface of the servo (the one opposite to the pivot).

8) grab and move on X axis until it snaps to the surface of the plastic part.



````python
bpy.data.objects["Servo 13"].hide = False
bpy.ops.object.select_all(action='TOGGLE')
bpy.ops.object.editmode_toggle()
bpy.ops.object.origin_set(type='ORIGIN_CURSOR')
bpy.ops.view3d.snap_cursor_to_selected()
bpy.ops.object.editmode_toggle()
bpy.ops.view3d.snap_cursor_to_selected()
bpy.ops.object.editmode_toggle()
bpy.ops.object.origin_set(type='ORIGIN_CURSOR')
bpy.ops.object.select_all(action='TOGGLE')
bpy.ops.view3d.snap_selected_to_cursor(use_offset=False)
bpy.ops.object.editmode_toggle()
bpy.ops.mesh.select_all(action='TOGGLE')
bpy.ops.mesh.select_all(action='TOGGLE')
bpy.ops.object.editmode_toggle()
bpy.ops.object.select_all(action='TOGGLE')
bpy.ops.object.editmode_toggle()
bpy.ops.object.editmode_toggle()
bpy.ops.transform.translate(value=(0.820005, 0, 0), constraint_axis=(True, False, False), constraint_orientation='GLOBAL', mirror=False, proportional='DISABLED', proportional_edit_falloff='SMOOTH', proportional_size=1)
bpy.ops.object.select_all(action='TOGGLE')
bpy.ops.object.select_all(action='TOGGLE')
````
