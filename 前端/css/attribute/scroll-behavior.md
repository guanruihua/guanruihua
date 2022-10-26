# scroll-behavior

> - auto
>   - 滚动框立即滚动。
>
> - smooth
>   - 滚动框通过一个用户代理预定义的时长、使用预定义的时间函数，来实现平稳的滚动，用户代理应遵循其平台的约定，如果有的话。



```css
/* Keyword values */
scroll-behavior: auto;
scroll-behavior: smooth;

/* Global values */
scroll-behavior: inherit;
scroll-behavior: initial;
scroll-behavior: unset;
```

![](scroll-behavior.assets/GIF 2021-6-7 15-30-05-1623051086791.gif)

## 栗子

```html
<style>
	a {
		display: inline-block;
		width: 50px;
		text-decoration: none;
	}

	nav,
	scroll-container {
		display: block;
		margin: 0 auto;
		text-align: center;
	}

	nav {
		width: 339px;
		padding: 5px;
		border: 1px solid black;
	}

	scroll-container {
		display: block;
		width: 350px;
		height: 200px;
		overflow-y: scroll;
		scroll-behavior: smooth;
	}

	scroll-page {
		display: flex;
		align-items: center;
		justify-content: center;
		height: 100%;
		font-size: 5em;
	}
</style>

<body>
	<nav>
		<a href="#page-1">1</a>
		<a href="#page-2">2</a>
		<a href="#page-3">3</a>
	</nav>
	<scroll-container>
		<scroll-page id="page-1">1</scroll-page>
		<scroll-page id="page-2">2</scroll-page>
		<scroll-page id="page-3">3</scroll-page>
	</scroll-container>
</body>
```





## 兼容性

![image-20210607153219790](scroll-behavior.assets/image-20210607153219790-1623051142758.png)