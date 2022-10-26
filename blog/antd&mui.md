# antd & Material Design

## antd

- 从指引、组件、模式、资源这些分类来介绍设计语言。
- 对设计原则进行整理，总结了十大设计原则，使用文字介绍配以图片示例，使这些设计原则通俗易懂地为我们所接受。
- 对于中台产品复杂的交互逻辑，尤其是表格、表单设计做了相对深入的研究，并展示了通俗易懂的示例。
- 鼓励使用 官方 轮子

## Material Design

- 从动画效果(以下简称动效)、样式、布局、组件、模式、增长传播策略、可用性、平台、资源这些分类来介绍设计语言
- 文档没有antd完善, 用例较少
- 概念不好理解, 使用门槛较高
- 包更加小约为antd的五分之一
- 鼓励造 轮子
- form 标签没有封装, 要搭配 react-hook-form 使用

# [@auth0/auth0-react](https://www.npmjs.com/package/@auth0/auth0-react) 和 AR 权限管理

## AR 权限管理

- AR项目用到权限处理的组件:
  - Permission 组件: 通过后台返回的权限列表, 通过传入的权限字串是否在权限列表中来判断是否显示控件
  - ARMenu 菜单组件: 和 Permission 类似

## [@auth0/auth0-react](https://www.npmjs.com/package/@auth0/auth0-react)

- 一定要https
- 需要收费?

# i18n 和 react-intl

- i18n: 是国际化标准, 并非工具包
- 现在主流的国际化组件
  - [react-intl](https://formatjs.io/docs/react-intl/): AR 使用的包
  - [react-i18next](https://www.npmjs.com/package/i18next):
  - [i18next](https://www.npmjs.com/package/i18next):
  - [react-intl-universal](https://www.npmjs.com/package/react-intl-universal): 阿里在react-intl 的基础上进行封装的包
