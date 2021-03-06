---
title: Clone
category: field-types
group: Layout
status: draft
---

## Description
The Clone field allows you to re-use existing fields across multiple field groups. Rather than duplicating fields in the database, it loads and displays the selected fields at run-time.

This field may be displayed in one of two ways. It can either replace itself with the selected fields (seamless) or display the selected fields as a group of sub fields. These two display types, along with the ability to customise the name of cloned fields, makes this field an extremely versatile tool for modular theme development.

## Screenshots
<div class="gallery">
	<figure>
		<a href="#">
			<img src="#" alt="A Clone field that allows you to display existing fields" />
		</a>
		<figcaption>The Clone field interface</figcaption>
	</figure>
	<figure>
		<a href="#">
			<img src="#" alt="List of field settings shown when setting up a Clone field" />
		</a>
		<figcaption>The Clone field settings</figcaption>
	</figure>
</div>

## Settings
- **Fields**  
  Defines fields you would like to clone. Select specific fields, or all fields from a specific field group.
  
- **Display**  
  Specifies the style used to render clone fields.  
  _Seamless_: Completely replaces the clone field with selected fields. Useful to re-use fields within a Repeater or Flexible Content field.  
  _Group_: Displays selected fields within a group. Useful to re-use an existing group containing 'button' settings, as seen in following examples.
  
- **Layout**  
  Defines the layout style used to render the field interface.  
  _Block_: Fields are displayed in blocks, one after the other. Labels are top aligned.  
  _Table_: Fields are displayed in a table with each field in its own column. Labels will appear in the table header.  
  _Row_: Fields are displayed in a two column table. Labels will appear in the first column.  
  
- **Prefix Field Labels**  
  Modifies all selected field labels and prefixes the current clone field's label. Useful when using the `Seamless` display. You could name your clone field 'Hero' and have it prefixed to all selected fields like 'Hero Button Text', 'Hero Button URL', etc.
  
- **Prefix Field Names**
  Modifies field's name (used to save/load values) Useful when using the `Group` display. You could clone a group multiple times on one edit screen but have them save data with different names. Eg. 'hero_button_text', 'welcome_button_text', etc.

## Changelog
- Added in version 5.4.0.

## Template usage
Loading the value of a cloned field is the same as loading a normal field. If using the ‘Group’ display setting, you may also load all cloned values as an array by loading the Clone field itself.

The following examples use a clone field called 'main_button' which is cloning a field group called 'Button'. The 'Button' field group contains 2 fields called 'text' and 'url'.

### Loading cloned values
This example demonstrates how to load a Clone field value with the following default settings:
  - **Name:** main_button
  - **Display:** seamless
  - **Prefix Names:** no
  - **Fields:** A field group called 'Button' described above

```
<?php
$text = get_field('text');
$url = get_field('url');
```

### Loading prefixed cloned values
This example demonstrates how to load a Clone field value with the following 'prefixed group' settings:
  - **Name:** main_button
  - **Display:** group
  - **Prefix Names:** yes
  - **Fields:** A field group called 'Button' described above
```
<?php 

// Load individual values.
$text = get_field('main_button_text');
$url = get_field('main_button_url');

// Load values as a group.
$button = get_field('main_button');
$text = $button['text'];
$url = $button['url'];
```

### Cloned fields within a Repeater
Clone fields may be used as a sub-field within a Repeater or Flexible Content field. This example demonstrates using the 'main_button' Clone field from previously examples within a Repeater field.
```
<?php 
if( have_rows('slides') ) {
    while( have_rows('slides') ) {
        the_row();
		
		// Load clone field as a group.
        $button = get_sub_field('button');

        // Do something...
    }
}
```

## Notes

### Limitations
The Clone field contains a few limitations.

1. Multiple cloned sub fields
A cloned sub field may not be able to save its value if it exists next to another instance of itself. For example, imagine a repeater field (called 'Row') containing 2 clone fields both cloning in the same field (called ‘Cell’). Even if both clone fields use the _Prefix Name_ setting to make each ‘Cell’ have a unique name, they both use the same `field_key`, so they will override each other during save.

2. have_rows() nested clone field
The `have_rows()` function will return false when loading a cloned sub field using the _Display_ setting 'Block'. Please note that using the 'Seamless' display setting will allow sub `have_rows()` loops to work as expected.

### Data structure
The Clone field does not change the data structure when saving values to the database. The only change comes from the _Prepend field name_ setting. Values are not saved as an array, but are saved as normal individual field values. This means that meta queries are possible as per normal.

### Data conflicts
It is possible to create data conflicts by cloning a field with a name already used in the group. This is the same issue as creating two fields using the same name. Please use the _Prefix Field Names_ setting when appropriate to avoid this.

A good rule of thumb is to use the _Prefix Field Names_ setting when choosing the 'Group' display setting, and the opposite when using the 'Seamless' group setting.

### Disabled field groups
It is possible to clone a field group that has is set to 'Disabled'. Disabling a field group prevents it from being loaded on a post edit page, which is a good idea when making 'module' groups.

### Cloning a clone
It is possible to clone a clone field, which can be useful in specific scenarios. For example, if you are creating a page builder with the Flexible Content field and find each layout contains the same 'settings' fields. Instead of duplicating these 'settings' fields in each layout (and the headache of maintaining changes to these fields across multiple layouts), you could define all 'settings' fields in solely the first layout. The second layout could use a clone field to clone these 'settings' fields. The third layout could use a clone field to clone the one defined in the second layout.

The benefit of following this pattern is that if you add a new 'setting' to the first layout, you only need to update the clone field in your second layout. All other clone fields (in layout 3, 4, etc.) will then mimic this and show the new 'settings' field.
