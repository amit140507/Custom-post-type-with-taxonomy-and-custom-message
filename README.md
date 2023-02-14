# Custom post type with taxonomy and custom message
## Ready to use code

```
// CUSTOM POST TYPE

function custom_post_type_book() {
	$labels = array(
		'name'               => _x( 'Books', 'post type general name' ),
		'singular_name'      => _x( 'Book', 'post type singular name' ),
		'add_new'            => _x( 'Add New Book', 'book' ),
		'add_new_item'       => __( 'Add New Book' ),
		'name_admin_bar'     => __( 'Book' ),
		'edit_item'          => __( 'Edit Book' ),
		'new_item'           => __( 'New Book' ),
		'all_items'          => __( 'All Books' ),
		'view_item'          => __( 'View Book' ),
		'search_items'       => __( 'Search Books' ),
		'not_found'          => __( 'No Book found' ),
		'not_found_in_trash' => __( 'No Book found in the Trash' ), 
		'parent_item_colon'  => '',
		'menu_name'          => 'Books'
	);
	$args = array(
		'labels'       	  => $labels,
		'description'  	  => 'Holds our books and book specific data',
		'public'       	  => true,
		'menu_position'	  => 5,
		'menu_icon' 	  => 'dashicons-book-alt', 
		'supports'        => array( 'title', 'editor', 'thumbnail', 'excerpt', 'comments' ),
		'has_archive'  	  => true,
		'capability_type' => 'post', 
		'hierarchical' 	  => true,
		'taxonomies'      => array( 'book_language','book_genre', 'post_tag' ),
		'rewrite'		  => array( 'slug' => 'books', 'with_front'=> true ),
		'query_var'       => true
	);
	register_post_type( 'book', $args ); 

}
add_action( 'init', 'custom_post_type_book' );

// CUSTOM TAXONOMY 1

function custom_taxonomy_book_language() {
	$labels = array(
		'name'              => _x( 'Book Languages', 'taxonomy general name' ),
		'singular_name'     => _x( 'Book Language', 'taxonomy singular name' ),
		'search_items'      => __( 'Search Book Language' ),
		'all_items'         => __( 'All Book Languages' ),
		'parent_item'       => __( 'Parent Book Language' ),
		'parent_item_colon' => __( 'Parent Book Language:' ),
		'edit_item'         => __( 'Edit Book Language' ), 
		'update_item'       => __( 'Update Book Language' ),
		'add_new_item'      => __( 'Add New Book Language' ),
		'new_item_name'     => __( 'New Book Language' ),
		'menu_name'         => __( 'Book Languages' ),
		'not_found'         => __( 'No Book Language found' ),
		'back_to_items'     => __( 'Back to Book Languages', 'textdomain' ),
	);
	$args = array(
		'labels'			=> $labels,
		'hierarchical'		=> true,
		'rewrite'			=> array( 'slug' => 'book-language', 'with_front'=> false, 'hierarchical'	=> false ),
		'query_var'         => true,
		'show_ui'           => true,
		'show_admin_column' => true,
	);
	register_taxonomy( 'book_languages', 'book', $args );
}
add_action( 'init', 'custom_taxonomy_book_language', 0 );

// CUSTOM TAXONOMY 2

function custom_taxonomy_book_genre() {
	$labels = array(
		'name'              => _x( 'Book Genres', 'taxonomy general name' ),
		'singular_name'     => _x( 'Book Genre', 'taxonomy singular name' ),
		'search_items'      => __( 'Search Book Genres' ),
		'all_items'         => __( 'All Book Genres' ),
		'parent_item'       => __( 'Parent Book Genre' ),
		'parent_item_colon' => __( 'Parent Book Genre:' ),
		'edit_item'         => __( 'Edit Book Genre' ), 
		'update_item'       => __( 'Update Book Genre' ),
		'add_new_item'      => __( 'Add New Book Genre' ),
		'new_item_name'     => __( 'New Book Genres' ),
		'menu_name'         => __( 'Book Genres' ),
		'not_found'         => __( 'No Book Genre found' ),
		'back_to_items'     => __( 'Back to Book Genres', 'textdomain' ),
	);
	$args = array(
		'labels'			=> $labels,
		'hierarchical'		=> true,
		'rewrite'			=> array('slug'	=> 'book-genre', 'with_front' => false, 'hierarchical'	=> false ),
		'query_var'         => true,
		'show_admin_column'	=> true,
		'show_ui'			=> true,
	);
	register_taxonomy( 'book_genres', 'book', $args );
}
add_action( 'init', 'custom_taxonomy_book_genre', 0 );


register_taxonomy_for_object_type( 'book_languages', 'book' );
register_taxonomy_for_object_type( 'book_genres', 'book' );
```
## References
[]()
[]()
[]()
[]()
[]()
[]()
