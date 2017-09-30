---
title: "Node.js 安裝與版本切換教學 (for MAC)"
date: 2014-01-09T22:21:28+08:00
draft: true
---


# 前言
安裝 Node.js（以下直接以node稱呼）有很多種方式。不過由於node更新非常快速，開發過程很有可能會有切換node版本的需求，因此強烈建議不要使用MAC上常用的 **Homebrew** 安裝node，而是使用 **nvm** （ **Node Version Manager** ）這個tool來安裝並管理node。不過我們還是需要用Homebrew來管理nvm，所以推薦的安裝流程如下：

1. 使用Homebrew安裝nvm
2. 使用nvm安裝Node.js
3. 使用nvm無痛切換Node.js版本

這篇文章針對上述流程做一個簡單的介紹

# 使用Homebrew安裝nvm
Homebrew基本上已經是MAC user必備的tool了，還沒裝的人強烈建議趕快裝上它吧！網路上教學很多，這邊就不再多介紹了。

首先先用`$ brew install nvm`安裝nvm：
```sh
$ brew install nvm
==> Downloading https://github.com/creationix/nvm/archive/v0.2.0.tar.gz
######################################################################## 100.0%
==> Caveats
Add the following to $HOME/.bashrc, $HOME/.zshrc, or your shell's equivalent configuration file:

  source $(brew --prefix nvm)/nvm.sh

Type `nvm help` for further information.
==> Summary
  /usr/local/Cellar/nvm/0.2.0: 3 files, 24K, built in 5 seconds
```

安裝完後，為了讓你可以直接在shell使用nvm指令，必須在你的 **.bash_profile** 加入以下這行（習慣把設定放在.bashrc的人可以把以下的.bash_profile改成.bashrc）
```sh
source $(brew --prefix nvm)/nvm.sh
```

或者直接輸入以下這行來加入
```sh
$ echo "source $(brew --prefix nvm)/nvm.sh" >> .bash_profile
```

記得重新source你的 **.bash_profile** 來讓設定生效
```sh
$ . ~/.bash_profile
```

OK，以上就完成了nvm的安裝，簡單吧！


# 使用nvm安裝Node.js

安裝完了nvm，接著安裝主角node。先用`$ nvm ls-remote`指令看一下有哪些版本可以安裝：
```sh
$ nvm ls-remote
      .
      .
      .
  v0.10.20
  v0.10.21
  v0.10.22
  v0.10.23
  v0.10.24
   v0.11.0
   v0.11.1
   v0.11.2
   v0.11.3
   v0.11.4
   v0.11.5
   v0.11.6
   v0.11.7
   v0.11.8
   v0.11.9
  v0.11.10
```
真夭壽多啊..果然是正在火速成長中的技術！

直接用`$ nvm install <version>`指令安裝[官網](http://nodejs.org/)上建議的版本：
```sh
$ nvm install v0.10.24
######################################################################## 100.0%
Now using node v0.10.24
```
也同時安裝最新版來測試nvm的版本管理功能：
```sh
$ nvm install v0.11.10
######################################################################## 100.0%
Now using node v0.11.10
```

# 使用nvm無痛切換Node.js版本

在介紹使用nvm切換版本前，先簡單說明nvm的工作原理。

nvm會把各個版本的node安裝在`/usr/local/opt/nvm`底下。我們可以看看該目錄底下放了哪些東西：
```sh
$ ls /usr/local/opt/nvm
INSTALL_RECEIPT.json  LICENSE.md  alias  bin  nvm.sh  v0.10.24  v0.11.10
```
我們可以發現透過nvm安裝這兩個版本，事實上會在nvm目錄下另外建立了`v0.10.24`以及`v0.11.10`兩個目錄來分別存放node的binary檔。又nvm會在你的`$PATH`最前面安插指定版本的目錄，透過這個方式你在使用node指令時就會用指定的版本來運作了。

實際確認PATH的值看看：
```sh
$ echo $PATH
/usr/local/opt/nvm/v0.11.10/bin: ...
```

nvm的用法非常的簡單。我們可以另外用`$ nvm ls`指令確認nvm目前可以管理的版本有哪些：
```sh
$ nvm ls

  v0.10.24
  v0.11.10
current: 	v0.11.10
```

由於透過nvm安裝node，會自動把最後安裝的版本設為目前使用中的版本，因此上述指令會看到`current:  v0.11.10`，表示我們目前正在使用`v0.11.10`

我們可以用`$ nvm use <version>`切換版本：
```sh
$ nvm use v0.10.24
Now using node v0.10.24
```
也可以偷懶一點，不用打完整的版號：
```sh
$ nvm use 0.10
Now using node v0.10.24
```
切換成別的版本看看：
```sh
$ nvm use 0.11
Now using node v0.11.10
```
夠簡單吧！

不過問題來了，如果你另外開一個shell視窗，並輸入nvm，會發現current version是空的：
```sh
$ nvm ls

  v0.10.24
  v0.11.10
current:
```
這是因為利用`nvm use`指令只會在當前的shell生效，當你開了新的shell就會發現`$PATH`的值已經不包含剛才設定的node目錄了。要解決這個問題就是利用`$ nvm alias default <version>`來設定一個預設的node版本：
```sh
$ nvm alias default 0.10
default -> 0.10 (-> v0.10.24)
```
此時再打開另一個shell視窗，就可以直接使用你所設定的node版本了。

# 實際跑跑看Node.js
我們直接拿官網的例子來試試看
先產生一個example.js的檔案：
```sh
$ touch example.js
```

內容如下
```js
http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(1337, '127.0.0.1');
console.log('Server running at http://127.0.0.1:1337/');
```

執行看看
```sh
$ node example.js
Server running at http://127.0.0.1:1337/
```
打開瀏覽器，輸入`http://127.0.0.1:1337`，如果看到 "Hello Word" 就代表成功了

# 後記
以上是我目前為止覺得比較好管理的安裝方式，如果有更推薦的方法歡迎一起討論！
