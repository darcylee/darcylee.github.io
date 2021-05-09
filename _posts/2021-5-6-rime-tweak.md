---
layout:         post
title:          "Rime输入法配置"
subtitle:       "中州韵输入配置和ibus-rime美化"
header-style:   "text"
date:           2021-2-22
tags:           [rime, linux]

---

# rime 通用配置

详细参见官网 <https://rime.im/docs/> 。以下是我使用的一些配置


**输入方案设定(通用)**

```yaml
#文件位置：  ~/.config/ibus/rime/default.custom.yaml
patch:
  "ascii_composer/switch_key/Shift_L": noop
  "ascii_composer/switch_key/Shift_R": "commit_code"

  schema_list:
    - schema: luna_pinyin_simp
    # - schema: luna_pinyin
    # - schema: luna_pinyin_fluency
    # - schema: bopomofo
    # - schema: bopomofo_tw
    # - schema: cangjie5
    # - schema: stroke
    # - schema: terra_pinyin

  switcher/caption: "〔方案选择〕"
```


**针对明月拼音的设定(通用)**

主要针对模糊音、emoji 做的设置，参照 <https://github.com/rime/rime-emoji> 中 自助安装说明

```yaml
#文件位置： ~/.config/ibus/rime/luna_pinyin_simp.custom.yaml
patch:
  style/horizontal: true
  "menu/page_size": 6
  'speller/algebra':
    - erase/^xx$/                      # 第一行保留
    # 模糊音定義
    # 需要哪組就刪去行首的 # 號，單雙向任選
    - derive/^([zcs])h/$1/             # zh, ch, sh => z, c, s
    - derive/^([zcs])([^h])/$1h$2/     # z, c, s => zh, ch, sh

    #- derive/^n/l/                     # n => l
    #- derive/^l/n/                     # l => n

    # 這兩組一般是單向的
    #- derive/^r/l/                     # r => l

    #- derive/^ren/yin/                 # ren => yin, reng => ying
    #- derive/^r/y/                     # r => y

    # 下面 hu <=> f 這組寫法複雜一些，分情況討論
    # - derive/^hu$/fu/                  # hu => fu
    # - derive/^hong$/feng/              # hong => feng
    # - derive/^hu([in])$/fe$1/          # hui => fei, hun => fen
    # - derive/^hu([ao])/f$1/            # hua => fa, ...

    # - derive/^fu$/hu/                  # fu => hu
    # - derive/^feng$/hong/              # feng => hong
    # - derive/^fe([in])$/hu$1/          # fei => hui, fen => hun
    # - derive/^f([ao])/hu$1/            # fa => hua, ...

    # 韻母部份
    # - derive/^([bpmf])eng$/$1ong/      # meng = mong, ...
    - derive/([aei])n$/$1ng/            # an => ang, en => eng, in => ing
    - derive/([aei])ng$/$1n/            # ang => an, eng => en, ing => in

    # 樣例足夠了，其他請自己總結……

    # 反模糊音？
    # 誰說方言沒有普通話精確、有模糊音，就能有反模糊音。
    # 示例爲分尖團的中原官話：
    # - derive/^ji$/zii/   # 在設計者安排下鳩佔鵲巢，尖音i只好雙寫了
    # - derive/^qi$/cii/
    # - derive/^xi$/sii/
    # - derive/^ji/zi/
    # - derive/^qi/ci/
    # - derive/^xi/si/
    # - derive/^ju/zv/
    # - derive/^qu/cv/
    # - derive/^xu/sv/
    # 韻母部份，只能從大面上覆蓋
    # - derive/^([bpm])o$/$1eh/          # bo => beh, ...
    # - derive/(^|[dtnlgkhzcs]h?)e$/$1eh/  # ge => geh, se => sheh, ...
    # - derive/^([gkh])uo$/$1ue/         # guo => gue, ...
    # - derive/^([gkh])e$/$1uo/          # he => huo, ...
    # - derive/([uv])e$/$1o/             # jue => juo, lve => lvo, ...
    # - derive/^fei$/fi/                 # fei => fi
    # - derive/^wei$/vi/                 # wei => vi
    # - derive/^([nl])ei$/$1ui/          # nei => nui, lei => lui
    # - derive/^([nlzcs])un$/$1vn/       # lun => lvn, zun => zvn, ...
    # - derive/^([nlzcs])ong$/$1iong/    # long => liong, song => siong, ...
    # 這個辦法雖從拼寫上做出了區分，然而受詞典制約，候選字仍是混的。
    # 只有真正的方音輸入方案纔能做到！但「反模糊音」這個玩法快速而有效！

    # 模糊音定義先於簡拼定義，方可令簡拼支持以上模糊音
    - abbrev/^([a-z]).+$/$1/           # 簡拼（首字母）
    - abbrev/^([zcs]h).+$/$1/          # 簡拼（zh, ch, sh）

    # 以下是一組容錯拼寫，《漢語拼音》方案以前者爲正
    - derive/^([nl])ve$/$1ue/          # nve = nue, lve = lue
    - derive/^([jqxy])u/$1v/           # ju = jv,
    - derive/un$/uen/                  # gun = guen,
    - derive/ui$/uei/                  # gui = guei,
    - derive/iu$/iou/                  # jiu = jiou,

    # 自動糾正一些常見的按鍵錯誤
    - derive/([aeiou])ng$/$1gn/        # dagn => dang
    - derive/([dtngkhrzcs])o(u|ng)$/$1o/  # zho => zhong|zhou
    - derive/ong$/on/                  # zhonguo => zhong guo
    - derive/ao$/oa/                   # hoa => hao
    - derive/([iu])a(o|ng?)$/a$1$2/    # tain => tian

  # 分尖團後 v => ü 的改寫條件也要相應地擴充：
  #'translator/preedit_format':
  #  - "xform/([nljqxyzcs])v/$1ü/"

  punctuator/import_preset: symbols
  recognizer/patterns/punct: '^/([0-9]0?|[A-Za-z]+)$' # "translator/dictionary": luna_pinyin.sogou

  punctuator:
    full_shape:
      " ": {commit: "　"}
      "!": {commit: "！"}
      "\"": {pair: ["“", "”"]}
      "#": ["＃", "#", "№", "⌘"]
      "$": ["￥", "$", "€", "£", "¥", "¢", "¤"]
      "%": ["%", "％", "°", "℃", "‰", "‱", "℉", "℅", "℆", "℀", "℁", "⅍"]
      "&": "＆"
      "'": {pair: ["‘", "’"]}
      "(": "（"
      ")": "）"
      "*": ["＊", "·", "・", "×", "※", "❂", "⁂", "☮", "☯", "☣"]
      "+": "＋"
      ",": {commit: "，"}
      "-": "－"
      .: {commit: "。"}
      "/": ["／", "÷"]
      ":": {commit: "："}
      ";": {commit: "；"}
      "<": ["《", "〈", "«", "‹"]
      "=": ["＝", "=", "々", "〃"]
      ">": ["》", "〉", "»", "›"]
      "?": {commit: "？"}
      "@": ["＠", "☯","©", "®", "℗"]
      "[": ["「", "【", "〔", "［"]
      "\\": ["、", "＼"]
      "]": ["」", "】", "〕", "］"]
      "^": {commit: "……"}
      _: "——"
      "`": "｀"
      "{": ["『", "〖", "｛"]
      "|": ["·", "・", "|", "｜", "§", "¦", "‖", "︴"]
      "}": ["』", "〗", "｝"]
      "~": ["~", "～", "˜", "˷", "ⸯ", "≈", "≋", "≃", "≅", "≇", "∽", "⋍", "≌", "﹏", "﹋", "﹌", "︴"]
    half_shape:
      "!": {commit: "！"}
      "\"": {pair: ["“", "”"]}
      "#": "#"
      "$": "$"
      "%": "%"
      "&": "&"
      "'": {pair: ["‘", "’"]}
      "(": ["(", "（"]
      ")": [")", "）"]
      "*": "*"
      "+": "+"
      ",": {commit: "，"}
      "-": "-"
      .: {commit: "。"}
      "/": ["、", "/", "／", "÷"]
      ":": {commit: "："}
      ";": {commit: "；"}
      "<": ["《", "〈", "«", "‹", "˂", "˱"]
      "=": "="
      ">": ["》", "〉", "»", "›", "˃", "˲"]
      "?": {commit: "？"}
      "@": "@"
      "[": ["「", "【", "〔", "［", "〚", "〘"]
      "\\": ["、", "\\", "＼"]
      "]": ["」", "】", "〕", "］", "〛", "〙"]
      "^": {commit: "……"}
      _: "_"
      "`": ["`", "‵", "‶", "‷", "′", "″", "‴", "⁗"]
      "{": ["『", "〖", "｛"]
      "|": "|"
      "}": ["』", "〗", "｝"]
      "~": "~"

  switches/@next:
    name: emoji_suggestion
    reset: 1
    states: [ "🈚️️\uFE0E", "🈶️️\uFE0F" ]
  'engine/filters/@before 0':
    simplifier@emoji_suggestion
  emoji_suggestion:
    opencc_config: emoji.json
    option_name: emoji_suggestion
    tips: all
```


**词库同步**

在 rime 用户文件夹下找到 installation.yaml 文件，添加如下关键配置：
```yaml
installation_id: "linux-emacs"             # 根据不同平台起不同名字
sync_dir: "/path/of/your/rime-user-data"   # 自定义共享路径
```

# 中州韵输入法安装 - IBUS

**安装**


```
sudo apt install librime1
sudo apt install ibus ibus-rime ibus-gtk
```

然后在 setting -> Region&Language 中将 rime 添加到 系统输入法列表中
![setting_rime.png](/img/post/2021-5-6-rime-tweak/setting_rime.png)


**针对IBUS的配置**

```yaml
#文件位置： ~/.config/ibus/rime/ibus_rime.yaml
style:
  horizontal: true
  inline_preedit: false
```


**美化**

1. 安装 ibus 美化 gnome-shell-extension  [Customize-IBus](https://github.com/hollowman6/customize-ibus)
2. 下载美化皮肤 [IBus-Theme-Hub](https://github.com/HollowMan6/IBus-Theme-Hub.git), 此为原始仓库，
个人 fork 了一个仓库 [darcylee/IBus-Theme-Hub](https://github.com/darcylee/IBus-Theme-Hub) ，并添
加了一个皮肤 Orange-Dark-Numix, 效果如下
![Orange-Dark-Numix.png](/img/post/2021-5-6-rime-tweak/Orange-Dark-Numix.png)


# emacs 中使用 rime 输入法(linux&macOS)

最开始使用的是 liberime+pyim 的方案， 经过使用和对比，最后选择的是 [emacs-rime]( https://www.github.com/DogLooksGood/emacs-rime)

在 emacs 中使用 rime 的优势个人觉得有如下几点
1. 可以在终端远程直接输入中文，着实很方便
2. 可以配合 evil 做状态切换，在 insert 和 normal 模式中自由切换，非常方便
3. emacs-rime 全自动编译支持，非常友好(macOS 需要做一些设定)

以下是基于 spacemacs+emacs-rime 的配置，

```elisp
(defun darcylee-rime-sync-delay ()
  (defvar already-sync nil)
  (if (not already-sync)
      (progn
        (setq already-sync t)
        (run-with-idle-timer 5 nil (lambda () (rime-sync)
                                     (message "rime sync done"))))))

(defun darcylee-better-defaults/init-rime ()
  (use-package rime
    :init
    (setq rime-title "Rime")
    (setq rime-show-candidate 'popup)
    (when (and (display-graphic-p)
               ;; (eq system-type 'darwin)
               (>= emacs-major-version 26))
      (require 'posframe)
      (setq rime-show-candidate 'posframe)
      (setq rime-posframe-properties
            (list :internal-border-width 8)))
    (add-hook 'rime-active-mode-hook #'darcylee-rime-sync-delay)

    :config
    (setq rime-disable-predicates
          '(rime-predicate-evil-mode-p
            rime-predicate-after-alphabet-char-p
            rime-predicate-prog-in-code-p))
    ;; (setq rime-cursor "˰")
    ;; support shift-l, shift-r, control-l, control-r
    (setq rime-inline-ascii-trigger 'shift-l)
    (setq rime-user-data-dir (expand-file-name "private/rime" dotspacemacs-directory))

    (define-key rime-active-mode-map (kbd "M-j") 'rime-inline-ascii)
    ;; (global-set-key (kbd "C-`") 'rime-inline-ascii)
    ;; (define-key evil-insert-state-map (kbd "C-`") 'rime-inline-ascii)
    (define-key rime-mode-map (kbd "C-`") 'rime-send-keybinding)
    (setq mode-line-mule-info '((:eval (rime-lighter))))

    :custom
    (default-input-method "rime")))
```


**效果**

![emacs-rime.png](/img/post/2021-5-6-rime-tweak/emacs-rime.png)


**自动同步词库**

主要针对 emacs-rime 生效，思想是在启动 rime 输入法后，延时执行 rime-sync, 使用 'rime-active-mode-hook 
实现，见上面代码 darcylee-rime-sync-delay。


**备注**

以上是在 macOS 中安装 emacs-rime 可能会遇到一些麻烦，可以参照 [emacs-rime]( https://www.github.com/DogLooksGood/emacs-rime) 中
的安装说明，实际安装也非常快

# windows 支持 rime 输入法 - Weasel

**美化 - 仿微软输入法**

weasel.custom.yaml

```yaml
patch:
  preset_color_schemes:
    win10:
        name: Windows10
        author: me
        text_color: 0x000000             # 編碼行文字顏色，24位色值，用十六進制書寫方便些，順序是藍綠紅0xBBGGRR
        candidate_text_color: 0x000000   # 候選項文字顏色，當與文字顏色不同時指定
        back_color: 0xFFFFFF             # 底色
        border_color: 0xFF9933           # 邊框顏色，與底色相同則爲無邊框的效果
        hilited_text_color: 0xFFFFFF     # 高亮文字，即與當前高亮候選對應的那部份輸入碼
        hilited_back_color: 0xFF9933     # 設定高亮文字的底色，可起到凸顯高亮部份的作用
        hilited_candidate_text_color: 0xFFFFFF  # 高亮候選項的文字顏色，要醒目！
        hilited_candidate_back_color: 0xFF9933  # 高亮候選項的底色，若與背景色不同就會顯出光棒

    
  style:
    color_scheme: win10
    horizontal: true
    font_point: 13
    inline_preedit: true
    font_face: Microsoft YaHei
    layout:
        min_width: 10
        round_corner: 0
        hilite_padding: 15
        margin_x: 15
        margin_y: 10
        candidate_spacing: 30
        border_width: 1
```

**效果**

![weasel-window10-theme.png](/img/post/2021-5-6-rime-tweak/weasel-window10-theme.png)

# macOS 使用 rime 输入法 - Squirrel

**关键配置**

```yaml
patch:
# 更换皮肤
  style/color_scheme: macos_light

  # 皮肤主题
  preset_color_schemes:
    macos_light:
      author: "一方<liuour@gmail.com>"
      back_color: 0xFFFFFF                      # 候选条背景色，24位色值，16进制，BGR顺序
      border_color: 0xFFFFFF                    # 边框色
      text_color: 0x424242                      # 拼音行文字颜色
      # hilited_back_color: 0xD75A00              # 第一候选项背景背景色
      hilited_back_color: 0xA84DEC              # 第一候选项背景背景色
      hilited_candidate_text_color: 0xFFFFFF    # 第一候选项文字颜色
      hilited_candidate_label_color: 0xFFFFFF   # 第一候选项编号颜色
      hilited_comment_text_color: 0x999999      # 注解文字高亮
      hilited_text_color: 0x999999              # 高亮拼音 (需要开启内嵌编码)
      candidate_text_color: 0x3c3c3c            # 预选项文字颜色
      comment_text_color: 0x999999              # 拼音等提示文字颜色
      horizontal: true                          # 水平排列
      inline_preedit: true                      # 单行显示，false双行显示
      label_color: 0x999999                     # 预选栏编号颜色
      candidate_format: "%c\u2005%@"            # 用 1/6 em 空格 U+2005 来控制编号 %c 和候选词 %@ 前后的空间
      font_face: "PingFangSC"                   # 候选词编号字体
      font_point: 16              # 候选文字大小
      label_font_point: 13        # 候选编号大小
      corner_radius: 5            # 候选条圆角
      hilited_corner_radius: 5    # 高亮圆角
      border_height: 4            # 窗口上下高度
      border_width: 4             # 窗口左右宽度
      border_color_width: 0       # 输入条边框宽度

    macos_dark:
      author: "一方<liuour@gmail.com>"
      back_color: 0x252a2e                      # 候选条背景色，24位色值，16进制，BGR顺序
      #border_color: 0x050505                    # 边框色
      border_color: 0x252a2e                    # 边框色
      text_color: 0x424242                      # 拼音行文字颜色
      #hilited_back_color: 0xD75A00              # 第一候选项背景背景色
      hilited_back_color: 0xA84DEC              # 第一候选项背景背景色
      hilited_candidate_text_color: 0xFFFFFF    # 第一候选项文字颜色
      hilited_candidate_label_color: 0xFFFFFF   # 第一候选项编号颜色
      hilited_comment_text_color: 0x999999      # 注解文字高亮
      hilited_text_color: 0x999999              # 高亮拼音 (需要开启内嵌编码)
      candidate_text_color: 0xe9e9ea            # 预选项文字颜色
      comment_text_color: 0x999999              # 拼音等提示文字颜色
      horizontal: true                          # 水平排列
      inline_preedit: true                      # 单行显示，false双行显示
      label_color: 0x999999                     # 预选栏编号颜色
      candidate_format: "%c\u2005%@"            # 用 1/6 em 空格 U+2005 来控制编号 %c 和候选词 %@ 前后的空间
      font_face: "PingFangSC"                   # 候选词编号字体
      font_point: 16              # 候选文字大小
      label_font_point: 13        # 候选编号大小
      corner_radius: 5            # 候选条圆角
      hilited_corner_radius: 5    # 高亮圆角
      border_height: 4            # 窗口上下高度
      border_width: 4             # 窗口左右宽度
      border_color_width: 0       # 输入条边框宽度

  app_options:
    com.apple.Spotlight:
      ascii_mode: true
    com.runningwithcrayons.Alfred:
      ascii_mode: true
    com.googlecode.iterm2:
      ascii_mode: true
    com.microsoft.VSCode:
      ascii_mode: false
      ascii_punct: true
    com.vandyke.SecureCRT:
      ascii_mode: true
      ascii_punct: true
    # org.gnu.Emacs:
    #   ascii_mode: true

```
