# `font-variant-numeric`

## 值

```css
font-variant-numeric: normal;
font-variant-numeric: ordinal;
font-variant-numeric: slashed-zero;
font-variant-numeric: lining-nums;         /* <numeric-figure-values> */
font-variant-numeric: oldstyle-nums;       /* <numeric-figure-values> */
font-variant-numeric: proportional-nums;   /* <numeric-spacing-values> */
font-variant-numeric: tabular-nums;        /* <numeric-spacing-values> */
font-variant-numeric: diagonal-fractions;  /* <numeric-fraction-values> */
font-variant-numeric: stacked-fractions;   /* <numeric-fraction-values> */
font-variant-numeric: oldstyle-nums stacked-fractions;

/* Global values */
font-variant-numeric: inherit;
font-variant-numeric: initial;
font-variant-numeric: unset;
```

- normal
下列特性均不启用。

- `ordinal`
启用序数形式显示。对序号标记强制启用特殊字形，如英文中的 $1^{st}$，$2^{nd}$，$3^{rd}$，$4^{th}$ ，意大利文中的 $1^a$。等同于 OpenType 特性 ordn。
- `slashed-zero`
启用区分零显示。强制使用带有斜杠的 0；常用于区分 O 和 0。等同于 OpenType 特性 zero。
- `<numeric-figure-values>`
下列值用于控制数字样式，可用值如下：
  - lining-nums 启用内衬数字显示。使数字全部对齐到基线。等同于 OpenType 特性 lnum。
  - oldstyle-nums 启用旧式数字显示。部分数字如 3、4、7、9 会有下沉。等同于 OpenType 特性 onum。
- `<numeric-spacing-values>`
下列值用于控制数字宽度，可用值如下：
- `proportional-nums` 启用比例数字显示。使数字变成基于字形本身形状下的特定宽度表现。等同于 OpenType 特性 `pnum`
- `tabular-nums` 启用表格数字显示。使数字等宽，易于像表格那样对齐。等同于 `OpenType` 特性 `tnum`
- `<numeric-fraction-values>`
下列值用于控制分数字形，可用值如下：
  - `diagonal-fractions` 启用斜角分数显示。使分子和分母变成像下标字，并用变长的斜线分隔。等同于 `OpenType` 特性 frac。
  - `stacked-fractions` 启用标准分数显示。使分子在上，分母在下，并用水平线分隔。等同于 `OpenType` 特性 afrc。

## 兼容性

![](./__assets__/font-variant-numeric-2022-07-14-15-43-50.png)
