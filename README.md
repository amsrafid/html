# HTML Tag

One of the most flexible view builder for php.

# Basic use

Very easy to use. Attribute and tag name is same as normal html.
Most notable fact is that sudo or short name is also work as normal HTML attributes.

~~~
\Html\Tag::{Tag name}([
	i/id => 'id-name',
	c/cls/class => 'class-name',
	d-./_./*./data-. => 'data-value',
	b/body/txt/text => string|array|number|bool|function(){}		<!-- Tag Body -->

	...

	<!--
		Attribute name as array key and value as key value
		Note: Data attribues is handled with sudo [d-name/_name/*name].
		In all case, attribute name will be data-name.
	-->
]);
~~~

## Sudo attributes is available

~~~
a 		=	alt,
c 		=	class,
cls		=	class,
cont		=	content,
cs 		=	colspan,
d 		=	data,
da 		=	disabled,
dt 		=	datetime,
f 		=	for,
fa 		=	formaction,
h 		=	href,
i  		=	id,
ln 		=	lang,
m 		=	method,
mx 		=	max,
mn 		=	min,
mxlen		=	maxlength,
mnlen		=	minlength,
mt 		=	muted,
n  		=	name,
p  		=	placeholder,
pt		=	pattern,
r 		=	required,
rs 		=	rowspan,
rw 		=	rows,
s  		=	src,
sc		=	selected,
st		=	style,
t  		=	type,
v 		=	value,
val		=	value
~~~

# Preset functionality
Amsrafid Html allows preset as attributes or wrapper. That reduces using of same attribute and wrapper on same tag.

## Preset attributes for identical tag

Preset common attributes value, using set Tag.

~~~
Tag::set([
	'input' => [
		'c/cls/class' => 'form-control',
		...
	],
	...
]);

Tag::input(['type' => 'text']);

<!--
	Output:
	<input type = "text" class = "form-control" />
-->

Tag::stopSet();
~~~

## Preset wrapper for identical tag

Preset common wrapper value, using wrap Tag.

~~~
Tag::wrap([
	'input' => ['div', ['c' => 'col-md-6', ...]],
	'textarea' => 'div',
	...
]);

Tag::input(['t' => 'text']);
Tag::textarea();

<!--
	Output:
	<div class = "col-md-6"><input type = "text" class = "form-control" /></div>
	<div><textarea></textarea></div>
-->

Tag::stopSet();
~~~

# Special use

## Table

Html table is able to be generated dynamically. Where, body can be passed an array with key as tag name and key value a normal array for tag body.

~~~
$arrs = [
	['age' => 24, 'name' => 'Amsrafid'],
	['age' => 33, 'name' => 'Sadman Rafid']
];
	
Tag::table(['border' => '1', 'b' => function() use($arrs) {
	Tag::tr(['b' => ['th' => ['#', 'Age', 'Name']]]);
	Tag::tr(['foreach' => $arrs, 'offset' => 'i', 'start' => 1, 'b' => [
			'td' => ['@i', '@age', '@name']
		]
	]);
}]);
~~~

## Control statement

As like normal control statement foreach/if/elseif/else. Control statements uses as attributes.

### foreach:

Act like normal foreach in php. Here, 'offset', 'start' used for loop array/object affset, and from which value offset count will be started.

~~~
Tag::ul(['if' => $arrs, 'b' => function() use($arrs) {
	Tag::li(['foreach' => $arrs, 'offset' => 'i' 'v' => '@id', 'b' => '@i. @name']);
}]);

<!--
	@id -> @{array key name}.
	Able to capture in any attributes value

	Special Attributes:
		Attributes given bellow are useful only iff 'foreach' attribute is present.

	'if' => string
		Normal if condition. Ex: (@i > 2 && (@age == 50 || '@name' == 'HTML')).
		Here, @i is offset, @name is array key.
		Note: @name value is string type. So '@name' is binded with quotes.
		On the other hand, @age value is integer type. So, quote is not required.

	'offset' => string
		Loop array offset.
		In logical expression, consided to be started form 0 and in view depends on start attribute. 

	'start' => int
		From where body/view offset will be started from. DEFAULT 1
-->
~~~

### if:

Normal if statement like php.

~~~
$var = 10;

Tag::span(['if' => $var > 10, 'b' => 'Var is greated than 10']);

<!-- 
	Normal use:
	if($var > 0)
		echo "<span>Var is greated than 10</span>
-->
~~~

### elseif:

Normal elseif statement like php. Here, this condition will only work iff if statment is present before this.

~~~
Tag::span(['elseif' => $var > 5, 'b' => 'Var is greated than 5']);

<!-- 
	Normal use:
	if ($var > 10)
		...
	else if ($var > 5)
		echo "<span>Var is greated than 5</span>
-->
~~~

### else:

Normal else statement like php. Value will be anything eccept false. Here, this condition will only work iff if or elseif statment is present before this.

~~~
Tag::span(['else' => true, 'b' => 'Var is less than 5']);

<!-- 
	Normal use:	
	if ($var > 10)
		...
	else
		echo "<span>Var is less than 5</span>
-->
~~~

## Security Vulnerabilities

If you discover a security vulnerability within this, please send an e-mail to Sadman Rafid via [amsrafid@gmail.com](mailto:amsrafid@gmail.com). All security vulnerabilities will be promptly addressed.

## License

The Asrafid Html is open-source software licensed under the [MIT license](https://opensource.org/licenses/MIT).
