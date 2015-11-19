# wordpress-snippets
A collection of WordPress Snippets


### Get children and their childrens

```HTML
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
´´´
