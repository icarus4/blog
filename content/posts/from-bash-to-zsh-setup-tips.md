---
title: "bash 轉移 zsh (oh-my-zsh) 設定心得"
date: 2014-01-30T13:39:28+08:00
draft: true
---


# 前言
一直以來看到很多人推薦用 zsh (z shell)，原因就不贅述了，簡單講就是更好用、更炫 (?)。今天花了一些時間從 bash 轉移到 zsh，過程中遇到一些小問題讓我卡了幾次小關，這篇主要是分享我遇到的狀況，有興趣裝 zsh 的人如果遇到類似問題可以參考看看。


# 安裝說明
這邊講 MAC 的安裝方法，不過 Linux 也差不多就是了。簡單來說，共有四個步驟：
1. 安裝 `zsh`
2. 安裝 `oh-my-zsh`
3. 安裝 `zsh-completions`
4. 調整設定

第2步安裝的 [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh) 是一套 zsh 的 framework，內建了非常多實用的設定、plugin、alias、theme...等等，可以讓剛跳到 zsh 的使用者大幅降低轉換和設定的成本，幾乎是達到了免設定就能非常好用的境界。從 google 趨勢搜尋可以看出 2011 年 oh-my-zsh 出現在 google 趨勢搜尋以後，[zsh的熱門度開始由下降的趨勢轉為上升](http://www.google.com.tw/trends/explore#q=zsh%2C%20oh-my-zsh)，所以 zsh 最近能越來越紅我想 oh-my-zsh 功不可沒。總之強烈建議要玩 zsh 的話記得一起安裝 oh-my-zsh！

不過即使 oh-my-zsh 再好用，還是有一些自動補全的功能沒有包含在裡面。例如說如果你在 bash 底下有安裝 `bash-completion`，會發現你用 homebrew 再按下 tab 時可以自動補全套件的名稱，像是這樣：
```sh
$ brew search z
```
這時按下 `tab` 鍵，會自動show出所有z開頭的套件：
```sh
$ brew search z
z                             zeromq22                      zsh-completions
z80asm                        zile                          zsh-history-substring-search
z80dasm                       zinc                          zsh-lovers
zabbix                        zint                          zsh-syntax-highlighting
zbackup                       zlib                          zshdb
zbar                          znc                           zssh
zdelta                        zookeeper                     zsync
zebra                         zopfli                        zxcc
zenity                        zpython                       zzuf
zeromq                        zsh
```

類似的 homebrew 補全功能在 oh-my-zsh 是沒有納入的，所以需要透過自行撰寫，或者利用類似 `zsh-completions` 這樣的套件來補足。


# 安裝方法


## 安裝 zsh
```sh
$ brew install zsh
```
然後記得修改預設的 shell 為 zsh ，這樣子以後開一個新的 shell 就都會是使用 zsh 了：
```sh
$ chsh -s /usr/local/bin/zsh
```


## 安裝 on-my-zsh
下載：
```sh
$ git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
```
預設安裝路徑是 `~/.oh-my-zsh`，如果想裝在別的地方記得記得修改 `~/.zshrc`。

zsh 的設定檔放在 `~/.zshrc`，這個檔案需要我們自己產生。由於我們用 oh-my-zsh，所以這邊建議使用 oh-my-zsh 預設的 templete 比較省事：
```sh
$ cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
```


## 安裝 zsh-completions
安裝：
```sh
$ brew install zsh-completions
```

要啟用還需要打開你的 `.zshrc` 加入以下兩行來納入 zsh-completions 的補全功能：
```diff .zshrc
@@ -1,8 +1,11 @@
 # Path to your oh-my-zsh configuration.
 ZSH=$HOME/.oh-my-zsh

+# zsh-completions
+fpath=(/usr/local/share/zsh-completions $fpath)

 # Set name of the theme to load.
 # Look in ~/.oh-my-zsh/themes/
 # Optionally, if you set this to "random", it'll load a random theme each
 # time that oh-my-zsh is loaded.
 #ZSH_THEME="robbyrussell"
```

同時還需要 rebuild zsh 的 `.zcompdump`
```sh
$ rm -f ~/.zcompdump; compinit
```
這樣子就搞定了


## 調整設定
經過以上的安裝，已經可以做一些基本的使用了（也很好用了），不過 oh-my-zsh 內建很多 theme 還有好用的 plugin，需要透過手動啟用。


### 1. 切換 theme
所有的主題都放在 `~/.oh-my-zsh/themes` 目錄中，先看一下有哪些可以用：
```
$ ls ~/.oh-my-zsh/themes
3den.zsh-theme           fwalch.zsh-theme           nanotech.zsh-theme
Soliah.zsh-theme         gallifrey.zsh-theme        nebirhos.zsh-theme
adben.zsh-theme          gallois.zsh-theme          nicoulaj.zsh-theme
af-magic.zsh-theme       garyblessington.zsh-theme  norm.zsh-theme
afowler.zsh-theme        gentoo.zsh-theme           obraun.zsh-theme
agnoster.zsh-theme       geoffgarside.zsh-theme     peepcode.zsh-theme
alanpeabody.zsh-theme    gianu.zsh-theme            philips.zsh-theme
amuse.zsh-theme          gnzh.zsh-theme             pmcgee.zsh-theme
apple.zsh-theme          gozilla.zsh-theme          pure.zsh-theme
arrow.zsh-theme          half-life.zsh-theme        pygmalion.zsh-theme
aussiegeek.zsh-theme     humza.zsh-theme            re5et.zsh-theme
avit.zsh-theme           imajes.zsh-theme           rgm.zsh-theme
awesomepanda.zsh-theme   intheloop.zsh-theme        risto.zsh-theme
bira.zsh-theme           itchy.zsh-theme            rixius.zsh-theme
blinks.zsh-theme         jaischeema.zsh-theme       rkj-repos.zsh-theme
bureau.zsh-theme         jbergantine.zsh-theme      rkj.zsh-theme
candy-kingdom.zsh-theme  jispwoso.zsh-theme         robbyrussell.zsh-theme
candy.zsh-theme          jnrowe.zsh-theme           sammy.zsh-theme
  .
  .
  .
```
切換方式是修改 `.zshrc` 的 `ZSH_THEME` 這個參數，預設是 `robbyrussell`，如果想改成 `apple.zsh-theme`，那麼請把 `ZSH_THEME` 改為 `apple`：
```diff .zshrc
 # Set name of the theme to load.
 # Look in ~/.oh-my-zsh/themes/
 # Optionally, if you set this to "random", it'll load a random theme each
 # time that oh-my-zsh is loaded.
-ZSH_THEME="robbyrussell"
+ZSH_THEME="apple"
```
改完後，重新登入看看，應該就可以發現你的 shell 長得不一樣了。

[這邊有一些 theme 的截圖](https://github.com/robbyrussell/oh-my-zsh/wiki/themes)，如果不想一個一個試來看效果，可以參考看看。

### 2. 啟用 plugin
oh-my-zsh 內建的 plugin 都放在 `~/.oh-my-zsh/plugins`：
```sh
$ ls ~/.oh-my-zsh/plugins
ant                github                    rails3
apache2-macports   gitignore                 rails4
archlinux          gnu-utils                 rake
autoenv            go                        rand-quote
autojump           golang                    rbenv
autopep8           gpg-agent                 rbfu
battery            gradle                    rebar
bower              grails                    redis-cli
brew               heroku                    repo
  .
  .
  .
```

啟用 plugin 一樣是在 `.zshrc` 中做設定，預設的設定只啟用了 git 的 plugin，如下：
```sh .zshrc
# Which plugins would you like to load? (plugins can be found in ~/.oh-my-zsh/plugins/*)
# Custom plugins may be added to ~/.oh-my-zsh/custom/plugins/
# Example format: plugins=(rails git textmate ruby lighthouse)
plugins=(git)
```

要啟用某個 plugin，就只要加在括號裡即可，比如說想要用 heroku 的 plugin，請把 .zshrc 改成：
```diff .zshrc
 # Which plugins would you like to load? (plugins can be found in ~/.oh-my-zsh/plugins/*)
 # Custom plugins may be added to ~/.oh-my-zsh/custom/plugins/
 # Example format: plugins=(rails git textmate ruby lighthouse)
-plugins=(git)
+plugins=(git heroku)
```
至於這麼多 plugin 分別有什麼用途，以後有空再介紹....

# 轉換過程中可能遇到的一些問題

### 1. alias
如果你本來就有設定一些 alias 在你的 `.bashrc`，你又把這些設定直接套用到 `.zshrc`，那有機會有一些指令會變怪怪的，這有可能是你設定的 alias 與 oh-my-zsh 內建的衝到了。oh-my-zsh 內建的 alias 放在 `~/.oh-my-zsh/lib/aliases.zsh`，內容如下：
```sh aliases.zsh
# Push and pop directories on directory stack
alias pu='pushd'
alias po='popd'

# Basic directory operations
alias ...='cd ../..'
alias -- -='cd -'

# Super user
alias _='sudo'
alias please='sudo'

#alias g='grep -in'

# Show history
if [ "$HIST_STAMPS" = "mm/dd/yyyy" ]
then
    alias history='fc -fl 1'
elif [ "$HIST_STAMPS" = "dd.mm.yyyy" ]
then
    alias history='fc -El 1'
elif [ "$HIST_STAMPS" = "yyyy-mm-dd" ]
then
    alias history='fc -il 1'
else
    alias history='fc -l 1'
fi
# List direcory contents
alias lsa='ls -lah'
alias l='ls -la'
alias ll='ls -l'
alias la='ls -lA'
alias sl=ls # often screw this up

alias afind='ack-grep -il'
```
建議要在 `.zshrc` 加上自己的 alias 前先確認看看這個檔案裡是不是有一些 alias 會跟你的衝到。
### 2. bash-completion
如果你跟我一樣用 bash 時有裝 bash-completion，而且又沒仔細弄清楚就把 `.bashrc` 的內容一股腦套用在 `.zshrc` 上，那麼當你開一個新的 shell 時有可能會發現遇到以下的訊息：
```sh
  .
  .
  .
/usr/local/etc/bash_completion:138: command not found: complete
/usr/local/etc/bash_completion:141: command not found: complete
/usr/local/etc/bash_completion:144: command not found: complete
/usr/local/etc/bash_completion:147: command not found: complete
/usr/local/etc/bash_completion:150: command not found: complete
/usr/local/etc/bash_completion:153: command not found: complete
/usr/local/etc/bash_completion:156: command not found: complete
/usr/local/etc/bash_completion:159: command not found: complete
/usr/local/etc/bash_completion:162: command not found: complete
/usr/local/etc/bash_completion:246: parse error near `]]'
```
這是因為在安裝 bash-completion 的時候，原則上都會加入以下內容在你的 `.bashrc`：
```sh .bashrc
if [ -f $(brew --prefix)/etc/bash_completion ]; then
    . $(brew --prefix)/etc/bash_completion
fi
```
以上指令會去source `bash_completion` 的內容，而 bash_completion 裡面使用了 [bash completion buitins](http://www.gnu.org/software/bash/manual/html_node/Programmable-Completion-Builtins.html) 中的 `complete` 這個指令，所以如果你的 .zshrc 也去 source bash_completion，那就會show出像上面的 `command not found: complete` 這種訊息了。所以記得在 .zshrc 中不要加入這段 code。

趕快一起來用 zsh 吧！