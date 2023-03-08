# Wordpess Snippets

Colección de snippets para crear loops en Wordpress

### Básicos

- [Loop básico](#loop-básico)
- [Query Custom Post](#query-custom-post)
- [Child Pages Loop](#child-pages-loop)
- [Loop post relacionados](#loop-post-relacionados)

### Terms

- [Get terms con link](#get-terms-link)

## Loop básico

> Ejemplo

```php
<?php if (have_posts()) : ?>
  <?php while (have_posts()) : the_post(); ?>

  <article <?php post_class(); ?> id="post-<?php the_ID(); ?>">
    <h2><a href="<?php the_permalink() ?>" rel="bookmark" title="Permanent Link to <?php the_title(); ?>"><?php the_title(); ?></a></h2>
    <div class="entry">
      <?php the_content('Leer más &raquo;'); ?>
    </div>
  </article>

  <?php endwhile; ?>

  <div class="post-nav">
    <div class="post-nav__left"><?php next_posts_link('&laquo; Previous Entries') ?></div>
    <div class="post-nav__right"><?php previous_posts_link('Next Entries &raquo;') ?></div>
  </div>

<?php else : ?>
    <h2 class="center">Not Found</h2>
    <p class="center">Sorry, but you are looking for something that isn't here.</p>
<?php endif; ?>

```

## Query Custom Post

> Ejemplo

```php
<?php $query = new WP_Query( array( 'post_type' => 'cp_name', 'posts_per_page' => -1 ) );
if ( $query->have_posts() ) : ?>
  <?php while ( $query->have_posts() ) : $query->the_post(); ?>

    <article class="post" id="post-<?php the_ID(); ?>">
      <h2><a href="<?php the_permalink() ?>" rel="bookmark" title="Permanent Link to <?php the_title(); ?>"><?php the_title(); ?></a></h2>
      <div class="entry">
        <?php the_content('Read the rest of this entry &raquo;'); ?>
      </div>
    </article>

  <?php endwhile; wp_reset_postdata(); ?>
  <!-- show pagination here -->
  <?php else : ?>
  <!-- show 404 error here -->
<?php endif; ?>
```

## Child Pages Loop

> Ejemplo

```php
<?php
$args = array(
    'post_parent' => $post->ID,
    'post_type' => 'page',
    'orderby' => 'menu_order'
);

$child_query = new WP_Query( $args );
?>

<?php while ( $child_query->have_posts() ) : $child_query->the_post(); ?>

    <div <?php post_class(); ?>>
        <?php
        if ( has_post_thumbnail() ) {
            the_post_thumbnail('page-thumb-mine');
        }
        ?>
        <h3><a href="<?php the_permalink(); ?>" rel="bookmark" title="<?php the_title(); ?>"><?php the_title(); ?></a></h3>
        <?php the_excerpt(); ?>
    </div>
<?php endwhile; ?>

<?php
wp_reset_postdata();

```

## Loop post relacionados

> Ejemplo

```php
<?php $orig_post = $post;
  global $post;
    $categories = get_the_category($post->ID);
    if ($categories) {
    $category_ids = array();
    foreach($categories as $individual_category) $category_ids[] = $individual_category->term_id;

  $args=array(
    'category__in' => $category_ids,
    'post__not_in' => array($post->ID),
    'posts_per_page'=> 3, // Number of related posts that will be shown.
    'caller_get_posts'=>1
  );

  $my_query = new wp_query( $args );
    if( $my_query->have_posts() ) {
    while( $my_query->have_posts() ) {
    $my_query->the_post();?>

    // content loop

<?php } } } $post = $orig_post; wp_reset_query(); ?>

```

### Terms

> Emjemplo basic

```php
$terms = get_terms( 'my_taxonomy' );
if ( ! empty( $terms ) && ! is_wp_error( $terms ) ){
	echo '<ul>';
	foreach ( $terms as $term ) {
		echo '<li>' . $term->name . '</li>';
	}
	echo '</ul>';
}
```

> Ejemplo custom

```php
$args = array( 'hide_empty=0' );

$terms = get_terms( 'my_term', $args );
if ( ! empty( $terms ) && ! is_wp_error( $terms ) ) {
	$count = count( $terms );
	$i = 0;
	$term_list = '<div class="my_term-archive">';
	foreach ( $terms as $term ) {
		$i++;
		$term_list .= '<a href="' . esc_url( get_term_link( $term ) ) . '" alt="' . esc_attr( sprintf( __( 'View all post filed under %s', 'my_localization_domain' ), $term->name ) ) . '">' . $term->name . '</a>';
		if ( $count != $i ) {
			$term_list .= ' &middot; ';
		}
		else {
			$term_list .= '</div>';
		}
	}
	echo $term_list;
}

```
