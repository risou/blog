title: alacritty で Emacs を使う
date: 2019-05-13
datetime: 2019-05-13 23:33:09.734916 JST
alias: 43

---
alacritty というターミナルエミュレータが速いという話を聞き、仕事が一段落したタイミングで導入してみた。

[alacritty](https://github.com/jwilm/alacritty)

まだまだ実装段階というかリッチな機能が不足していてつらいという話を聞いたりもするが（タブで複数開けないとか）、個人的には今のところ使っていくのに最低限必要なものは揃ってくれていると感じた。

僕が使っていくのに困らない大きな要因は tmux を愛用しているから、というのが大きそうではある。  
tmux でタブや分割に相当する機能が確立されているため、ターミナルエミュレータ側での実装を必要としていない。

### alacritty で Emacs を使う

本題。

Mac OS 上で試してみたところ、いくつかの問題が起きた。

#### Meta キーが反応しない

Mac OS で Emacs を使うにあたっては option/alt キーを Meta として扱うように設定しているが、これまで使っていた iTerm2 では設定できたこれが alacritty では今はまだ設定できない。

これにどう対応するかというと、以下のような設定を追加する。
以下の設定は [Set option as meta in MacOS · Issue #62 · jwilm/alacritty](https://github.com/jwilm/alacritty/issues/62#issuecomment-347552058) から拝借した。


```
key_bindings:
  # meta
  - { key: A,         mods: Alt,       chars: "\x1ba"                       }
  - { key: B,         mods: Alt,       chars: "\x1bb"                       }
  - { key: C,         mods: Alt,       chars: "\x1bc"                       }
  - { key: D,         mods: Alt,       chars: "\x1bd"                       }
  - { key: E,         mods: Alt,       chars: "\x1be"                       }
  - { key: F,         mods: Alt,       chars: "\x1bf"                       }
  - { key: G,         mods: Alt,       chars: "\x1bg"                       }
  - { key: H,         mods: Alt,       chars: "\x1bh"                       }
  - { key: I,         mods: Alt,       chars: "\x1bi"                       }
  - { key: J,         mods: Alt,       chars: "\x1bj"                       }
  - { key: K,         mods: Alt,       chars: "\x1bk"                       }
  - { key: L,         mods: Alt,       chars: "\x1bl"                       }
  - { key: M,         mods: Alt,       chars: "\x1bm"                       }
  - { key: N,         mods: Alt,       chars: "\x1bn"                       }
  - { key: O,         mods: Alt,       chars: "\x1bo"                       }
  - { key: P,         mods: Alt,       chars: "\x1bp"                       }
  - { key: Q,         mods: Alt,       chars: "\x1bq"                       }
  - { key: R,         mods: Alt,       chars: "\x1br"                       }
  - { key: S,         mods: Alt,       chars: "\x1bs"                       }
  - { key: T,         mods: Alt,       chars: "\x1bt"                       }
  - { key: U,         mods: Alt,       chars: "\x1bu"                       }
  - { key: V,         mods: Alt,       chars: "\x1bv"                       }
  - { key: W,         mods: Alt,       chars: "\x1bw"                       }
  - { key: X,         mods: Alt,       chars: "\x1bx"                       }
  - { key: Y,         mods: Alt,       chars: "\x1by"                       }
  - { key: Z,         mods: Alt,       chars: "\x1bz"                       }
  - { key: A,         mods: Alt|Shift, chars: "\x1bA"                       }
  - { key: B,         mods: Alt|Shift, chars: "\x1bB"                       }
  - { key: C,         mods: Alt|Shift, chars: "\x1bC"                       }
  - { key: D,         mods: Alt|Shift, chars: "\x1bD"                       }
  - { key: E,         mods: Alt|Shift, chars: "\x1bE"                       }
  - { key: F,         mods: Alt|Shift, chars: "\x1bF"                       }
  - { key: G,         mods: Alt|Shift, chars: "\x1bG"                       }
  - { key: H,         mods: Alt|Shift, chars: "\x1bH"                       }
  - { key: I,         mods: Alt|Shift, chars: "\x1bI"                       }
  - { key: J,         mods: Alt|Shift, chars: "\x1bJ"                       }
  - { key: K,         mods: Alt|Shift, chars: "\x1bK"                       }
  - { key: L,         mods: Alt|Shift, chars: "\x1bL"                       }
  - { key: M,         mods: Alt|Shift, chars: "\x1bM"                       }
  - { key: N,         mods: Alt|Shift, chars: "\x1bN"                       }
  - { key: O,         mods: Alt|Shift, chars: "\x1bO"                       }
  - { key: P,         mods: Alt|Shift, chars: "\x1bP"                       }
  - { key: Q,         mods: Alt|Shift, chars: "\x1bQ"                       }
  - { key: R,         mods: Alt|Shift, chars: "\x1bR"                       }
  - { key: S,         mods: Alt|Shift, chars: "\x1bS"                       }
  - { key: T,         mods: Alt|Shift, chars: "\x1bT"                       }
  - { key: U,         mods: Alt|Shift, chars: "\x1bU"                       }
  - { key: V,         mods: Alt|Shift, chars: "\x1bV"                       }
  - { key: W,         mods: Alt|Shift, chars: "\x1bW"                       }
  - { key: X,         mods: Alt|Shift, chars: "\x1bX"                       }
  - { key: Y,         mods: Alt|Shift, chars: "\x1bY"                       }
  - { key: Z,         mods: Alt|Shift, chars: "\x1bZ"                       }
  - { key: Key1,      mods: Alt,       chars: "\x1b1"                       }
  - { key: Key2,      mods: Alt,       chars: "\x1b2"                       }
  - { key: Key3,      mods: Alt,       chars: "\x1b3"                       }
  - { key: Key4,      mods: Alt,       chars: "\x1b4"                       }
  - { key: Key5,      mods: Alt,       chars: "\x1b5"                       }
  - { key: Key6,      mods: Alt,       chars: "\x1b6"                       }
  - { key: Key7,      mods: Alt,       chars: "\x1b7"                       }
  - { key: Key8,      mods: Alt,       chars: "\x1b8"                       }
  - { key: Key9,      mods: Alt,       chars: "\x1b9"                       }
  - { key: Key0,      mods: Alt,       chars: "\x1b0"                       }
  - { key: Space,     mods: Control,   chars: "\x00"                        } # Ctrl + Space
  - { key: Grave,     mods: Alt,       chars: "\x1b`"                       } # Alt + `
  - { key: Grave,     mods: Alt|Shift, chars: "\x1b~"                       } # Alt + ~
  - { key: Period,    mods: Alt,       chars: "\x1b."                       } # Alt + .
  - { key: Key8,      mods: Alt|Shift, chars: "\x1b*"                       } # Alt + *
  - { key: Key3,      mods: Alt|Shift, chars: "\x1b#"                       } # Alt + #
  - { key: Period,    mods: Alt|Shift, chars: "\x1b>"                       } # Alt + >
  - { key: Comma,     mods: Alt|Shift, chars: "\x1b<"                       } # Alt + <
  - { key: Minus,     mods: Alt|Shift, chars: "\x1b_"                       } # Alt + _
  - { key: Key5,      mods: Alt|Shift, chars: "\x1b%"                       } # Alt + %
  - { key: Key6,      mods: Alt|Shift, chars: "\x1b^"                       } # Alt + ^
  - { key: Backslash, mods: Alt,       chars: "\x1b\\"                      } # Alt + \
  - { key: Backslash, mods: Alt|Shift, chars: "\x1b|"                       } # Alt + |
```

#### Ctrl/Alt + 記号の一部が反応しない

上記の他に、僕が Emacs 上で設定しているホットキーが一部反応しなかったのでそれについても設定を加えている。

```
  - { key: Semicolon, mods: Control,   chars: "\x18\x40\x63\x3b"            } # Ctrl + ;
  - { key: Semicolon, mods: Alt,       chars: "\x1b;"                       } # Alt + ;
  - { key: Slash,     mods: Control,   chars: "\x1f"                        } # Ctrl + /
  - { key: Apostrophe, mods: Control,  chars: "\x18\x40\x63\x27"            } # Ctrl + ''
```

それぞれ `helm-mini`, `comment-dwim-2`, `undo-tree-undo`, `redo` を割り当てていたが、この設定を追加しておかないと反応しなかった。

### その他のカスタマイズ

本題は以上だが、その他にいくつか不便を解消するために行った設定があるので残しておく。

#### 日本語を表示するためのフォントの設定

これまで `Menlo for Powerline` を使っていたが、 El Capitan で alacritty を動かすとマルチバイト文字の右半分が欠けてしまった。  
これについては `Cica` というフォントに切り替えることで解消した。  
ちなみに High Sierra ではこの問題には遭遇しなかった。

#### locale を自動判定してもらえない

シェルの起動時に locale が自動判定されず Powerline の設定を読み込むタイミングで Python がエラーを吐いてしまった。  
これはうまく設定する方法がなさそうだったので、ひとまず rc ファイルに以下の1行を加えて事なきを得た。

```
export LANG="ja_JP.UTF-8"
```

ちなみにこの問題は日本語の表示とは逆に High Sierra では起きたが El Capitan では起きなかった（ただしこれについてはそれぞれの環境での設定の違いが原因であって OS のバージョンの問題ではなさそうな気がしている）。
