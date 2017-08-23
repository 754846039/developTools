# less

## less的用途
less是css预处理语言，扩充了CSS，如变量，继承，运算，函数等。

## 安装与使用
### 1. 在客户端通过 less.js 对于 .less 文件进行编辑（less.js 运行需基于服务器）
>不推荐使用，会加重浏览器负担，占用浏览器资源去编译 .less (浏览器打开时才编译)

- http://www.bootcss.com/p/lesscss/
- 下载 less.js 或在 github 上下载
- demo.html
```html
<link rel="stylesheet/less" type="text/css" href="demo.less">
<script src="less.js" type="text/javascript"></script>
```
- demo.less
```less
@color:#ccc;
body{
    background: @color;
}
```

### 2. 在本地/编辑器中直接把 less 文件编辑成 CSS 文件
- 安装 nodejs https://nodejs.org/en/
  - 在官网下载稳定版本
  - 装到C盘
  - 安装过程中勾选"Add to Path",添加到环境变量中
- 检查是否安装成功
  - cmd 打开命令窗口，`npm -v`查看版本号，如果正常输出则表示安装成功
  - 或者打开 webstorm ，Alt+F12 打开命令行 Teminal ，输入`npm -v`
- 安装 less  
  - 通过 npm 包管理工具全局安装 `npm install -g less`
  - `lessc -v`查看版本号
  - 安装css压缩插件`npm install -g less-plugin-clean-css`
- 编译方法
  - 通过命令行
    - `$ lessc styles.less`
    - `$ lessc demo.less > demo.css`
    - `$ lessc demo.less > demo.css -x` 压缩css
  - 利用 webstorm 监视并自动编译
    - 【方法一】文件-->设置-->工具-->File Watcher-->点击右上角添加less-->默认编译到less文件下，所以修改output path 到指定文件目录下`../css/....`
    - 【方法二】新建目录less-->新建 .less 文件--> 写入一些内容后顶部会出现自动提示，点击右上角 Add Watcher
http://blog.csdn.net/lhtzbj12/article/details/54882499
![less自动编译配置](amWiki/images/less.png)

## less配置
[http://blog.csdn.net/lhtzbj12/article/details/54882683](http://blog.csdn.net/lhtzbj12/article/details/54882683)

- 不压缩
    - `--no-color $FileName$ $FileNameWithoutExtension$.css --source-map=$FileNameWithoutExtension$.css.map`

- 压缩版
    - `--no-color $FileName$ $FileNameWithoutExtension$.min.css --clean-css --source-map=$FileNameWithoutExtension$.min.css.map`

## 语法
### 变量
- 声明一个变量
```less
@color:#000;
div{
    color:@color;
}
```   
- 可以用变量名定义为变量
```less
@black:#000;
@color:@black;
div{
    color:@@color;
}
```

### 嵌套规则
- 后代选择器嵌套
```less
// 后代选择器写法
#header {
    width:1200px;
    height:600px;
    p {
        font-size: 12px;
        a {
            text-decoration: none;
        }
    }
}
```

- 伪类选择器嵌套
```less
#header {
    width:1200px;
    height:600px;
    p {
        font-size: 12px;
        &:hover{
            background: #ccc;
        }
    }
}
```

- 多人协作开发一个页面时，为避免类名冲突，通常在类名前加前缀`hlj-header`
```html
<div class="hlj">
    <div class="hlj-box1"></div>
    <div class="hlj-box2"></div>
</div>
```
```less
.hlj{
    &-box1{
        color:"red";
    }
    & &-box2{
        color:"blue";
    }
}
```
```css
/*less生成的css*/
.hlj-box1{
    color:"red";
}
.hlj .hlj-box2{
    color:"blue";
}
```

### 混合
在一个类中调用另一个类，实现继承；还可以带参数的去调用，就像函数一样
- 不带参数混合
  - 暴露在css中
  ```less
  .cube{
      width:100px;
      height:100px;
  }
  .box{
      .cube;
  }
  ```
  - 不暴露在css中
  ```less
  .cube(){
      width:100px;
      height:100px;
  }
  .box{
      .cube;
  }
  ```

- 带参数混合（不会暴露在css中）
  - 参数没有默认值（必须加括号()调用）
  ```less
  .border-radius (@radius) {
      // 可以用来处理兼容性
      border-radius: @radius;
      -moz-border-radius: @radius;
      -webkit-border-radius: @radius;
  }

  .box{
      .border-radius(4px);
  }
  ```

  - 参数有默认值（可以不加括号()调用）
  ```less
  .rect(@width:200,@height:100) {
      width:@width*1px;
      height:@height*1px;
  }
  .box1{
      .rect;  
  }
  .box2{
      .rect(500,200);
  }
  .box3{
      .rect(@height:50);
  }
  ```

  - @arguments对象
  ```less
  .animation(@name,@duration,@fun:linear,@delay:0s,@loop:1,@direction:normal,@fillMode:backwards){
      /* @arguments代表所有参数,快速把所有参数全部写进来 */
      animation: @arguments;
  }
  ```

### 引用
```less
@import "basic.less";
@import "basic";
```

### 注释
- `//不会被编译的注释`
- `/*会被编译到css文件中的注释*/`

### 运算
任何数字、颜色或者变量都可以参与运算<br>
运算提供了加，减，乘，除操作；我们可以做属性值和颜色的运算，这样就可以实现属性值之间的复杂关系。<br>
括号也同样允许使用,并且可以在复合属性中进行运算。
```less
.opacity(@opacity){
    opacity: @opacity;
    @num:@opacity*100;
    filter: alpha(opacity=@num);
}
.box{
    .opacity(0.5);
}
```

### 模式匹配
只有被匹配的混合才会被使用。变量可以匹配任意的传入值，而变量以外的固定值就仅仅匹配与其相等的传入值。
```less
// 文章白天和晚上的样式不同
.article(day){
    background: #fff;
    color: #333;
}
.article(night){
    background: #333;
    color: #fff;
}

@type:day;
.box{
    .article(@type);
}
```

### 导引表达式
导引中可用的全部比较运算有： > >= = =< <。此外，关键字true只表示布尔真值，除去关键字true以外的值都被视为布尔假。<br>
导引序列使用逗号‘,’—分割，当且仅当所有条件都符合时，才会被视为匹配成功。<br>
在导引序列中可以使用and关键字实现"与"条件.<br>
在导引序列中可以使用not关键字实现"或"条件.<br>
```less
.max (@a, @b) when (@a < @b) { width: @b };
.max (@a, @b) when (@a >= @b) { width: @a };
/*多个条件用逗号分隔*/
.middle (@num) when (@num>10), (@num<20){
    width:800px;  
}
// 递归(十二栅格)
.response (@i:1,@type:xs) when (@i<=12){
    .col-@{type}-@{i}{
        width:@i/12*100%;
    }
    .response(@i+1,@type);
}

```

### 函数
#### 内置检测函数
- 常见检测函式
  - iscolor
  - isnumber
  - isstring
  - iskeyword
  - isurl
- 判断一个值是纯数字，还是某个单位量
  - ispixel
  - ispercentage
  - isem

#### 颜色函数
- `lighten(@color, 10%);`    
- `darken(@color, 10%);`
...

#### 数学函数
- `round(1.67); // 2`
- `ceil(2.4);   // 3`
- `floor(2.6);  // 2`
- `percentage(0.5); // 50%`

### 命名空间
### 作用域
### 字符串插值
```less
@base-url: "http://assets.fnord.com";
background-image: url("@{base-url}/images/bg.png");
```

### 避免编译
### JavaScript表达式
```less
//需要用反引号
@var: `"hello".toUpperCase() + '!'`;
@height: `document.body.clientHeight`;
```
.container-fluid流式布局
