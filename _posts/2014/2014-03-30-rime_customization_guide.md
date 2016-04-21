---
layout: post
title: 鼠鬚管输入法自定义
date: 2014-03-30
author: scsidisk
category: MacOSX
tags: [MacOSX]
---

## 输入法外观

```
# 適用於【鼠鬚管】0.9.13+
# 位置：~/Library/Rime/squirrel.custom.yaml
# 用法：想要哪項生效，就刪去該行行首的#字符，但注意保留用於縮進的空格

patch:
  us_keyboard_layout: true # 鍵盤選項：應用美式鍵盤佈局
  show_notifications_when: growl_is_running # 狀態通知，默認裝有Growl時顯示，也可設爲全開（always）全關（never）
  style/horizontal: true # 候選窗横向顯示
  style/font_face: "Hei" # 我喜歡的字體名稱
  style/font_point: 16 # 字號
  style/corner_radius: 10 # 窗口圓角半徑
  style/border_height: 0 # 窗口邊界高度，大於圓角半徑才有效果
  style/border_width: 0 # 窗口邊界寬度，大於圓角半徑才有效果
  style/color_scheme: google # 選擇配色方案
  # 註：預設的配色方案及代碼（指定爲 style/color_scheme ）
  # 碧水 - aqua
  # 青天 - azure
  # 明月 - luna
  # 墨池 - ink
  # 孤寺 - lost_temple
  # 暗堂 - dark_temple
  # 星際我爭霸 - starcraft
  # 谷歌 - google
```

## 输入法键盘绑定

```
# 位置：~/Library/Rime/default.custom.yamlpatch:

menu:
  page_size: 9
  schema_list:
    - schema: double_pinyin
  ascii_composer/switch_key:
  Control_R: noop
  Shift_R: noop
  Control_L: noop
  Shift_L: inline_ascii
  key_binder/bindings:
    - {accept: ";", send: 2, when: composing} # 使用";"键选择第二个字
    - {accept: "'", send: 3, when: composing} # 使用"'"键选择第三个字
    - {accept: "[", send: Page_Up, when: has_menu} # [ 上翻页
    - {accept: "]", send: Page_Down, when: has_menu} # ] 下翻页
```