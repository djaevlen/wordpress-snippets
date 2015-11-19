# wordpress-snippets
A collection of WordPress Snippets

### Get latest posts from any post type outside the main loop

```PHP
<?php $args = array (
	'post_type'              => array( 'post' ),
	'posts_per_page'         => '4'
);
$nyheder = new WP_Query( $args ); ?>
<?php if ( $nyheder->have_posts() ) : while ( $nyheder->have_posts() ) : $nyheder->the_post(); ?>

	<h3><?php the_title(); ?></h3>

	<?php the_content(); ?>

	<a href="<?php echo get_permalink(); ?>" class="btn btn-primary">
		Læs mere
	</a>

	<?php wp_reset_postdata();  ?>
<?php endwhile; endif; ?>
```


### Get content from an page, single or CPT

```PHP
<?php if (have_posts()) : while (have_posts()) : the_post(); ?>
	<h1 class="h2 title"><?php the_title(); ?></h1>
	<?php the_content(); ?>
<?php endwhile; endif;?>

```


### Get children and their childrens
It also adds an `active` class to the `li`

```PHP
<?php $id = get_the_ID(); ?>
<ul>
	<?php $args = array(
		'post_parent' 		=> 10,
		'post_type'   		=> 'page', 
		'numberposts' 		=> -1,
		'posts_per_page' 	=> -1,
		'order' 			=> 'ASC',
		'orderby'  			=> 'title'
	); ?>
	<?php $children_array = get_posts($args); ?> 
	<?php //print_r($children_array); ?>
	<?php foreach ($children_array as $value) : ?>
		<li class="<?php echo ($id === $value->ID ? "active" : "" ); ?>">
			<a href="<?php echo get_permalink($value->ID); ?>" title="<?php echo $value->post_title; ?>"><?php echo $value->post_title; ?></a>
		</li>
		<?php $args = array(
			'post_parent' 		=> $value->ID,
			'post_type'   		=> 'page',
			'numberposts' 		=> -1,
			'posts_per_page' 	=> -1,
			'order' 			=> 'ASC',
			'orderby'  			=> 'title'
		); ?>
		<?php $children_array_sub = get_posts( $args); ?> 
		<?php if($children_array_sub) : ?>
			<ul class="sub">
				<?php foreach ($children_array_sub as $value) : ?>
					<li class="<?php echo ($id === $value->ID ? "active" : "" ); ?>">
						<a href="<?php echo get_permalink($value->ID); ?>" title="<?php echo $value->post_title; ?>"><?php echo $value->post_title; ?></a>
					</li>
				<?php endforeach; ?>
			</ul>
		<?php endif; ?>
	<?php endforeach; ?>
</ul>
```

### Bootstrap collapse
Need the url_slug() function in the functions.php
```PHP
<?php $gruppe = 0; ?>
<?php if( have_rows('faq_kategorier') ) : while ( have_rows('faq_kategorier') ) : the_row(); ?>
    	<h3><?php the_sub_field('kategori_titel'); ?></h3>
    	// Need the url_slug() function in the functions.php
    	<?php $current = url_slug(get_sub_field('kategori_titel')); ?>
    	<div class="panel-group" id="<?php echo $current; ?>" role="tablist" aria-multiselectable="true">
		<div class="panel panel-default">
			<?php $i = 0; ?>
			<?php if( have_rows('sporgsmal_og_svar') ) : while ( have_rows('sporgsmal_og_svar') ) : the_row(); ?>
    				<div class="group">
					<div class="panel-heading" role="tab" id="heading-<?php echo $i; ?>">
						<h4 class="panel-title">
							<a data-toggle="collapse" data-parent="#<?php echo $current; ?>" href="#<?php echo $current; ?>-<?php echo $gruppe; ?>-<?php echo $i; ?>" aria-expanded="true" aria-controls="collapse-<?php echo $gruppe; ?>-<?php echo $i; ?>">
								<?php the_sub_field('sporgsmal'); ?>
							</a>
						</h4>
					</div>
					<div id="<?php echo $current; ?>-<?php echo $gruppe; ?>-<?php echo $i; ?>" class="panel-collapse collapse" role="tabpanel" aria-labelledby="heading-<?php echo $gruppe; ?>-<?php echo $i; ?>">
						<div class="panel-body">
							<?php the_sub_field('svar'); ?>
						</div>
					</div>
				</div>
				<?php $i++; ?>
		    	<?php endwhile; endif; ?>
		</div>
	</div>
	<?php $gruppe++; ?>
<?php endwhile; endif; ?>
```

