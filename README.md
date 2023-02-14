# Custom post type with taxonomy and custom message

## Custom Post Type
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
```

## Custom Taxonomy
```
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
```

## Custom Message
### 1
```

// CUSTOM MESSAGES

function book_custom_messages( $messages ) {
  global $post, $post_ID;
  $messages['book'] = array(
    0  => '', // Unused. Messages start at index 1. 
    1 => sprintf( __('Book updated. <a href="%s">View Book</a>'), esc_url( get_permalink($post_ID) ) ),
    2 => __('Custom field updated.'),
    3 => __('Custom field deleted.'),
    4 => __('Book updated.'),
    5 => isset($_GET['revision']) ? sprintf( __('Book restored to revision from %s'), wp_post_revision_title( (int) $_GET['revision'], false ) ) : false,
    6 => sprintf( __('Book published. <a href="%s">View Book</a>'), esc_url( get_permalink($post_ID) ) ),
    7 => __('Product saved.'),
    8 => sprintf( __('Book submitted. <a target="_blank" href="%s">Preview product</a>'), esc_url( add_query_arg( 'preview', 'true', get_permalink($post_ID) ) ) ),
    9 => sprintf( __('Book scheduled for: <strong>%1$s</strong>. <a target="_blank" href="%2$s">Preview product</a>'), date_i18n( __( 'M j, Y @ G:i' ), strtotime( $post->post_date ) ), esc_url( get_permalink($post_ID) ) ),
    10 => sprintf( __('Book draft updated. <a target="_blank" href="%s">Preview product</a>'), esc_url( add_query_arg( 'preview', 'true', get_permalink($post_ID) ) ) ),
  );
  return $messages;
}
add_filter( 'post_updated_messages', 'book_custom_messages' );
```
### 2
```
add_filter( 'post_updated_messages', 'book_custom_messages' );
function book_custom_messages( $messages ) {
    $post             = get_post();
    $post_type        = get_post_type( $post );
    $post_type_object = get_post_type_object( $post_type );
    $messages['book'] = array(
        0  => '', // Unused. Messages start at index 1.
        1  => __( 'Book updated.', 'textdomain' ),
        2  => __( 'Custom field updated.', 'textdomain' ),
        3  => __( 'Custom field deleted.', 'textdomain' ),
        4  => __( 'Book updated.', 'textdomain' ),
        5  => isset( $_GET['revision'] ) ? sprintf( __( 'Book restored to revision from %s', 'textdomain' ), wp_post_revision_title( (int) $_GET['revision'], false ) ) : false,
        6  => __( 'Book published.', 'textdomain' ),
        7  => __( 'Book saved.', 'textdomain' ),
        8  => __( 'Book submitted.', 'textdomain' ),
        9  => sprintf(
            __( 'Book scheduled for: <strong>%1$s</strong>.', 'textdomain' ),
            date_i18n( __( 'M j, Y @ G:i', 'textdomain' ), strtotime( $post->post_date ) )
        ),
        10 => __( 'Book draft updated.', 'textdomain' )
    );
    if ( $post_type_object->publicly_queryable ) {
        $permalink = get_permalink( $post->ID );
        $view_link = sprintf( ' <a href="%s">%s</a>', esc_url( $permalink ), __( 'View book', 'textdomain' ) );
        $messages[ $post_type ][1] .= $view_link;
        $messages[ $post_type ][6] .= $view_link;
        $messages[ $post_type ][9] .= $view_link;
        $preview_permalink = add_query_arg( 'preview', 'true', $permalink );
        $preview_link      = sprintf( ' <a target="_blank" href="%s">%s</a>', esc_url( $preview_permalink ), __( 'Preview book', 'textdomain' ) );
        $messages[ $post_type ][8] .= $preview_link;
        $messages[ $post_type ][10] .= $preview_link;
    }
    return $messages;
}
```


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


// CUSTOM MESSAGES

function book_custom_messages( $messages ) {
  global $post, $post_ID;
  $messages['book'] = array(
    0  => '', // Unused. Messages start at index 1. 
    1 => sprintf( __('Book updated. <a href="%s">View Book</a>'), esc_url( get_permalink($post_ID) ) ),
    2 => __('Custom field updated.'),
    3 => __('Custom field deleted.'),
    4 => __('Book updated.'),
    5 => isset($_GET['revision']) ? sprintf( __('Book restored to revision from %s'), wp_post_revision_title( (int) $_GET['revision'], false ) ) : false,
    6 => sprintf( __('Book published. <a href="%s">View Book</a>'), esc_url( get_permalink($post_ID) ) ),
    7 => __('Product saved.'),
    8 => sprintf( __('Book submitted. <a target="_blank" href="%s">Preview product</a>'), esc_url( add_query_arg( 'preview', 'true', get_permalink($post_ID) ) ) ),
    9 => sprintf( __('Book scheduled for: <strong>%1$s</strong>. <a target="_blank" href="%2$s">Preview product</a>'), date_i18n( __( 'M j, Y @ G:i' ), strtotime( $post->post_date ) ), esc_url( get_permalink($post_ID) ) ),
    10 => sprintf( __('Book draft updated. <a target="_blank" href="%s">Preview product</a>'), esc_url( add_query_arg( 'preview', 'true', get_permalink($post_ID) ) ) ),
  );
  return $messages;
}
add_filter( 'post_updated_messages', 'book_custom_messages' );
```

## References
[register_post_type](https://developer.wordpress.org/reference/functions/register_post_type/)

[register_taxonomy](https://developer.wordpress.org/reference/functions/register_taxonomy/)

[register_taxonomy_for_object_type](https://developer.wordpress.org/reference/functions/register_taxonomy_for_object_type/)

[The Complete Guide To WordPress Custom Post Types](https://www.smashingmagazine.com/2012/11/complete-guide-custom-post-types/)

[How To Create A Custom Taxonomy In WordPress](https://www.smashingmagazine.com/2012/01/create-custom-taxonomies-wordpress/)

[Extending WordPress With Custom Content Types](https://www.smashingmagazine.com/2015/04/extending-wordpress-custom-content-types/)
