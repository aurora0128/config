



# 1.什么是Tmux？

  **在与服务器进行ssh连接的时候，有时会中断了连接,导致当前正在做的任务被迫停止.且任务状态丢失.而tmux可以作为一个进程保存在服务器中,下回登录时可以还原上回的工作状态<font color=red>且其还是一款非常好用的终端复用神器</font>**

可以通过自定义的按键在多个终端中自由移动,不需要在抬手移动鼠标(*十分的保护手腕但废手指*)

在开始之前,我们先了解下Tmux中一个**很重要的概念**:

## session、windows、pane

**他们之间的关系可以通过下面这张图来理清**

![54a371de2c15ba0d900b8d3db1af566](54a371de2c15ba0d900b8d3db1af566.jpg)

总而言之：**每一个session中可以有多个window，一个window下还可以有多个pane。session是tmux中最大单位**

# 2.使用Tmux

一个正常的页面中可以看到以下几个信息:

![cded116486b46eb8ba0ea256edfbba3](cded116486b46eb8ba0ea256edfbba3.jpg)

## 2.1安装Tmux



在Ubuntu中通过

~~~bash
sudo apt-get install tmux
~~~

来安装Tmux，安装完成后输入

~~~bash
tmux -V
~~~

若显示版本号则表示安装成功。

## 2.2 创建第一个Tmux窗口

可以使用

~~~bash
tmux kill-server #关闭tmux
~~~



# Session



可以直接输入tmux,使用默认参数创建一个session

~~~bash
tmux #此时默认session的名字为0
~~~

或者使用

~~~bash
tmux new -s target_session_name #创建一个新的名为target_session_name的session
~~~

| 命令                                           | 效果                                |
| ---------------------------------------------- | ----------------------------------- |
| tmux kill-session -t target_session_name       | 关闭一个名为target_session的session |
| tmux ls                                        | 列出当前所有session                 |
| tmux switch -t target_session_name             | 切换到指定的session                 |
| tmux rename-session -t old_session new_session | 重新命名session                     |
| tmux a -t target_session_name                  | 进入(恢复)指定session               |

**当我们进入Tmux内部的时候,大多数都是用快捷键来进行操作.在tmux中有一个很重要的按键叫做:前缀键**(目的是让tmux知道你在对他进行操作.)

默认是ctrl+b 下文对这个按键简称pre

| 指令  | 效果              |
| ----- | ----------------- |
| pre+? | 显示所有快捷键    |
| pre+d | 挂起当前session() |
| pre+s | 显示session列表   |
| pre+: | 进入命令行模式    |
| pre+[ | 进入copy-mode     |

# Window

**一个session下可以有多个window**

创建一个名为new_window_name的window

~~~bash
tmux new-window -n new_window_name
~~~

重命名

~~~bash
tmux rename-window -t old_name new_name
~~~



| 指令    | 效果                                   |
| ------- | -------------------------------------- |
| pre+c   | 新建窗口                               |
| pre+&   | 关闭当前窗口                           |
| pre+0~9 | 切换到指定窗口                         |
| pre+p   | 切换到上一窗口                         |
| pre+n   | 切换到下一窗口                         |
| pre+w   | 打开窗口列表，用于且切换窗口           |
| pre+,   | 重命名当前窗口                         |
| pre+.   | 修改当前窗口编号（适用于窗口重新排序） |



# Pane

**一个window可以有多个pane**

| 指令       | 效果                                                         |
| ---------- | ------------------------------------------------------------ |
| pre+"      | 当前面板上下一分为二，下侧新建面板                           |
| pre+%      | 当前面板左右一分为二，右侧新建面板                           |
| pre+x      | 关闭当前面板（关闭前需输入y or n确认）                       |
| pre+z      | 最大化当前面板，再重复一次按键后恢复正常（v1.8版本新增）     |
| pre+!      | 将当前面板移动到新的窗口打开（原窗口中存在两个及以上面板有效） |
| pre+;      | 切换到最后一次使用的面板                                     |
| pre+q      | 显示面板编号，在编号消失前输入对应的数字可切换到相应的面板   |
| pre+{      | 向前置换当前面板                                             |
| pre+}      | 向后置换当前面板                                             |
| pre+方向键 | 移动光标切换面板前缀                                         |



# 3.Tmux配置

在home目录下创建一个.tmux.conf文件

~~~bash
touch .tmux.conf
~~~

## 设置为vi的复制粘贴样式

> \# mode to vim
>
> set-option -g status-keys vi  #将当前按键的模式设置为vi的风格而不是emacs的风格
>
> bind -T copy-mode-vi v send-keys -X begin-selection # vim的选中
>
> bind -T copy-mode-vi y send-keys -X copy-selection #vim的copy
>
> bind-key p pasteb # vim的粘贴

<font color=blue>具体的为:**使用  pre+[  进入copy-mode 使用v可以选择文本 y进行复制 pre+p进行粘贴**</font>

> #更换前置键 为ctrl+a
>
> set-option -g prefix C-a
>
> unbind-key C-b
>
> bind-key C-a send-prefix

## 选择pane

> #通过pre+hjkl进行左上下右选择pane
>
> bind h select-pane -L
>
> bind j select-pane -D
>
> bind k select-pane -U
>
> bind l select-pane -R
>
> #通过alt+上下左右方向键选择pane
>
> bind -n M-Left select-pane -L
>
> bind -n M-Right select-pane -R
>
> bind -n M-Up select-pane -U
>
> bind -n M-Down select-pane -D

<font color=red>**-n为不使用pre,M-为alt键**</font>

## 调整pane的宽度



> #通过 **> < - +** 来调整上下左右的宽度
>
> bind -r < resize-pane -L 7
>
> bind -r > resize-pane -R 7
>
> bind -r - resize-pane -D 7
>
> bind -r + resize-pane -U 7

<font color=red>**-r表示不用多次输入前置键**</font>

## 通过alt快速切换窗口

> bind -n M-1 select-window -t 1
>
> bind -n M-2 select-window -t 2
>
> bind -n M-3 select-window -t 3
>
> bind -n M-4 select-window -t 4
>
> bind -n M-5 select-window -t 5
>
> bind -n M-6 select-window -t 6
>
> bind -n M-7 select-window -t 7
>
> bind -n M-8 select-window -t 8
>
> bind -n M-9 select-window -t 9
>
> bind -n M-p previous-window
>
> bind -n M-n next-window

## 新建pane与window为当前工作空间

> bind '"' split-window -vc "#{pane_current_path}"
>
> bind '%' split-window -hc "#{pane_current_path}"
>
> bind 'c' new-window -c "#{pane_current_path}"

## pre+r刷新tmux配置

> bind r source-file ~/.tmux.conf \; display "Config reloaded.."

## Bar设置

> set -g status-right "#[fg=green]%H:%M:%S #[fg=magenta]%a %m-%d #[default]" #将日期设置为红色,时间设置为绿色
>
> set -g status-interval 1 #设置时间实时刷新

> set -g status-justify centre #将状态栏居中显示
>
> set -g status-bg black # 设置背景色为黑色
>
> set -g status-fg blue # 设置前景色为蓝色
>
> set -g status-bg default #设置状态栏颜色为背景色
>
> set -g status-left " #[fg=green]#S@#H #[default]" #设置状态栏左边的名字#S为当前session名称#H为用户主机名称
>
> set -g status-left-length 20 #设置左边显示长度为20

## 开启鼠标支持

> \# mouse support
>
> set -g mouse on