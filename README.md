# wp-allimport-UID
Snippet of code to generate a UID based on the row count

This really needs to be combined with other data on your spreadsheet and is definitely an edge-case scenario. (Context: multiple rows with exactly the same data, but needing to be unique entries)

Use `[wpai_increment_number_persistent()]` to place the index.

Add the following to your WP allimport functions:
`function wpai_increment_number_persistent() {
    // Check if the counter has been initialized.
    $current_number = get_option('wpai_last_used_number', 0);

    // Increment the number by 1.
    $new_number = $current_number + 1;

    // Update the option for the next entry.
    update_option('wpai_last_used_number', $new_number);

    // Return the new number for this entry.
    return $new_number;
}
function wpai_reset_increment_number() {
    // Reset the counter to 0 (or any desired starting value).
    update_option('wpai_last_used_number', 0);
}
add_action('pmxi_before_xml_import', 'wpai_reset_increment_number');

`
The last function and action resets the count, to ensure you can process the same data again.
