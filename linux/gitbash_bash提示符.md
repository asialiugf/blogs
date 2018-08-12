### 修改/etc/profile，增加以下一行。修改了颜色。
```js
export PS1="\[\e[37;40m\][\[\e[36;40m\]\u\[\e[37;40m\]@\h \[\e[32;40m\]\w\[\e[0m\]]\\$ "
```

### vim 闪屏解决
在 .vimrc中增加以下两行
```js
set vb t_vb=
au GuiEnter * set t_vb=
```
