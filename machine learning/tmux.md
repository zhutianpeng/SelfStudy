Tmux
====

http://kchen.cc/2016/11/18/custom-multiplexer-k-tmux/

Session
-------

**启动新会话：**

tmux [new -s 会话名 -n 窗口名]

**恢复会话：**

tmux at [-t 会话名]

**列出所有会话：**

tmux ls

**关闭会话：**

tmux kill-session -t 会话名

**关闭所有会话：**

tmux ls \| grep : \| cut -d. -f1 \| awk '{print substr(\$1, 0, length(\$1)-1)}'
\| xargs kill

**创建一个叫做 session_name 的 tmux session**

tmux new -s session_name

**重新开启叫做 session_name 的 tmux session**

tmux attach -t session_name

**转换到叫做 session_name 的 tmux session**

tmux switch -t session_name

**离开当前开启的 session**

tmux detach (prefix + d)
