# THE PRACTICE OF PROGRAMMING
# プログラミング作法

## 所感
記載が古いが、テストの閾値チェックから回帰テストの自動化など、有効だと感じる。

ログ出力についての記載は少ないが、１行のみの例示であっても意味がわかる出力になっている。規約で定めて今でも守るべきだと感じる。
```
[Sun Dec 27 16:19:24 1998]
HTTPd: access to /usr/local/httpd/cgi-bin/test.html
    failed for m1.cs.bell-labs.com,
    reason: client denied by server (CGI non-executable)
    from http://m2.cs.bell-labs.com/cgi-bin/test.pl
```
p176 ログファイルを出力しよう
>（ページに収まるように編集してある）

とあるので、一行で出力しているのではないかと思う。

## 第一章　スタイル
冒頭コメントの例は、とてもありがちなように思う。コードを読めばわかることがコメントに書いてあるので、オフフック時、応答時、選ばれた国などの、理由が必要に思うが、単純に理由だけかかれても、コメントから読み取れないかもしれない。
### 1.1 名前
変数の冗長さを嫌うのは、可読性に直結するため意味があることかもしれない。関数名のisoctal()は名前がソースコードから読み取る情報になっている。コメントと同じような意味合いを持つように思う。
### 1.2 式と文
> ? leap_year = y % 4 == 0 && y % 100 != 0 || y % 400 == 0;

> leap_year = ((y%4 == 0) && (y%100 != 0)) || (y%400 == 0);  

この例の空白も一部取り除くというのは、気をつけないと自分も空白を入れたままにしがちな気がする。可読性についてどちらが良いと考えずに、機械的に演算子と変数との間は空白を入れていたようにも思う。この場合では後者のほうが読みやすいのは確かだ。空白がないと結びつきが強く見えるのはたしかにそのとおりだと思う。

#### 1-4
```
? if ( !(c == 'y' || c == 'Y') )
?     return;
```
>? length = (length < BUFSIZE) ? length : BUFSIZE;

>? flag = flag ? 0 : 1;

>? quote = (*line == '"') ? 1 : 0;

```
? if (val & 1)
?     bit = 1;
? else
?     bit = 0;
```

#### 1-6
>様々な評価順によって生成される可能性のある出力をすべて列挙せよ。

```
? n = 1;
? printf("%d %d\n", n++, n++);
```
1,1. 1,2. 1,3. 2,2. 2,3. 
1,3のパターンも可能性としてはありうるか。
- [paiza.io](https://paiza.io/projects/7RfBUir8Q_Lts22Ai97S-g?language=c) だと1,2になる。
- [coding_ground](https://www.tutorialspoint.com/compile_c_online.php)だと、Compile and Execute C Online (GNU GCC v7.1.1)とあるが、2,1になる。
- [code shef](https://www.codechef.com/ide)だと、Gcc6.3とあり、2,1になる。
- [ide one](https://ideone.com/enGkHn)だと、gcc 8.3とあり、2,1になる。
- [tech io](https://tech.io/snippet?l=c)だと、2,1になる。たぶんGcc系列だろう。
- [wandbox](https://wandbox.org/)は猛烈に早いし選択肢が大量にある。Gcc HEAD 10.0.0.1だと以下になる。
```
prog.c: In function 'main':
prog.c:6:29: warning: operation on 'n' may be undefined [-Wsequence-point]
    6 |     printf("%d %d\n", n++, n++);
      |                            ~^~
2 1
```
警告が出る。gcc 1.27でも2,1だ。
同じwandboxではclang HEAD 11.0.0も選べるが、こちらの結果は1,2だ。
```
prog.c:6:24: warning: multiple unsequenced modifications to 'n' [-Wunsequenced]
    printf("%d %d\n", n++, n++);
                       ^    ~~
1 warning generated.
1 2

```
同じく警告が出るが、clangの方が明確に、unsequenced modifications to 'n'と警告される。
- [codepad](http://codepad.org/68WJYzeS)は、2,1だ。
- [jdoodle](https://www.jdoodle.com/c-online-compiler/)は、gcc 9.1.0 で2,1だ。zapcc5.1.0だと1,2になる。なぜかzapcc側だけwarningが出る。
```
1 2

jdoodle.c:6:24: warning: multiple unsequenced modifications to 'n' [-Wunsequenced]
    printf("%d %d\n", n++, n++);
                       ^    ~~
1 warning generated.

```

- [Repl.it](https://repl.it/languages/c)では、clangを使っており1,2になるし警告も出る。コピペがしづらい。
```
clang version 7.0.0-3~ubuntu0.18.04.1 (tags/RELEASE_700/final)
 clang-7 -pthread -lm -o main main.c
main.c:6:22: warning: multiple unsequenced
      modifications to 'n' [-Wunsequenced]
  printf("%d %d\n", n++, n++);
                     ^    ~~
1 warning generated.
 ./main
1 2
 ^C
```

実際にやってみるとバラエティに飛んでいて面白いが、コンパイラはgcc, clang, zapccの３種類になる。
（いくつか調べたものはサービスが終了しているものもあった）
wandboxが使いやすい。[シェア用のリンクも生成できた](https://wandbox.org/permlink/tE8vWRYTGRqOF9Jp)

