# Steps to aling servos with plastic parts for assembly

### Align pivot on plastic part with joint cylinder

1) Make the center of the top surface of the joint cylinder the origin for that geometry then hide geometry




2) Select the top surface of the pivot

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
