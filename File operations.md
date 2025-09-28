# 文件/文件夹

## mkdir [-p] linux路径 （创建文件夹）

 参数必填（要创建文件夹的路径）、-p 选填（自动创建不存在的目录，适用于连续创建多层级的目录）

```powershell
mkdir test
mkdir -p test/demo 如果test不存在，必须有-p
```

##  touch linux路径 （创建文件）

无选项，参数必填，表示要创建的文件路径。

```powershell
touch test/test.txt
```

## cp [-r] 参数1 参数2（复制文件/文件夹）

- -r 可选，用于复制文件夹使用，表示递归。
- 参数1，linux，表示被复制的文件/文件夹。
- 参数2，linux，表示要复制去的地方。

## mv 参数1 参数2 （移动文件/文件夹）

- 参数1，linux，表示被移动的文件/文件夹。
- 参数2，linux，表示要移动去的地方。

**特殊用法：**

```powershell
mv test.txt test2.txt   当test2.txt不存在时，test.txt会改名为test2.txt
```

## rm [-r -f] 参数1 参数2 ....... 参数n  (删除文件/文件夹)

- 同 cp 一样，-r 用于删除文件夹使用。
- -f 表示强制删除（不会弹出确认信息）--只有管理员删除内容时会弹出提示，所以一般用户用不到 -f 。
- 参数表示要删除的文件/文件名路径，用空格隔开。

## cat linux路径            more linux路径 （都是查看文件内容）

无选项，参数必填，表示被查看的文件路径
**不同：**
cat 直接将所以内容显示出来。
more 支持翻页，如果文件内容过多，可以一页一页的展示，通过 `空格` 翻页，按 `q` 退出。

## find 查找文件

**用法1：find 起始路径   -name  “被查找文件名”  （按照文件名查找）**  
为了拥有最大权限，可以切换到root用户以获取管理员权限。或者使用**sudo**指令

```powershell
find / -name "test"
# 也可以使用通配符
find / -name "*test"
find / -name "test*"
find / -name "*test*"
```

**用法2：find 起始路径 -size + | - n[kMG] （按文件大小查找）**

`+ -`  表示大于和小于、`n`  表示大小数字、`kMG`  表示大小单位，**k 是小写**

```powershell
find / -size -10k  查找小于10kb的文件
find / -size +100M  查找大于100Mb的文件
find / -size +1G  查找大于1Gb的文件
```

## grep [-n] 关键字 文件路径 （从文件中通过关键字过滤文件行）

- `-n` 可选，表示在结果中显示匹配的行的行号。
- 关键字，必填，表示过滤的关键字，带有空格或其它特殊符号，建议使用`“ ”`将关键字包围起来。
- 文件路径，必填，表示要过滤内容的文件路径，**可作为内容输入端口  (管道符里使用)**。

```powershell
grep fine test.txt
grep -n "fine ok" test.txt   # 最好带上 双引号“”
```

**另一个重点用法： -r 递归搜索**

```powershell
grep -r "function" ./src  # 在 ./src 目录及其子目录的所有文件中搜索 "function"
```

## wc [-c  -m  -l  -w] 文件路径 （统计文件的信息）

- 直接  `wc  文件路径`  会返回 行数、单词数、字节数

- `-c`  统计 `bytes` 数量
- `-m`  统计字符数量
- `-l`  统计行数
- `-w`  统计单词数量
- 文件路径，被统计的文件，**可作为内容输入端口**。

## tail [-f   -num]  Linux路径     head [-num]  Linux路径

`tail ` 查看文件尾部内容，跟踪文件的最新更改。

- -f  表示持续跟踪，退出跟踪按 `ctrl+c` 。
- -num  表示查看尾部多少行，不填默认10行。

```powershell
tail test.txt
tail -5 test.txt
tail -f test.txt
```

`head ` 查看文件头部

```powershell
head test.txt
head -5 test.txt
```

## sort [-r -R] 文件路径

```powershell
sort test.txt   将内容按ASCII排序
sort -r test.txt   与上面相反
sort -R test.txt   随机
sort -n test.txt   数字按大小排序
sort -o test.txt1 test.txt  新建test.txt1将test.txt排序后内容复制到test.txt1中
```

## cut

 **cut -c N** 

```powershell
echo "hello" | cut -c 2      # 输出 'e'（第2个字符）
echo "hello" | cut -c 2-4    # 输出 'ell'（第2到4个字符）
echo "hello" | cut -c 1,3,5  # 输出 'hlo'（第1、3、5个字符）
```

 **cut -d '分隔符'  -f N [文件]**

```powershell
# 提取 `/etc/passwd` 的第1列（用户名）
cut -d ':' -f 1 /etc/passwd
# 提取 CSV 文件的第2列（假设分隔符是 `,`）
cut -d ',' -f 2 data.csv
# 提取多个列（如第1和第3列）
cut -d ':' -f 1,3 /etc/passwd
# 提取第2列之后的所有列
cut -d ':' -f 2- /etc/passwd
```

# 
