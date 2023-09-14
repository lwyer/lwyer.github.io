# Closing a stale SSH connection关闭一个失效的SSH链接 | 翻译


Suppose you’re connected to a remote host with SSH and after a while the SSH session goes stale. The terminal is unresponsive and no keypress seem to take effect. There might be something with the network, the remote host is restarting or maybe your machine has been in hibernation, there could be multiple reasons for a stale session.

> 假设你用SSH连接到一个远程主机，然后过一会儿SSH会话失效了。终端没有反应，而且似乎没有按键有效。可能是你的网络出了问题，远程主机正在重启或者也许是你的机器处于休眠状态，有多种原因可能使一个会话失效。

The first solution that might come to mind is to just close the terminal emulator and create another one, but there is a better way.

SSH escape sequences
Before I show the trick we take a quick detour and explore a kind of hidden feature that is implemented in many of the available SSH clients.

Built in to the SSH client are multiple hidden commands that can be triggered with a so called escape sequence. These commands can be access by a combination of the tilde prefix (~) followed by the command.

> 第一个想到的解决方法可能是只需要关闭终端模拟器然后再创建另一个，但还有更好的办法。
>
>  SSH转义序列
>
> 在我展示这个技巧之前，我们先绕道而行，探索一种隐藏的功能，它在许多可用的SSH客户端中都有实现。

For example ~? print the help message containing all of the supported escape sequences:

> 比如说，~？打印了帮助信息，它包含了所有受支持的转义序列。

```
david@remote-host:~$ ~?
Supported escape sequences:
 ~.   - terminate session
 ~B   - send a BREAK to the remote system
 ~R   - request rekey
 ~#   - list forwarded connections
 ~?   - this message
 ~~   - send the escape character by typing it twice
(Note that escapes are only recognized immediately after newline.)
```
Pay extra attention to the last line;

(Note that escapes are only recognized immediately after newline.)

This means that for the escape sequence to take effect a preceding newline is required.

> 要特别注意最后一行。
>
> （注意，转义只在新的一行立即被识别）
>
> 这意味着要使转义序列生效，前面需要有换行。

Also, a small note for people using a keyboard with a nordic layout; To type the tilde character, press AltGr+^ <Space>. I know that this tripped me up when I first learned about escape sequences.

The “terminate session” escape sequence
So, back to the initial problem.

> 还有，提醒一下使用北欧布局键盘的人 ，要打出一个波浪字符，按下AltGr+^ <Space>，我知道这个（是因为）当我第一次学习转义序列时，这让我出错了。
>
> “终止会话”的转义序列
>
> 所以，回到一开始的问题。 

As the help message stated above we can close the session with ~.. And remember, press enter a couple of times before initiating the sequence:

> 正如上面帮助信息说明的那样，我们可以使用~.来关闭会话。记住，在初始化序列时按几次回车键。

```
david@remote-host:~$ ~.
david@remote-host:~$ Shared connection to remote-host.davidisaksson.dev closed.
david@local:~$ echo $?
255
```
From here we can try to initiate the connection again, or just reuse the terminal.

> 从这里我们可以尝试再次初始化连接，或者只是重新使用终端。

> 所有翻译内容仅供个人学习使用，如涉及侵权，请联系本人删除。
> All translated content is for personal study only, in case of infringement, please contact me to delete.

---

hibernation 冬眠，休眠
trip sb up 使某人出错，绊倒
