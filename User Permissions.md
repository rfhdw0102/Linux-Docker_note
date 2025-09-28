# 用户权限

## 权限

- 普通用户的权限，一般在其 HOME 目录内是不受限的，一旦出了 HOME 目录，在大多数地方，普通用户只有读和执行的权限，没有修改的权限。
- `root`  用户拥有最大的系统操作权限。

## su - 用户名   切换用户

- `-` 符号是可选的，表示是否在切换用户后加载环境变量，建议带上。
- 用户名表示要切换的用户，可以省略，省略表示切换到 root。
- 切换用户后，可以通过 `exit`命令退回上一个用户，也可以使用 `ctrl+d`
- **root下使用 rm -rf / 或 rm -rf /* 相当于windows格式化。**

## sudo 命令 但是使用时必须有授权

不建议长期使用  root  用户，避免带来系统损坏，sudo 命令可以为普通的命令授权，临时以  root  身份执行,但并不是所有的用户，都有权利使用 sudo，我们需要为普通用户配置 sudo 认证。

```powershell
切换到root 执行visudo命令，会自动通过vi编辑器打开 /etc/sudoers
在文件的最后添加:
	user ALL=(ALL)     NOPASSWD: ALL  #这个表示使用sudo命令无需输入密码
最后通过wq保存
```

## 用户/用户组

**必须在root 用户下执行：** 

- 创建用户组：   `groupadd`  用户组名

  ```shell
  groupadd itcast
  ```

- 删除用户组：   `groupdel`  用户组名

  ```shell
  groupdel itcast
  ```

- 创建用户：    `useradd [-g -d]`  用户名

  -g  指定用户的组，不指定 -g，会创建同名组并自动加入，指定 -g  需要组已经存在，如已存在同名组，必须使用 -g 。

  -d  指定用户HOME路径，不指定，HOME目录默认在：/home/用户名。

  ```shell
  useradd person   默认创建并加入person组，默认路径/home/person 
  useradd person -g itcast -d /home/newperson  
  ```

- 删除用户：    `userdel [-r]`  用户名

  -r  删除用户的HOME目录，不适用  -r  ，删除用户时，HOME 目录保留。

  ```shell
  userdel person  删除用户，用户的HOME路径不回被删除
  userdel -r person  用户及路径全删除
  ```

- 查看用户：    `id `  用户名

  用户名，被查看的用户，如果不提供则查看自身

  ```shell
  id person
  ```

- 用户加入用户组：    `usermod -aG`  用户组  用户名

  将指定用户加入指定用户组

- 从组中删除用户   `gpasswd -d`  用户名 组名

- 查看用户 `getent  passwd`

  显示内容：用户名、密码、用户id、组id、描述信息（无用）、HOME目录、执行终端（默认bash）

- 查看用户组 `getent  group`

  显示内容：组名、组认证、组id

## chmod [-R] 权限 文件或文件夹

**ls -l  可以查看文件/文件夹的权限信息**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/91c03865fe6f409fb0954f4e40c79535.png#pic_center)
`-R`  对文件夹内的全部内容应用同样的操作。

**只有文件，文件夹的所属用户或root用户可以修改**

```shell
chmod u=rwx,g=rx,o=x hello.txt   将文件权限修改为：rwxr-x--x
chmod u+rwx,g-r hello.txt
u表示所属用户权限，g表示所属用户组权限，o表示其他用户权限
chmod -R u=rwx,g=rx,o=x test  将文件夹test以及文件夹内全部内容权限设置为：rwxr-x--x
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f2c7a7915c6047399a9f21f8237896d9.png#pic_center)

```shell
chmod 751 hello.txt   将文件权限修改为：rwxr-x--x
chmod -R 751 test  将文件夹test以及文件夹内全部内容权限设置为：rwxr-x--x
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/a69df783e67b45249fa182d7a634f8c0.png#pic_center)

## chown [-R] [用户] [:] [用户组] 文件或文件夹

- `-R`  同chmod，对文件夹内全部内容应用相同规则。
- 用户，修改所属用户。
- 用户组，修改所属用户组。
- `:`  用于分隔用户和用户组。

**只有root用户可以修改**

```shell
chown root hello.txt 将hello.txt所属用户修改为root
chown :root hello.text 将hello.txt所属用户组修改为root
chown root:itcast hello.text 将hello.txt所属用户修改为root、用户组修改为itcast
chown -R root hello 将文件hello所属用户修改为root、并对文件夹内全部内容应用相同规则
```

## chgrp  修改用户所在组

```shell
修改单个文件的所属组
chgrp users document.txt
# 将 document.txt 的所属组改为 users 组
```

```shell
递归修改目录及其内容的所属组
chgrp -R admins /data/project
# 将 /data/project 目录及其中所有文件、子目录的所属组改为 admins 组

## su、su-、sudo的区别

| 命令 | 密码要求     | 环境变量     | 适用场景               | 安全性 |
| ---- | ------------ | ------------ | ---------------------- | ------ |
| su   | 目标用户密码 | 保留原环境   | 临时切换用户           | 低     |
| su - | 目标用户密码 | 加载目标环境 | 完全切换到目标用户环境 | 中     |
| sudo | 当前用户密码 | 保留原环境   | 临时提权执行命令       | 高     |

### 补充说明

- su：直接切换用户但保留原环境变量，可能导致权限混淆。
- su -：模拟目标用户完整登录环境，包括HOME和PATH变量。
- sudo：通过配置文件（/etc/sudoers）限制权限，避免直接暴露root密码。

### 安全性对比

- su需共享目标用户密码，风险最高。
- sudo仅需当前用户密码，且支持审计日志，安全性最佳。
```

# 
