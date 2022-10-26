# `isolation`

- 决定一个元素是否要创建一个新的层叠上下文

```html
<section>
  <h1>
    <span>Elaborate title</span>
  </h1>
</section>

<article>
  This is a <span class="tooltip-parent"><u>tooltip</u>
    <span class="tooltip">
      Lorem ipsum dolor sit amet, consectetur adipiscing elit. In ac vulputate augue, sed consequat velit. Praesent rhoncus viverra ullamcorper.
    </span>

  </span> that is in a hovered state for your convinience.

  <p> Lorem ipsum dolor sit amet, consectetur adipiscing elit. In ac vulputate augue, sed consequat velit. Praesent rhoncus viverra ullamcorper.
  </p>
  
  <p>
    In eu erat commodo enim varius feugiat ac vitae arcu. Aenean id varius justo. Curabitur eget mollis nibh. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. Vestibulum eu sapien sed lorem imperdiet auctor sit amet in felis. Donec non sem vel turpis laoreet pretium vitae sit amet odio. In sit amet fermentum arcu. Ut fringilla risus ac venenatis euismod. Donec non eleifend purus. Suspendisse a aliquet tellus. Maecenas bibendum commodo augue et porta.
  </p>
</article>
```

```css
*,
*::before {
  box-content: content-box;
}

h1 {
  isolation: isolate;
  font-size: 64px;
  text-transform: uppercase;
  margin: 0;
  position: relative;
  display: inline-block;
  margin-bottom: 0.5em;
}

h1 span {
  position: relative;
  z-index: 999999;
  padding: 0 0.25em;
}

h1::before {
  content: "";
  width: 100%;
  height: 20px;
  background-color: goldenrod;
  opacity: 0.6;
  z-index: 1;
  position: absolute;
  top: 60%;
  left: 0;
  transform: skew(-45deg, 0);
  transform-origin: 50% 50%;
}

body {
  padding: 3rem 2rem;
  width: 768px;
  margin: 0 auto;
  line-height: 1.5;
  color: #111111;
}

u {
  color: darkgoldenrod;
  font-weight: 600;
}

.tooltip-parent {
  
  position: relative;
  display: inline-block;
}

.tooltip {
  isolation: isolate;
  width: 300px;
  z-index: 1;
  position: absolute;
  top: 0;
  left: 0;
  transform: translate3d(0, calc(-100% - 16px), 0);
  background-color: #ffffff;
  padding: 0.5rem;
  border: 2px solid darkgoldenrod;
  border-radius: 4px;
}

.tooltip::before {
  content: "";
  width: 0; 
  height: 0; 
  border-left: 12px solid transparent;
  border-right: 12px solid transparent;
  border-top :12px solid darkgoldenrod;
  position: absolute;
  top: 100%;
  margin-top: 1px;
  left:12px;
  z-index: -1;
}

```

<section>
  <h1>
    <span>Elaborate title</span>
  </h1>
</section>

<article>
  This is a <span class="tooltip-parent"><u>tooltip</u>
    <span class="tooltip">
      Lorem ipsum dolor sit amet, consectetur adipiscing elit. In ac vulputate augue, sed consequat velit. Praesent rhoncus viverra ullamcorper.
    </span>

  </span> that is in a hovered state for your convinience.

  <p> Lorem ipsum dolor sit amet, consectetur adipiscing elit. In ac vulputate augue, sed consequat velit. Praesent rhoncus viverra ullamcorper.
  </p>
  
  <p>
    In eu erat commodo enim varius feugiat ac vitae arcu. Aenean id varius justo. Curabitur eget mollis nibh. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. Vestibulum eu sapien sed lorem imperdiet auctor sit amet in felis. Donec non sem vel turpis laoreet pretium vitae sit amet odio. In sit amet fermentum arcu. Ut fringilla risus ac venenatis euismod. Donec non eleifend purus. Suspendisse a aliquet tellus. Maecenas bibendum commodo augue et porta.
  </p>
</article>

<style>
*,
*::before {
  box-content: content-box;
}

h1 {
  isolation: isolate;
  font-size: 64px;
  text-transform: uppercase;
  margin: 0;
  position: relative;
  display: inline-block;
  margin-bottom: 0.5em;
}

h1 span {
  position: relative;
  z-index: 999999;
  padding: 0 0.25em;
}

h1::before {
  content: "";
  width: 100%;
  height: 20px;
  background-color: goldenrod;
  opacity: 0.6;
  z-index: 1;
  position: absolute;
  top: 60%;
  left: 0;
  transform: skew(-45deg, 0);
  transform-origin: 50% 50%;
}

body {
  padding: 3rem 2rem;
  width: 768px;
  margin: 0 auto;
  line-height: 1.5;
  color: #111111;
}

u {
  color: darkgoldenrod;
  font-weight: 600;
}

.tooltip-parent {
  
  position: relative;
  display: inline-block;
}

.tooltip {
  isolation: isolate;
  width: 300px;
  z-index: 1;
  position: absolute;
  top: 0;
  left: 0;
  transform: translate3d(0, calc(-100% - 16px), 0);
  background-color: #ffffff;
  padding: 0.5rem;
  border: 2px solid darkgoldenrod;
  border-radius: 4px;
}

.tooltip::before {
  content: "";
  width: 0;
  height: 0;
  border-left: 12px solid transparent;
  border-right: 12px solid transparent;
  border-top :12px solid darkgoldenrod;
  position: absolute;
  top: 100%;
  margin-top: 1px;
  left:12px;
  z-index: -1;
}
</style>

## 兼容性

![](./__assets__/isolation-2022-07-14-14-59-35.png)
