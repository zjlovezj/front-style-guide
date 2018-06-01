# CSS / Sass 指南

## 目录

  1. [通用约定](#css)
    - [格式](#formatting)
    - [注释](#comments)
    - [OOCSS 和 BEM](#oocss-and-bem)
    - [ID 选择器](#id-selectors)
    - [JavaScript 钩子](#javascript-hooks)
    - [边框](#border)
  1. [CSS约定](#css)
  	- [文件引用](#link)
  	- [命名-组成元素](#element)
  	- [命名-词汇规范](#word)
  	- [命名-缩写规范](#abbr)
  	- [命名-前缀规范](#prefix)
  	- [id与class](#id)
  	- [书写格式](#packaging)
  	- [规则与分号](#semicolon)
  	- [0与单位](#unit)
  	- [0与小数](#decimal)
  	- [去掉uri中引用资源的引号](#non-quotes)
  	- [HEX颜色值写法](#hex)
  	- [属性书写顺序](#order)
  	- [注释规范](#css-comment)
  	- [hack规范](#hack)
  	- [避免低效率选择器](#low-selector)
  	- [属性缩写与分拆](#override)
  	- [模块化](#css-module)
  1. [Sass](#sass)
    - [语法](#syntax)
    - [排序](#ordering-of-property-declarations)
    - [变量](#variables)
    - [Mixins](#mixins)
    - [扩展指令](#extend-directive)
    - [嵌套选择器](#nested-selectors)

<a name="css"></a>
## CSS

<a name="formatting"></a>
### 格式

* 使用 2 个空格作为缩进。
* 类名建议使用破折号代替驼峰法。如果你使用 BEM，也可以使用下划线（参见下面的 [OOCSS 和 BEM](#oocss-and-bem)）。
* 不要使用 ID 选择器。
* 在一个规则声明中应用了多个选择器时，每个选择器独占一行。
* 在规则声明的左大括号 `{` 前加上一个空格。
* 在属性的冒号 `:` 后面加上一个空格，前面不加空格。
* 规则声明的右大括号 `}` 独占一行。
* 规则声明之间用空行分隔开。

**Bad**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Good**

```css
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

<a name="comments"></a>
### 注释

* 建议使用行注释 (在 Sass 中是 `//`) 代替块注释。
* 建议注释独占一行。避免行末注释。
* 给没有自注释的代码写上详细说明，比如：
  - 为什么用到了 z-index
  - 兼容性处理或者针对特定浏览器的 hack

<a name="oocss-and-bem"></a>
### OOCSS 和 BEM

出于以下原因，我们鼓励使用 OOCSS 和 BEM 的某种组合：

  * 可以帮助我们理清 CSS 和 HTML 之间清晰且严谨的关系。
  * 可以帮助我们创建出可重用、易装配的组件。
  * 可以减少嵌套，降低特定性。
  * 可以帮助我们创建出可扩展的样式表。

**OOCSS**，也就是 “Object Oriented CSS（面向对象的CSS）”，是一种写 CSS 的方法，其思想就是鼓励你把样式表看作“对象”的集合：创建可重用性、可重复性的代码段让你可以在整个网站中多次使用。

参考资料：

  * Nicole Sullivan 的 [OOCSS wiki](https://github.com/stubbornella/oocss/wiki)
  * Smashing Magazine 的 [Introduction to OOCSS](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/)

**BEM**，也就是 “Block-Element-Modifier”，是一种用于 HTML 和 CSS 类名的_命名约定_。BEM 最初是由 Yandex 提出的，要知道他们拥有巨大的代码库和可伸缩性，BEM 就是为此而生的，并且可以作为一套遵循 OOCSS 的参考指导规范。

  * CSS Trick 的 [BEM 101](https://css-tricks.com/bem-101/)
  * Harry Roberts 的 [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

**示例**

```html
<article class="listing-card listing-card--featured">

  <h1 class="listing-card__title">Adorable 2BR in the sunny Mission</h1>

  <div class="listing-card__content">
    <p>Vestibulum id ligula porta felis euismod semper.</p>
  </div>

</article>
```

```css
.listing-card { }
.listing-card--featured { }
.listing-card__title { }
.listing-card__content { }
```

  * `.listing-card` 是一个块（block），表示高层次的组件。
  * `.listing-card__title` 是一个元素（element），它属于 `.listing-card` 的一部分，因此块是由元素组成的。
  * `.listing-card--featured` 是一个修饰符（modifier），表示这个块与 `.listing-card` 有着不同的状态或者变化。

<a name="id-selectors"></a>
### ID 选择器

在 CSS 中，虽然可以通过 ID 选择元素，但大家通常都会把这种方式列为反面教材。ID 选择器给你的规则声明带来了不必要的高[优先级](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity)，而且 ID 选择器是不可重用的。

想要了解关于这个主题的更多内容，参见 [CSS Wizardry 的文章](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/)，文章中有关于如何处理优先级的内容。

<a name="javascript-hooks"></a>
### JavaScript 钩子

避免在 CSS 和 JavaScript 中绑定相同的类。否则开发者在重构时通常会出现以下情况：轻则浪费时间在对照查找每个要改变的类，重则因为害怕破坏功能而不敢作出更改。

我们推荐在创建用于特定 JavaScript 的类名时，添加 `.js-` 前缀：

```html
<button class="btn btn-primary js-request-to-book">Request to Book</button>
```

<a name="border"></a>
### 边框

在定义无边框样式时，使用 `0` 代替 `none`。

**Bad**

```css
.foo {
  border: none;
}
```

**Good**

```css
.foo {
  border: 0;
}
```

<a name="sass"></a>


## CSS约定

<a name="link"></a>
### 文件引用

* 一律使用link的方式调用外部样式
* 不允许在页面中使用 `<style>` 块；
* 不允许在 `<style>` 块中使用 `@import`；
* 不允许使用 `style` 属性写行内样式。

> 一般情况下，在页面中只允许使用 `<link />` 标签来引用CSS文件，

<a name="element"></a>
### 命名-组成元素

* 命名必须由单词、中划线①或数字组成；
* 不允许使用拼音（约定俗成的除外，如：youku, baidu），尤其是缩写的拼音、拼音与英文的混合。

不推荐：

	.xiangqing { sRules; }
	.news_list { sRules; }
	.zhuti { sRules; }

推荐：

	.detail { sRules; }
	.news-list { sRules; }
	.topic { sRules; }

> ①我们使用中划线 “-” 作为连接字符，而不是下划线 "—"。
>
> 我们知道2种方式都有不少支持者，但 "-" 能让你少按一次shift键，并且更符合CSS原生语法，所以我们只选一种目前业内普遍使用的方式

<a name="word"></a>
### 命名-词汇规范

* 不依据表现形式来命名；
* 可根据内容来命名；
* 可根据功能来命名。

不推荐：

	left, right, center, red, black

推荐：

	nav, aside, news, type, search

<a name="abbr"></a>
### 命名-缩写规范

* 保证缩写后还能较为清晰保持原单词所能表述的意思；
* 使用业界熟知的或者约定俗成的。

不推荐：

	navigation   =>  navi
	header       =>  head
	description  =>  des

推荐：

	navigation   =>  nav
	header       =>  hd
	description  =>  desc

<a name="prefix"></a>
### 命名-前缀规范

前缀|说明|示例
---|---|---|
g-|全局通用样式命名，前缀g全称为global，一旦修改将影响全站样式|g-mod
m-|模块命名方式|m-detail
ui-|组件命名方式|ui-selector
js-|所有用于纯交互的命名，不涉及任何样式规则。JSer拥有全部定义权限|js-switch

* 选择器必须是以某个前缀开头

不推荐：

	.info { sRules; }
	.current { sRules; }
	.news { sRules; }

> 因为这样将给我们带来不可预知的管理麻烦以及沉重的历史包袱。你永远也不会知道哪些样式名已经被用掉了，如果你是一个新人，你可能会遭遇，你每定义个样式名，都有同名的样式已存在，然后你只能是换样式名或者覆盖规则。

推荐：

	.m-detail .info { sRules; }
	.m-detail .current { sRules; }
	.m-detail .news { sRules; }

> 所有的选择器必须是以 g-, m-, ui- 等有前缀的选择符开头的，意思就是说所有的规则都必须在某个相对的作用域下才生效，尽可能减少全局污染。

js- 这种级别的className完全交由JSer自定义，但是命名的规则也可以保持跟重构一致，比如说不能使用拼音之类的

<a name="id"></a>
### id与class

重构工程师只允许使用class（因历史原因及大家的习惯做出妥协）。

<a name="packaging"></a>
### 书写格式

* 选择器与大括号之间保留一个空格；
* 分号之后保留一个空格；
* 逗号之后保留一个空格；
* 所有规则需换行；
* 多组选择器之间需换行。

不推荐：

	main{
		display:inline-block;
	}
	h1,h2,h3{
		margin:0;
		background-color:rgba(0,0,0,.5);
	}

推荐：

	main {
		display: inline-block;
	}
	h1,
	h2,
	h3 {
		margin: 0;
		background-color: rgba(0, 0, 0, .5);
	}

<a name="semicolon"></a>
### 规则与分号

每条规则结束后都必须加上分号

不推荐：

	body {
		margin: 0;
		padding: 0;
		font-size: 14px
	}

推荐：

	body {
		margin: 0;
		padding: 0;
		font-size: 14px;
	}

<a name="unit"></a>
### 0与单位

如果属性值为0，则不需要为0加单位

不推荐：

	body {
		margin: 0px;
		padding: 0px;
	}

推荐：

	body {
		margin: 0;
		padding: 0;
	}

<a name="decimal"></a>
### 0与小数

如果是0开始的小数，前面的0可以省略不写

不推荐：

	body {
		opacity: 0.6;
		text-shadow: 1px 1px 5px rgba(0, 0, 0, 0.5);
	}

推荐：

	body {
		opacity: .6;
		text-shadow: 1px 1px 5px rgba(0, 0, 0, .5);
	}

<a name="non-quotes"></a>
### 去掉uri中引用资源的引号

不要在url()里对引用资源加引号

不推荐：

	body {
		background-image: url("sprites.png");
	}
	@import url("global.css");

推荐：

	body {
		background-image: url(sprites.png);
	}
	@import url(global.css);

<a name="hex"></a>
### HEX颜色值写法

* 将所有的颜色值小写；
* 可以缩写的缩写至3位。

不推荐：

	body {
		background-color: #FF0000;
	}

推荐：

	body {
		background-color: #f00;
	}

<a name="order"></a>
### 属性书写顺序

* 遵循先布局后内容的顺序。

```
.g-box {
　　　display: block;
　　　float: left;
　　　width: 500px;
　　　height: 200px;
　　　margin: 10px;
　　　padding: 10px;
　　　border: 10px solid;
　　　background: #aaa;
　　　color: #000;
　　　font: 14px/1.5 sans-serif;
}
```

> 这个应该好理解，比如优先布局，我们知道布局属性有 display, float, overflow 等等；内容次之，比如 color, font, text-align 之类。

* 组概念。

拿上例的代码来说，如果我们还需要进行定位及堆叠，规则我们可以改成如下：

```
.g-box {
　　　display: block;
　　　position: relative;
　　　z-index: 2;
　　　top: 10px;
　　　left: 100px;
　　　float: left;
　　　width: 500px;
　　　height: 200px;
　　　margin: 10px;
　　　padding: 10px;
　　　border: 10px solid;
　　　background: #aaa;
　　　color: #000;
　　　font: 14px/1.5 sans-serif;
}
```

> 从代码中可以看到，我们直接将z-index, top, left 紧跟在 position 之后，因为这几个属性其实是一组的，如果去掉position，则后3条属性规则都将失效。

* 私有属性在前标准属性在后

```
.g-box {
　　　-webkit-box-shadow: 1px 1px 5px rgba(0, 0, 0, .5);
　　　-moz-box-shadow: 1px 1px 5px rgba(0, 0, 0, .5);
　　　-o-box-shadow: 1px 1px 5px rgba(0, 0, 0, .5);
　　　box-shadow: 1px 1px 5px rgba(0, 0, 0, .5);
}
```

> 当有一天你的浏览器升级后，可能不再支持私有写法，那么这时写在后面的标准写法将生效，避免无法向后兼容的情况发生。

<a name="css-comment"></a>
### 注释规范

保持注释内容与星号之间有一个空格的距离

**普通注释（单行）**

	/* 普通注释 */

**区块注释**

	/**
	 * 模块: m-detail
	 * 描述：酒店详情模块
	 * 应用：page detail, info and etc...etc
	 */

> 有特殊作用的规则一定要有注释说明
> 应用了高级技巧的地方一定要注释说明

<a name="hack"></a>
### hack规范

* 尽可能的减少对Hack的使用和依赖，如果在项目中对Hack的使用太多太复杂，对项目的维护将是一个巨大的挑战；
* 使用其它的解决方案代替Hack思路；
* 如果非Hack不可，选择稳定且常用并易于理解的。

```
.test {
　　　color: #000;       /* For all */
　　　color: #111\9;     /* For all IE */
　　　color: #222\0;     /* For IE8 and later, Opera without Webkit */
　　　color: #333\9\0;   /* For IE8 and later */
　　　color: #444\0/;    /* For IE8 and later */
　　　*color: #666;      /* For IE7 and earlier */
　　　_color: #777;      /* For IE6 and earlier */
}
```

* 严谨且长期的项目，针对IE可以使用条件注释作为预留Hack或者在当前使用

IE条件注释语法：

	<!--[if <keywords>? IE <version>?]>
	<link rel="stylesheet" href="*.css" />
	<![endif]-->

语法说明：

```
<keywords>
if条件共包含6种选择方式：是否、大于、大于或等于、小于、小于或等于、非指定版本
是否：指定是否IE或IE某个版本。关键字：空
大于：选择大于指定版本的IE版本。关键字：gt（greater than）
大于或等于：选择大于或等于指定版本的IE版本。关键字：gte（greater than or equal）
小于：选择小于指定版本的IE版本。关键字：lt（less than）
小于或等于：选择小于或等于指定版本的IE版本。关键字：lte（less than or equal）
非指定版本：选择除指定版本外的所有IE版本。关键字：!
```

```
<version>
目前的常用IE版本为6.0及以上，推荐酌情忽略低版本，把精力花在为使用高级浏览器的用户提供更好的体验上，另从IE10开始已无此特性
```

<a name="low-selector"></a>
### 避免低效率选择器

* 避免类型选择器

不允许：

	div#doc { sRules; }
	li.first { sRules; }

应该：

	#doc { sRules; }
	.first { sRules; }

> CSS选择器是由右到左进行解析的，所以 div#doc 本身并不会比 #doc 更快

* 避免多id选择器

不允许：

	#xxx #yyy { sRules; }

应该：

	#yyy { sRules; }

<a name="override"></a>
### 属性缩写与分拆

* 无继承关系时，使用缩写

不推荐：

```
body {
　　　margin-top: 10px;
　　　margin-right: 10px;
　　　margin-bottom: 10px;
　　　margin-left: 10px;
}
```

推荐：

```
body {
　　　margin: 10px;
}
```

* 存在继承关系时，使用分拆方式

不推荐：

```
.m-detail {
　　　font: bold 12px/1.5 arial, sans-serif;
}
.m-detail .info {
　　　font: normal 14px/1.5 arial, sans-serif;
}
```

要避免错误的覆盖：

```
.m-detail .info {
　　　font: 14px sans;
}
```

> 如果你只是想改字号和字体，然后写成了上面这样，这是错误的写法，因为 `font` 复合属性里的其他属性将会被重置为 user agent 的默认值，比如 `font-weight` 就会被重置为 `normal`。

推荐：

```
.m-detail {
　　　font: bold 12px/1.5 arial, sans-serif;
}
.m-detail .info {
　　　font-weight: normal;
　　　font-size: 14px;
}
```

> 在存在继承关系的情况下，只将需要变更的属性重定义，不进行缩写，避免不需要的重写的属性被覆盖定义

* 根据规则条数选择缩写和拆分

不推荐：

```
.m-detail {
　　　border-width: 1px;
　　　border-style: solid;
　　　border-color: #000 #000 #f00;
}
```

推荐：

```
.m-detail {
　　　border: 1px solid #000;
　　　border-bottom-color: #f00;
}
```

<a name="css-module"></a>
### 模块化

* 每个模块必须是一个独立的样式文件，文件名与模块名一致；
* 模块样式的选择器必须以模块名开头以作范围约定；

假定有一个模块如前文 [HTML模块化](#html-module)，那么 `m-detail.scss` 的写法大致如下：

	.m-detail {
		background: #fff;
		color: #333;
		&-hd {
			padding: 5px 10px;
			background: #eee;
			.title {
				background: #eee;
			}
		}
		&-bd {
			padding: 10px;
			.info {
				font-size: 14px;
				text-indent: 2em;
			}
		}
		&-ft {
			text-align: center;
			.more {
				color: blue;
			}
		}
	}

编译之后代码如下：

	.m-detail {
		background: #fff;
		color: #333;
	}
	.m-detail-hd {
    	padding: 5px 10px;
    	background: #eee;
    }
    .m-detail-hd .title {
    	background: #eee;
    }
	.m-detail-bd {
		padding: 10px;
	}
    .m-detail-bd .info {
		font-size: 14px;
		text-indent: 2em;
	}
	.m-detail-ft {
		text-align: center;
	}
    .m-detail-ft .more {
    	color: blue;
    }

> 任何超过3级的选择器，需要思考是否必要，是否有无歧义的，能唯一命中的更简短的写法


## Sass

<a name="syntax"></a>
### 语法

* 使用 `.scss` 的语法，不使用 `.sass` 原本的语法。
* CSS 和 `@include` 声明按照以下逻辑排序（参见下文）

<a name="ordering-of-property-declarations"></a>
### 属性声明的排序

1. 属性声明

    首先列出除去 `@include` 和嵌套选择器之外的所有属性声明。

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      // ...
    }
    ```

2. `@include` 声明

    紧随后面的是 `@include`，这样可以使得整个选择器的可读性更高。

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);
      // ...
    }
    ```

3. 嵌套选择器

    如果有必要用到嵌套选择器，把它们放到最后，在规则声明和嵌套选择器之间要加上空白，相邻嵌套选择器之间也要加上空白。嵌套选择器中的内容也要遵循上述指引。

    ```scss
    .btn {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);

      .icon {
        margin-right: 10px;
      }
    }
    ```

<a name="variables"></a>
### 变量

变量名应使用破折号（例如 `$my-variable`）代替 camelCased 和 snake_cased 风格。对于仅用在当前文件的变量，可以在变量名之前添加下划线前缀（例如 `$_my-variable`）。

<a name="mixins"></a>
### Mixins

为了让代码遵循 DRY 原则（Don't Repeat Yourself）、增强清晰性或抽象化复杂性，应该使用 mixin，这与那些命名良好的函数的作用是异曲同工的。虽然 mixin 可以不接收参数，但要注意，假如你不压缩负载（比如通过 gzip），这样会导致最终的样式包含不必要的代码重复。

<a name="extend-directive"></a>
### 扩展指令

应避免使用 `@extend` 指令，因为它并不直观，而且具有潜在风险，特别是用在嵌套选择器的时候。即便是在顶层占位符选择器使用扩展，如果选择器的顺序最终会改变，也可能会导致问题。（比如，如果它们存在于其他文件，而加载顺序发生了变化）。其实，使用 @extend 所获得的大部分优化效果，gzip 压缩已经帮助你做到了，因此你只需要通过 mixin 让样式表更符合 DRY 原则就足够了。

<a name="nested-selectors"></a>
### 嵌套选择器

**请不要让嵌套选择器的深度超过 3 层！**

```scss
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

当遇到以上情况的时候，你也许是这样写 CSS 的：

* 与 HTML 强耦合的（也是脆弱的）*—或者—*
* 过于具体（强大）*—或者—*
* 没有重用


再说一遍: **永远不要嵌套 ID 选择器！**

如果你始终坚持要使用 ID 选择器（劝你三思），那也不应该嵌套它们。如果你正打算这么做，你需要先重新检查你的标签，或者指明原因。如果你想要写出风格良好的 HTML 和 CSS，你是**不**应该这样做的。
