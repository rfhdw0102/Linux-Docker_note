# vim指令 （文本编辑器）

<img src="https://i-blog.csdnimg.cn/direct/058bbe010a7049a1a20b910741faa87f.png#pic_center" alt="在这里插入图片描述" style="zoom: 50%;" />
**命令模式**：默认的模式，可以通过键盘快捷键控制文件内容。

**输入模式**：通过命令模式进入，可以输入内容进行编辑，按esc退回命令模式。

**底线命令模式**：通过命令模式进入，可以对文件进行保存、关闭等操作。

**下面介绍一些快捷键：**
<img src="https://i-blog.csdnimg.cn/direct/94df4421d1744218969d2a21104f39ab.png#pic_center" alt="在这里插入图片描述" style="zoom: 79.5%;" />
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/dd0f15fd3a804c1394dfa90a94192434.png#pic_center)
<img src="https://i-blog.csdnimg.cn/direct/c1ab6c600ddc479088ce7686f05b68dc.png#pic_center" alt="在这里插入图片描述" style="zoom: 42%;" />

**关闭行号：set no nu**

**跳到指定行：nG**

**有个比较重要的快捷键需要说一下:[范围]s/源字符串/目标字符串/[选项]**

**范围**：指定替换的范围（不指定则默认当前行）

- n,m：从第 n 行到第 m 行（如 3,5s/... 表示 3-5 行）
- %：整个文件（等价于 1,$）

**选项**：控制替换行为

- g：全局替换（一行中所有匹配项，默认只替换第一个）

```powershell
:s/old/new/    # 替换当前行第一个 "old" 为 "new"
:s/old/new/g   # 替换当前行所有 "old" 为 "new"
:5,10s/old/new/g  # 替换 5-10 行的所有 "old" 为 "new"
:%s/old/new/g  # 替换所有行的所有 "old" 为 "new"
```

# 