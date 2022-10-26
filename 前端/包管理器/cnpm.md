# cnpm

## 问题

### cnpm : 无法加载文件 C:\Users\AppData\Roaming\npm\cnpm.ps1，因为在此系统上禁止运行脚本

> 提示信息：系统禁止运行脚本。这时我们需要更改系统的执行策略，使得脚本能够在我们的系统上执行。
过程如下：
>
> 首先，以管理员身份运行 Windows PowerShell
> 然后，输入命令 set-ExecutionPolicy RemoteSigned 更改执行策略。
> 最后，输入 Y, 执行更改策略。
