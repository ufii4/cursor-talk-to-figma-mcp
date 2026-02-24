# Workflow Recipes

## New Design System from Scratch

1. **Create pages**:
   `create_page("Foundations")`, `create_page("Components")`, `create_page("Examples")`

2. **Set up tokens on Foundations**:
   - `set_current_page(pageName: "Foundations")`
   - `get_available_fonts(query: "Inter")` — verify fonts first
   - `create_variable_collection("Appearance")` — rename default mode to "Light", add "Dark"
   - `create_variable_collection("Sizes")` — set up spacing/radius tokens
   - Create color variables + `set_variable_value` per mode
   - Create paint styles for brand colors
   - Create text styles for type scale
   - Create effect styles for shadows

3. **Build components on Components page**:
   - `set_current_page(pageName: "Components")`
   - For each component group:
     - Create section as container
     - Create auto-layout frame inside section for content
     - Build variant components with `create_component`
     - `combine_as_variants` to create the set
     - `add_component_property` for text/boolean overrides
     - `set_variable_binding` to connect tokens
     - `set_export_settings` for dev handoff presets
     - Verify with `export_node_as_image`

4. **Compose examples on Examples page**:
   - Create auto-layout frames for layouts
   - Drop in instances with `create_instance_from_local`
   - `set_multiple_text_contents` to batch-update labels
   - Verify with `export_node_as_image`

## Adding a New Component

1. Explore existing patterns:
   `get_local_components(setsOnly: true, limit: 5)` then `get_component_by_id` on one
2. Navigate: `set_current_page(pageName: "Components")`
3. Create variants following existing naming convention
4. `combine_as_variants(componentIds: [...], name: "Component Name")`
5. Verify: `export_node_as_image(nodeId, format: "PNG", scale: 2)`

## Exploring an Unknown File

1. Connect: `join_channel("channel-id")` then `get_current_page()`
2. Scan structure: `get_pages()`
3. Explore tokens: `get_styles()`, `get_local_variable_collections()`
4. Explore components: `get_local_components(setsOnly: true, limit: 20)`
5. Navigate: `set_current_page(pageName: "...")` then `get_current_page()`
6. Visual overview: `export_node_as_image(nodeId, format: "PNG", scale: 0.5)`

## Batch Property Updates

Use `set_node_properties` for efficient multi-property updates:
```
# Instead of 3 separate calls:
set_node_properties(nodeId, properties: { opacity: 0.5, cornerRadius: 8 })
# Note: fill color still needs set_fill_color for the paint array
```

## Tool Categories Quick Reference

**Structure**: `create_page` `rename_page` `create_section` `create_frame` `create_auto_layout` `create_component` `create_component_from_node` `combine_as_variants` `add_component_property`

**Primitives**: `create_rectangle` `create_ellipse` `create_text` `create_line` `create_node_from_svg` `create_boolean_operation`

**Layout**: `set_layout_mode` `set_item_spacing` `set_padding` `set_axis_align` `set_layout_sizing` `set_constraints`

**Appearance**: `set_fill_color` `set_stroke_color` `set_corner_radius` `set_opacity` `set_effects` `set_export_settings`

**Tokens**: `create_variable_collection` `create_variable` `set_variable_value` `add_mode` `rename_mode` `remove_mode` `set_variable_binding` `create_paint_style` `create_text_style` `create_effect_style` `apply_style_to_node`

**Manipulation**: `move_node` `resize_node` `clone_node` `delete_node` `delete_multiple_nodes` `insert_child` `set_node_properties`

**Text**: `set_text_content` `set_multiple_text_contents` `scan_text_nodes`

**Instances**: `create_instance_from_local` `get_instance_overrides`

**Navigation**: `set_current_page` `set_focus` `set_selections` `zoom_into_view` `set_viewport`

**Read/Explore**: `get_current_page` `get_pages` `get_document_info` `get_node_info` `get_nodes_info` `search_nodes` `get_selection` `read_my_design` `export_node_as_image` `get_node_css` `get_available_fonts`

**Token/Component Read**: `get_styles` `get_style_by_id` `remove_style` `get_local_variables` `get_variable_by_id` `get_local_variable_collections` `get_variable_collection_by_id` `get_local_components` `get_component_by_id`
