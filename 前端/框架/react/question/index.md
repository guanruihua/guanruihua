# Question

## TypeError: this.getOptions is not a function 的解决

> 问题的出现：
>
> - 在进行 react 项目开发的时候，出现了这个错误，`TypeError: this.getOptions is not a function`，如下所示：
>
> 问题的分析及解决：
>
> - 问题的分析：这个实际上就是 `less-loader` 的版本过高，不兼容 `getOptions` 函数方法，所以需要对 `less-loader` 进行降级处理
> - 问题的解决：通过 `npm uninstall less-loader` 命令卸载原版本的 `less-loader`，然后 通过 `npm install less-loader@5.0.0` 命令下载降级版本的 less-loader，这个问题就可以解决了
