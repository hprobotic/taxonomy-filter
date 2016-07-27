# taxonomy-filter
Add wordpress taxonomy filter on list post
```
<?php
/**
 * Display a custom taxonomy dropdown in admin
 * @author John Pham
 * @link http://github.com/hprobotic
 */
add_action('restrict_manage_posts', 'filter_post_type_by_taxonomy');
function filter_post_type_by_taxonomy() {
	global $typenow;
	$post_type = 'product'; // your post type slug here
	$taxonomy  = 'product_category'; // your taxonomy slug here
	if ($typenow == $post_type) {
		$selected      = isset($_GET[$taxonomy]) ? $_GET[$taxonomy] : '';
		$info_taxonomy = get_taxonomy($taxonomy);
		wp_dropdown_categories(array(
			'show_option_all' => __("Show All {$info_taxonomy->label}"),
			'taxonomy'        => $taxonomy,
			'name'            => $taxonomy, 
			'selected'        => $selected,
			'show_count'      => true,
			'hide_empty'      => true,
		));
	};
}
/**
 * Filter posts by taxonomy in admin
 * @author  John Pham
 * @link http://github.com/hprobotic
 */
add_filter('parse_query', 'convert_id_to_term_in_query');
function convert_id_to_term_in_query($query) {
	global $pagenow;
	$post_type = 'product'; // your post type slug here
	$taxonomy  = 'product_category'; // your taxonomy slug here
	$q_vars    = &$query->query_vars;
	if ( $pagenow == 'edit.php' && isset($q_vars['post_type']) && $q_vars['post_type'] == $post_type && isset($q_vars[$taxonomy]) && is_numeric($q_vars[$taxonomy]) && $q_vars[$taxonomy] != 0 ) {
		$term = get_term_by('id', $q_vars[$taxonomy], $taxonomy);
		$q_vars[$taxonomy] = $term->slug;
	}
}

```
