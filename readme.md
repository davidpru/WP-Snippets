# Wordpess Snippets
***
Colección de snippets para crear loops en Wordpress desde lo más básico hasta lo más complejo.

### Loop básico
Loop para entradas y páginas de Wordpress

```
<?php if (have_posts()) : ?>
  <?php while (have_posts()) : the_post(); ?>

    <article class="post" id="post-<?php the_ID(); ?>">
      <h2><a href="<?php the_permalink() ?>" rel="bookmark" title="Permanent Link to <?php the_title(); ?>"><?php the_title(); ?></a></h2>             
      <div class="entry">
        <?php the_content('Read the rest of this entry &raquo;'); ?>
      </div>       
    </article>    

  <?php endwhile; ?>

  <div class="navigation">
      <div class="alignleft"><?php next_posts_link('&laquo; Previous Entries') ?></div>
      <div class="alignright"><?php previous_posts_link('Next Entries &raquo;') ?></div>
  </div>

<?php else : ?>

    <h2 class="center">Not Found</h2>
    <p class="center">Sorry, but you are looking for something that isn't here.</p>
    <?php include (TEMPLATEPATH . "/searchform.php"); ?>

<?php endif; ?>

```


### Query post

```
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
