# comment_asm

* for arm asm
* for g++ asm(not clang++)

## how to run
```
$ g++ -std=c++11 main.cpp -g -S
$ cat main.s | ./comment_asm > main.comment.s
```

## REQUIRED
* gawk

## NOTE
* 対象ファイルと同一のディレクトリで実行すること
* `-g`オプションでコンパイルした場合には`.loc`ファイルの情報がある

## FYI
* [awk \- Distributing a script: Should I use /bin/gawk or /usr/bin/gawk for shebang? \- Unix & Linux Stack Exchange]( https://unix.stackexchange.com/questions/97141/distributing-a-script-should-i-use-bin-gawk-or-usr-bin-gawk-for-shebang )

## TODO
* asmの説明が足りない
* awkでは速度が遅い
* clang++やx86などへの対応

## FMY
* vim command: `:AsmComment`
