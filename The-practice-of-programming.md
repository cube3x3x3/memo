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
