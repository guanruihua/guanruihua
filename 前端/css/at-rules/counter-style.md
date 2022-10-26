# @counter-style

> `@counter-style`是一个 [CSS](https://developer.mozilla.org/en-US/CSS) [at-rule](https://developer.mozilla.org/en-US/CSS/At-rule) ，它让开发者可以自定义counter的样式。 一个 `@counter-style规则` 定义了如何把一个计数器的值转化为字符串表示。

## 语法

```css
@counter-style <counter-style-name> {
    system: <counter system>
    symbols: <counter symbols>
    additive-symbols: <additive-symbols>
    negative: <negative symbol>
    prefix: <prefix>
    suffix: <suffix>
    range: <range>
    pad: <padding>
    speak-as: <speak-as>
    fallback: <counter-style-name>
}
```

## 描述

> 每个 `@counter-style` 由一个名称标识并具有一组描述符
>
> - [system (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@counter-style/system)
>
>   指定一个算法，用于将计数器的整数值转化为字符串表示。
>
> - [negative(en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@counter-style/negative)
>
>   指定一个符号，当计数器表示的值为负的时候，把这个符号加在值的前面或后面
>
> - [prefix (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@counter-style/prefix)
>
>   指定一个符号，加在标记表示符的前面。前缀在最后阶段才会被加上，所以在计数器的最终表示中，它在[negative(en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@counter-style/negative)前。
>
> - [suffix (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@counter-style/suffix)
>
>   与[prefix (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@counter-style/prefix)类似，suffix指定一个符号，加在标记表示符的后面。
>
> - [range(en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@counter-style/range)
>
>    指定一个counter-style生效的范围，如果计数器的值不再这个范围内，那么自定义的counter-style不会生效，counter-style会后退到[fallback (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@counter-style/fallback)的style。
>
> - [pad(en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@counter-style/pad)
>
>   在你想要给标记表示符最小值时使用。比如说，你想要计数器从01开始，经过02，03，04，那么这时可以使用pad了。对于大于pad指定值的表示符，标记会恢复为normal。
>
> - [fallback (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@counter-style/fallback)
>
>   定义一个备用的系统，当自定义的系统不能使用或者计数器的值超过了定义的范围时使用。如果备用系统也不能表示计数器的值，那么备用系统的备用系统（如果有的话）将会启用。如果没有指定备用系统，或者备用系统链不能够正确表示一个值，那么最终会降为十进制样式表示。
>
> - [symbols (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@counter-style/symbols)
>
>   定义一个符号，用于标记的表示。符号可以包含字符串，图片或自定义的识别码。这个符号怎样构建标记呢？这依赖于system描述符里面所定义的算法。 举个例子，如果system的值是fixed,那么symbols属性指定的固定的N个符号，将被用来表示计数器的前N个值。用完了前N个符号后，列表里剩下的值将使用fallback定义的样式来表示。  下面的@counter-style规则使用图片而不是字符标记。
>
>   ```css
>   @counter-style winners-list { 
>     system: fixed; 
>     symbols: url(gold-medal.svg)  url(silver-medal.svg) url(bronze-medal.svg);  
>     suffix: " "; 
>   }
>   ```
>
>
>
> - [additive-symbols (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@counter-style/additive-symbols)
>
>   尽管symbols属性中指定的符号可以被system中定义的大部分算法所使用，但是一些system属性的值，比如additive，依赖于本描述符所描述的加性元组。Each additive tuple consists of a counter symbol and a non negative integer weight. 每个加性元组包含一个可数的符号和一个非负证书的权重。The additive tuples must be specified in the descending order of their weights.
>
> - [speak-as(en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@counter-style/speak-as)
>
>   定义如何在语音识别器中读出计数器样式，比如屏幕阅读器。例如基于该描述符的值，标记符号的值可以作为有序列表的数字或者字幕作为无序列表的音频提示读出。

```css
@counter-style circled-alpha {
  system: fixed;
  symbols: Ⓐ Ⓑ Ⓒ Ⓓ Ⓔ Ⓕ Ⓖ Ⓗ Ⓘ Ⓙ Ⓚ Ⓛ Ⓜ Ⓝ Ⓞ Ⓟ Ⓠ Ⓡ Ⓢ Ⓣ Ⓤ Ⓥ Ⓦ Ⓧ Ⓨ Ⓩ;
  suffix: " ";
}

.items {
   list-style: circled-alpha;
}


```

Ⓐ One
Ⓑ Two
Ⓒ Three
Ⓓ Four
Ⓔ FIve
....
...
Ⓨ Twenty Five
Ⓩ Twenty Six

27 Twenty Seven
28 Twenty Eight
29 Twenty Nine
30 Thirty
