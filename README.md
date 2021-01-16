# custom_post_types WP主题开发教学QQ群： 706173813

/**
	@package https://developer.wordpress.org/reference/functions/register_post_type/
	https://developer.wordpress.org/reference/functions/get_post_type_capabilities/
*/

//get_post_type_object 函数来获取文章类型的object

add_action('init','custom_post_types');

function custom_post_types() {
	$args = array(
		'label'  => 'Portfolios',     //posttype的名称，显示在左侧菜单上，通常是复数，默认的值是$labels数组里面的name项。
		'labels'  => array(     //labels是post type一些列的名称标签，也是数组.如果没有设置labels, 默认的话non-hierarchical types继承的是post的标签，hierarchical types继承的是page的标签。
			'name'  => 'Portfolios',     //post type 名称，一般是复数，等同于 $post_type_object->label，
			'singular_name'  => 'Portfolio',    //单个项目名称。单数
			'add_new'  => _x( 'Add New', 'portfolio', 'textdomain' );    //默认是‘Add New’ for both hierarchical and non-hierarchical types. 
			'add_new_item'  => 'Add New Portfolio',    //添加单个项目时候的标签，默认是‘Add New Post’ / ‘Add New Page’.
			'edit_item'  => 'Edit Portfolio',   //编辑单个项目时候的标签，默认是Edit Post’ / ‘Edit Page
			'new_item'  => 'New Portfolio',    //新项目页面标题标签，默认是New Post’ / ‘New Page
			'view_item' => 'View Portfolio',    //查看单个项目时候的标签，默认是 View Post’ / ‘View Page，
			'view_items'  => 'View Portfolios',    //查看post type archives时候的标签，默认是View Posts’ / ‘View Pages
			'search_items'  => 'Search Portfolios',   //搜索多个项目时候的标签。
			'not_found'  => 'No Portfolio Found',   //当项目没有找到时候的标签 默认是 No posts found
			'not_found_in_trash'  => 'No portfolios found in Trash', //当没有项目在垃圾箱里的标签
			'parent_item_colon'  => 'Paren Porfolio',   // 来命名作为父层时候的名字，Label used to prefix parents of hierarchical items. Not used on non-hierarchical post types。只在有层级的文章类型使用
			'all_items' => 'All Portfolios',   //在菜单名称下面子级菜单表示所有项目名称时候的标签。默认是All Posts’ / ‘All Pages
			'archives'  => 'Portfolio Archives',   //在导航菜单添加显示分类名称的标签，默认是Post Archives’ / ‘Page Archives
			'attributes'  => 'Portfolio Attributes',   //给meta box 添加属性时候的标签，默认是Post Attributes’ / ‘Page Attributes
			'insert_into_item' => 'Insert into portoflio',   //媒体上传框架按钮标签，默认是‘Insert into post’ / ‘Insert into page
			'uploaded_to_this_item'  => 'Uploaded to this portfolio',   //媒体框架过滤标签。 默认是Uploaded to this post’ / ‘Uploaded to this page
			'featured_image'  => 'Featured image', //缩略图字段盒子标题， 默认是 'Featured image',
			'set_featured_image'  => 'Set featured image', //设置缩略图时候标签，默认是 Set featured image
			'remove_featured_image'  => 'Remove featured image',  //移除缩略图时候标签，默认是 Remove featured image
			'use_featured_image'  => 'Use as featured image', //在图片上传弹出media框里设置为缩略图时候的标签，默认是 Use as featured image
			'menu_name'  => 'Portfolios', //项目的菜单名称， 默认是 labels里面的name. (name是post type的name而这个menu name是显示在菜单上的name),
			'filter_items_list'  => 'Filter portfolios list',    //表格视图隐藏的标题标签。Label for the table views hidden heading. Default is ‘Filter posts list’ / ‘Filter pages list’.
			'items_list_navigation'  => 'Portfolios list navigation', //我也不知道啥意思。暂时没遇到过。Label for the table pagination hidden heading. Default is ‘Posts list navigation’ / ‘Pages list navigation’.
			'items_list'  => 'Portfolios list',  //列表隐藏标题标签。 Label for the table hidden heading. Default is ‘Posts list’ / ‘Pages list’.
			'item_published'  => 'Portfolio published',//用在项目被发布时候的标签，Label used when an item is published. Default is ‘Post published.’ / ‘Page published.’
			'item_published_privately'  => 'Portfolio published privately.'    //用在项目被带隐私权限时候发布的标签， Default is ‘Post published privately.’ / ‘Page published privately.’
			'item_reverted_to_draft'  => 'Porfolio reverted to draft.'  //Label used when an item is switched to a draft. Default is ‘Post reverted to draft.’ / ‘Page reverted to draft.’
			'item_scheduled'   => 'Portfolio scheduled.'    //用在当项目被按计划发布时候的标签。 Default is ‘Post scheduled.’ / ‘Page scheduled.’
			'item_updated'  => 'Portfolio updated.'  // 用在项目被更新时候的标签。默认是 Post updated.’ / ‘Page updated
		),

		'description'  => 'Portfolio post types', //(string) 简要介绍文章类型
		'public'  => true,   // 文章类型是否公开给管理界面或前端使用。默认是false. true显示此类型的用户页面。该类型可以从前端查询。 $exclude_from_search, $publicly_queryable, $show_ui, and $show_in_nav_menus  这三个值均继承自public的默认值。 当public设置为true的时候 这三个值分别默认为false,true,true,true.当public设置为false的时候，分别都可以自定义三个值的属性。 
		'hierarchical'  => false, //设置是否这个文章类型是分层级的，像page一样可以有子页面。 默认是false. 
		'exclude_from_search'  => false, // 从搜索结果排除，默认是 public值的相反。 
		'publicly_queryable'   => true, // 该类型查询允许从前端进行当作parse_request()的一部分。 Endpoints would include:
// ?post_type={post_type_key}
// ?{post_type_key}={single_post_slug}
// ?{post_type_query_var}={single_post_slug}
		'show_ui'  => true, // 是否在管理页面展现可 用来管理，默认是$public值
		'show_in_menu'  => true, // 是否在管理页面左侧菜单展示，选择在管理左侧菜单上显示。 要起作用， $show_ui要设置为true, 依附于show ui. 默认值是$show_ui的值。If a string of an existing top level menu (eg. 'tools.php' or 'edit.php?post_type=page'), the post type will be placed as a sub-menu of that. 
		'show_in_nav_menus'  => true, //设置文章类型可以在菜单选择页面那里可以被设置为菜单。  默认值为public的值。
		'show_in_admin_bar'  => true,  //设置文章类型可通过admin bar显示, 也就是在new 那里添加的时候会显示， 默认值是 $show_in_menu. 如果show_in_menu为否，这里选择是依然会显示。
		'show_in_rest'  => true,  //是否在rest api 添加这个文章类型 。 设置为true的话文章类型可以使用古腾堡块状编辑器。
		'rest_base'  => $post_type, //(string) To change the base url of REST API route. Default is $post_type.
		'rest_controller_class'  => 'WP_REST_Posts_Controller',  //(string) REST API Controller class name. Default is 'WP_REST_Posts_Controller'.
		'menu_position'  => null,   //在管理菜单上显示的顺序， 默认是按照先后顺序来。要生效的话，$show_in_menu必须设置为true，
		'menu_icon'  => 'dashicons-settings', //使用的icon图标，可以使用wp自带的dashicons也可以自己粘贴上icon的链接。 
		'capability_type'  => 'post',   //用来创建读、编辑和删除能力的文本。  该字符用于建立 read, edit, delete 功能。也可以通过传递数组来初始化功能，例如 array('story', 'stories').
		'capabilities'  => 'post',   //(array) Array of capabilities for this post type. $capability_type is used as a base to construct capabilities by default. See get_post_type_capabilities().
		'map_meta_cap'  => false,    //是否使用内部元能力处理   默认是false, 
		'supports'  => array('title','editor','thumbnail'),   // 文章类型调用支持的核心特征，如缩略图编辑器等。 可通过add_post_type_support( string $post_type, string|array $feature, mixed $args )添加自定义的特征
		'register_meta_box_cb'  => null, //提供一个回调函数，为编辑表单设置元框。  默认是null 
		'taxonomies'  => array('category','tags'),   //设置文章类型关联的分类类型。并在文章编辑页面显示和调用。 可以通过register_taxonomy() or register_taxonomy_for_object_type(). 来注册自定义分类类型
		'has_archive'  => true,   //是否设置文章类型包含分类存档列表， 默认是false. 如果设置为文本，就是这个archive要用的slug. 如果$rewrite可用就会重写规则。
		'rewrite'  => array('slug'  => 'anli'), // (bool|array)  是否激活url重写。  避免重写，设置为false.  默认是true， 使用$post_type当作slug. 要设置特定规则，通过array传递参数：
// 		'slug'
// (string) Customize the permastruct slug. Defaults to $post_type key.
// 'with_front'
// (bool) Whether the permastruct should be prepended with WP_Rewrite::$front. Default true.
// 'feeds'
// (bool) Whether the feed permastruct should be built for this post type. Default is value of $has_archive.
// 'pages'
// (bool) Whether the permastruct should provide for pagination. Default true.
// 'ep_mask'
// (const) Endpoint mask to assign. If not specified and permalink_epmask is set, inherits from $permalink_epmask. If not specified and permalink_epmask is not set, defaults to EP_PERMALINK.
		'query_var'  => $post_type,                 //(string|bool) 为文章类型设置query_var键。  默认是$post_type 键。 如果设置为否，?{query_var}={post_slug} 不能加载文章类型。如果设置为文本， ?{query_var_string}={post_slug} 查询有效
		'can_export'  => true, //是否设置可以通过wp 导出器导出这篇文章，默认 true， 
		'delete_with_user'  => null,   //是否当删除用户的时候也删除这类型的文章。  如果是，这用户所属的这类型文章都会被删除。  默认是null 
		'template'  =>    //用作编辑器会话的默认初始状态的块数组。每个项都应该是一个包含块名和可选属性的数组。
		'template_lock'  =>   //(string|false) Whether the block template should be locked if $template is set.
// If set to 'all', the user is unable to insert new blocks, move existing blocks and delete blocks.
// If set to 'insert', the user is able to move existing blocks but is unable to insert new blocks and delete blocks. Default false.
		





	);


	register_post_type( 'portfolios', $args);
