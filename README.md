# 组合字拼音

配方： ℞ **combination-pinyin**

[Rime](https://rime.im) 拼音输入方案

基于 [朙月拼音](https://github.com/rime/rime-luna-pinyin)，根据  [汉语拆字字典](https://github.com/kfcd/chaizi) 和 [汉字拼音数据](https://github.com/mozillazg/pinyin-data) 生成该拼音输入方案的字典。

## 简介

在输入拼音的时候，遇到一些生僻字只知其写法而不了解其发音，使用该拼音输入方案可以通过输入组成该字的字旁拼音即可获得。

该拼音输入方案也可以作为其他拼音输入方案（朙月拼音等）的反查功能，通过输入反查前缀（默认为 `u`）+ 组成该字的字旁拼音即可获得。

## 安装

组合字拼音可作为独立的拼音输入方案，也可用于其他拼音输入方案（朙月拼音等）作为反查功能。

* [作为拼音输入方案](#作为拼音输入方案)
* [作为反查功能](#作为反查功能)

将 [combination_pinyin.dict.yaml](https://github.com/sakuyaa/rime-combination-pinyin/raw/main/combination_pinyin.dict.yaml) ([CDN](https://cdn.jsdelivr.net/gh/sakuyaa/rime-combination-pinyin@main/combination_pinyin.dict.yaml)) 和 [combination_pinyin.schema.yaml](https://github.com/sakuyaa/rime-combination-pinyin/raw/main/combination_pinyin.schema.yaml) ([CDN](https://cdn.jsdelivr.net/gh/sakuyaa/rime-combination-pinyin@main/combination_pinyin.schema.yaml)) 两个文件复制到部署位置：

~/.config/ibus/rime  (Linux)

~/Library/Rime  (Mac OS)

%APPDATA%\\Rime  (Windows)

### **作为拼音输入方案**

复制文件后修改 `default.custom.yaml` 文件打上 patch：

```yaml
patch:
  schema_list:
    - schema: luna_pinyin
    - schema: terra_pinyin
    - schema: combination_pinyin   #上面是现有的拼音输入方案，只增加这一行
```

重新部署即可

### **作为反查功能**

复制文件后给现有的拼音输入方案的 custom 文件（例如 `luna_pinyin_simp.custom.yaml`）打上 patch：

```yaml
patch:
  schema/dependencies:
    - combination_pinyin
  engine/translators/+:
    - reverse_lookup_translator
  reverse_lookup:
    dictionary: combination_pinyin
    enable_completion: true
    prefix: "u"
    tips: 〔組合字〕
  recognizer/patterns/reverse_lookup: "u[a-z]*'?$"
```

如果现有的拼音输入方案已有反查功能（例如朙月拼音，前缀为 `` ` ``），可给其 custom 文件（`luna_pinyin.custom.yaml`）打上如下 patch 替代：

```yaml
patch:
  schema/dependencies:
    - combination_pinyin
  reverse_lookup/dictionary: combination_pinyin
  reverse_lookup/prefix: "u"
  reverse_lookup/tips: 〔組合字〕
  recognizer/patterns/reverse_lookup: "u[a-z]*'?$"
```

若要指定其他反查前缀，修改 patch 的 `prefix` 和 `recognizer/patterns/reverse_lookup` 项中的 `u` 为其他字符即可。

重新部署即可

## 构建字典

正常情况下，组合字拼音字典直接使用本仓库下的 [combination_pinyin.dict.yaml](https://github.com/sakuyaa/rime-combination-pinyin/raw/main/combination_pinyin.dict.yaml) ([CDN](https://cdn.jsdelivr.net/gh/sakuyaa/rime-combination-pinyin@main/combination_pinyin.dict.yaml)) 文件即可。

若要自行构建组合字拼音字典，先从以下地址下载 [汉语拆字字典](https://github.com/kfcd/chaizi) 和 [汉字拼音数据](https://github.com/mozillazg/pinyin-data) ：

* 汉语拆字字典：[简体](https://github.com/kfcd/chaizi/raw/master/chaizi-jt.txt) ([CDN](https://cdn.jsdelivr.net/gh/kfcd/chaizi@master/chaizi-jt.txt))、[繁體](https://github.com/kfcd/chaizi/raw/master/chaizi-ft.txt) ([CDN](https://cdn.jsdelivr.net/gh/kfcd/chaizi@master/chaizi-ft.txt))
* 汉字拼音数据：[pinyin.txt](https://github.com/mozillazg/pinyin-data/raw/master/pinyin.txt) ([CDN](https://cdn.jsdelivr.net/gh/mozillazg/pinyin-data@master/pinyin.txt))

下载并用浏览器打开 [build-dict.html](https://github.com/sakuyaa/rime-combination-pinyin/raw/main/build-dict.html) ([CDN](https://cdn.jsdelivr.net/gh/sakuyaa/rime-combination-pinyin@main/build-dict.html)) 网页文件，将上述文件拖放至网页中以生成字典文件。