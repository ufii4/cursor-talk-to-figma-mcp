# Common Pitfalls & Solutions

## 1. Absolute Coordinates for Children

**Problem**: Using page-absolute coordinates when positioning children inside frames.
**Solution**: Use auto-layout (preferred). If manual positioning required:
```
child_x = desired_absolute_x - parent_absolute_x
child_y = desired_absolute_y - parent_absolute_y
move_node(nodeId: child, x: child_x, y: child_y)
```

## 2. Orphaned Nodes

**Problem**: Children placed outside parent's visible bounds.
**Solution**: Use auto-layout containers — children automatically flow into position.

## 3. Sections Don't Auto-Layout

**Problem**: Expecting sections to auto-layout their children.
**Solution**: Sections are canvas organizers only. Nest an auto-layout frame inside:
```
create_section(name: "Buttons", width: 800, height: 400)
create_frame(name: "Buttons Content", parentId: sectionId,
  layoutMode: "VERTICAL", itemSpacing: 24,
  layoutSizingHorizontal: "FILL", layoutSizingVertical: "HUG")
```

## 4. Text Overflow

**Problem**: Text content wider than parent frame.
**Solution**: `set_layout_sizing(nodeId: textId, layoutSizingHorizontal: "FILL")`

## 5. Font Loading Failures

**Problem**: `create_text_style` fails because font family/style doesn't exist.
**Solution**: Always verify first: `get_available_fonts(query: "Inter")`.
**Never** call `get_available_fonts()` without `query` — returns 450K+ chars.

## 6. Colors are 0-1, Not 0-255

**Problem**: Passing RGB values as 0-255.
**Solution**: Figma uses normalized 0-1 values.
```
# Hex #007AFF -> { r: 0/255, g: 122/255, b: 255/255 } = { r: 0, g: 0.478, b: 1 }
```

## 7. Local Components Use create_instance_from_local

**Problem**: Trying to instantiate a local component with the wrong tool or wrong ID type.
**Solution**: Always use `create_instance_from_local(componentId)` for same-file components. Pass the component's node ID (not a key). For component sets, pass the set ID — the default variant is auto-selected.

## 8. insert_child Doesn't Reposition

**Problem**: After `insert_child`, node appears at wrong position in new parent.
**Solution**: Node keeps old coordinates. Use an auto-layout parent (best) or follow with `move_node` using parent-relative coordinates.

## 9. create_component_from_node Changes the Node ID

**Problem**: After promoting a frame to a component, old node ID is invalid.
**Solution**: Always capture and use the returned ID:
```
create_component_from_node(nodeId: "4:22")  -> { id: "4:24", ... }
# "4:22" is now INVALID — use "4:24" for all subsequent operations
```

## 10. Component Set Naming Mismatch

**Problem**: `combine_as_variants` fails or creates wrong properties.
**Solution**: ALL variant component names must share the exact same property names:
```
# GOOD
"Size=Small, State=Idle"
"Size=Large, State=Idle"

# BAD — mismatched property names
"Size=Small, State=Idle"
"Style=Large, Active=True"
```
