用于同时打印进程的PID、oom_score_adj值和进程名称：
```
for pid in $(ls /proc | grep '^[0-9]'); do echo "PID: $pid - oom_score_adj: $(cat /proc/$pid/oom_score_adj 2>/dev/null) - Name: $(cat /proc/$pid/cmdline 2>/dev/null | tr '\0' ' ' | cut -d ' ' -f1)"; done
```
这个命令的工作流程如下：

遍历所有PID目录：for pid in $(ls /proc | grep '^[0-9]') 循环通过 /proc 目录下的所有数字目录，这些目录以进程的PID命名。

读取oom_score_adj：对每个PID，使用 cat /proc/$pid/oom_score_adj 读取其 oom_score_adj 值。

读取进程名称：使用 cat /proc/$pid/cmdline 读取进程的命令行内容。由于命令行参数以空字符 (\0) 分隔，我们使用 tr '\0' ' ' 将它们转换为以空格分隔，然后使用 cut -d ' ' -f1 获取第一个参数作为进程名称。

打印信息：对于每个进程，打印其PID、oom_score_adj值和进程名称。

请注意，这个命令在打印进程名称时假设第一个命令行参数是有意义的名称。对于一些服务或系统进程，这可能会显示为路径或不完整的名称。此外，由于访问 /proc 文件系统可能需要相应的权限，因此在没有足够权限的情况下，某些文件的内容可能无法被读取。
