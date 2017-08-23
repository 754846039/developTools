# sass
## Sass简介

Sass 是一种css预处理器,CSS 预处理器定义了一种新的语言，其基本思想是，用一种专门的编程语言，为 CSS 增加了一些编程的特性，将 CSS 作为目标生成文件，然后开发者就只要使用这种语言进行编码工作。

二、sass安装
1、安装预处理器
安装node
打开cmd  
npm install cnpm -g		-g全局安装
cnpm install node-sass -g	转换语法
node-sass  -v 	-v查看版本号

新建文件index.html,index.scss文件--->
键入代码，执行cmd，键入cd（change directory更换目录）空格+路径--->
如果不是在c盘，键入F:回车 然后再cd...--->
键入dir 检查--->
运行 node-sass index.scss index.css--->
监测机制：
node-sass -w index.scss index.css --output-style（输出样式） expanded --source-map index.css.map（映射）

--source-map   从源代码到编译的代码映射

退出监测：两次ctrl+c

#### 输出方式
* nested：嵌套缩进的css代码，它是默认值。
* expanded：没有缩进的、扩展的css代码。
* compact：简洁格式的css代码。
* compressed：压缩后的css代码。

三、Sass的基本特性
  1、变量（定义变量后必须加分号）
	$link-color:#000 ！default;默认（拥有最低优先级）
	a{color:$link-color} 属性值可以直接使用变量
     局部变量（块级作用域{}）
	.box{
	    $local:#fff;
	    background:$local;
	}

  2、嵌套 &表示当前选择器   最多4层
	.big{
	    .small{
		&:hover{
		}
	    }
	    &:hover .small{
	    }
	}
  3、混合宏（相当于js中的函数）支持参数，适合在需要参数时用
     @mixin test($width:1px){
	border:$width solid balck
     }不会再css中显示
     .box{
	@include test(7px)
     }

  4、扩展/继承 @entend（利用群组选择器提高代码使用效率，代码优化）
     .btn{样式代码}
     .btn-default{@extend .btn;background:red;}

  5、占位符
     %btn{样式代码}
     .btn-default{@extend %btn;background:red;}

  6、插值
     #{}允许在#{}内部解析变量
·选择器和属性要使用变量，需采用插值的方式，属性值可以直接使用变量

  7、注释
	/*会在编译出来的 CSS 显示*/
	//不会在编译出来的 CSS 显示

  8、数据类型
	①数字: 如，1、 2、 13、 10px；
	②字符串：有引号字符串或无引号字符串，如，”foo” ‘bar’ baz；
	③颜色：如，blue, #04a3f9, rgba(255,0,0,0.5);
	④布尔型：如，true, false；
	⑤空值：如，null；
	⑥值列表：用空格或者逗号分开，如，1.5em 1em 0 2em , Helvetica, Arial, sans-serif

  9、运算
颜色、宽高都可进行+-*/运算
 font: 10px/8px;             // 纯 CSS，不是除法运算

  10、@debug	输出到编译器里

  11、@import './_base.scss';如果是scss文件可以不写后缀

  12、转百分比 percentage(3/12 )
  13、@media  在scss中可以嵌套，省去选择器，直接输入属性

四、Sass高级特性

  1、@each
@each $x in $list{}		遍历每一个列表数据（只能遍历列表）

index(列表，值)			返回下标(从1开始)
nth(列表，下标)			返回对应下标在列表里的值
#{}插值			选择器和属性要使用变量，需采用插值的方式，属性值可以直接使用变量
lighten(颜色，百分比)		把每个颜色变浅百分之。。
darken()			加深

【eg】
创建3个不同颜色的按钮  .btn-red  .btn-blue  .btn-green
$red：#121212；
$blue：#121212；
$green：#121212；			定义颜色变量
$color-list:$red,$blue,$green;		定义一个变量，用，或空格分开
$class-list:'red'.'blue','green';
@each $color in $color-list{		
    $i:index($color-list,$color)	1,2,3
    $n:nth($class-list,$i)		red,blue,green
    .btn-#{$n}{				
	@expend %btn;
	background:$color; 		
	border:1px solid lighten($color,30)
    }
}

  2、@for
@for $i from <start> through <end>
@for $i from <start> to <end>

  3、@if
 @if 条件 { }@else { }

五、sass函数

  1、map类型的数据
map_keys(map)		返回一个列表，列表中包含所有的键
map_get(map,key)	根据键取值

【eg】
$map:(
    'black':black,
    'red':$red,
    'blue':#6aacff
)

@each $key in map_keys($map){
    .btn-#{$key}{
	@extend %btn;
	background:map_get($map,$key)	获取值
    }
}
【eg】
$base-size:16px;
$font-map:(
    'small':0,8,
    'middle':1,
    'large':1.5
)
@each $key in map_keys($font-map){
    .#{$key}{
	font-size:$base-size * map_get($font-map,$key);
    }
