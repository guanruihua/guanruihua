# template & slots

> `<template>` 和 `<slot>` 元素创建一个可以用来灵活填充 Web组件的 shadow DOM 的模板。

```html
<link rel="stylesheet" href="../../config/css/test-body.css">
<script src="../../config/js/addWindowSize.js" defer></script>

<style>
 dl {
  margin-left: 6px;
 }

 dt {
  font-weight: bold;
  color: #217ac0;
  font-size: 110%
 }

 dt {
  font-family: Consolas, "Liberation Mono", Courier
 }

 dd {
  margin-left: 16px
 }
</style>


<template id="element-details-template">
 <style>
  details {
   font-family: "Open Sans Light", Helvetica, Arial
  }

  .name {
   font-weight: bold;
   color: #217ac0;
   font-size: 120%
  }

  h4 {
   margin: 10px 0 -8px 0;
  }

  h4 span {
   background: #217ac0;
   padding: 2px 6px 2px 6px
  }

  h4 span {
   border: 1px solid #cee9f9;
   border-radius: 4px
  }

  h4 span {
   color: white
  }

  .attributes {
   margin-left: 22px;
   font-size: 90%
  }

  .attributes h4 {
   margin-bottom: 10px;
  }

  .attributes p {
   margin-left: 16px;
   font-style: italic
  }
 </style>
 <details>
  <summary>
   <span>
    <code class="name">&lt;<slot name="element-name">NEED NAME</slot>&gt;</code>
    <i class="desc">
     <slot name="description">NEED DESCRIPTION</slot>
    </i>
   </span>
  </summary>
  <div class="attributes">
   <h4><span>Attributes</span></h4>
   <slot name="attributes">
    <p>None</p>
   </slot>
  </div>
 </details>
 <hr>
</template>

<script>
 customElements.define('element-details',
  class extends HTMLElement {
   constructor() {
    super();
    var template = document
     .getElementById('element-details-template')
     .content;
    const shadowRoot = this.attachShadow({ mode: 'open' })
     .appendChild(template.cloneNode(true));
   }
  })
</script>

<element-details>
 <span slot="element-name">slot</span>
 <span slot="description">A placeholder inside a web
  component that users can fill with their own markup,
  with the effect of composing different DOM trees
  together.</span>
 <dl slot="attributes">
  <dt>name</dt>
  <dd>The name of the slot.</dd>
 </dl>
</element-details>

<element-details>
 <span slot="element-name">template</span>
 <span slot="description">A mechanism for holding client-
  side content that is not to be rendered when a page is
  loaded but may subsequently be instantiated during
  runtime using JavaScript.</span>
</element-details>
```

![](/__assets__/img/2022-01-10-17-09-40.png)
