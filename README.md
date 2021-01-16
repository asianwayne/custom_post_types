/**
	@package https://developer.wordpress.org/reference/functions/register_post_type/
	https://developer.wordpress.org/reference/functions/get_post_type_capabilities/
*/

//get_post_type_object 函数来获取文章类型的object

add_action('init','custom_post_types');

function custom_post_types() {
	
	register_post_type( 'portfolios', $args); }
注册自定义文章类型参数详解
