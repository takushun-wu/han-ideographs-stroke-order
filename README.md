[Click to read English version of readme](README-en.md)

# han-ideographs-stroke-order
汉字笔画排序数据库 / A database of Han ideographs stroke order

## 简述
本项目提供了一个包含汉字笔画排序信息的数据库，旨在帮助开发者和研究人员更方便地获取和使用汉字笔画排序数据。数据库中的每个条目包含了汉字的Unicode编码、笔画数量、笔画顺序等信息。


## 规格参数
### 汉字范围
|区块名称               |区块名称（英文）                  |Unicode范围 |  数量|备注|
|-----------------------|----------------------------------|:----------:|-----:|---|
|中日韩部首补充         |CJK Radicals Supplement           | 2E80..2EFF |   115||
|康熙部首               |Kangxi Radicals                   | 2F00..2FFF |   214||
|中日韩符号和标点       |CJK Symbols and Punctuation       | 3000..303F |     1|〇|
|中日韩统一表意文字扩展A|CJK Unified Ideographs Extension A| 3400..4DBF | 6,592||
|中日韩统一表意文字     |CJK Unified Ideographs            | 4E00..9FFF |20,992||
|中日韩兼容表意文字     |CJK Compatibility Ideographs      | F900..FAFF |    21|GBK定义的非兼容汉字及重复字|
|中日韩统一表意文字扩展B|CJK Unified Ideographs Extension B|20000..2A6DF|    46|《通用规范汉字表》、GBK与Adobe-GB1部首|
|中日韩统一表意文字扩展C|CJK Unified Ideographs Extension C|2A700..2B73F|    44|《通用规范汉字表》|
|中日韩统一表意文字扩展D|CJK Unified Ideographs Extension D|2B740..2B81F|     8|同上|
|中日韩统一表意文字扩展E|CJK Unified Ideographs Extension E|2B820..2CEAF|   108|同上|
|                       |                                  |    **共计**|**28,141**||

### 参照字体
鉴于不同正文字体存在少量的字形差异，本项目使用以下两个来源的字体字形作为参照：

- **主要**：[文源宋体v0.904](https://github.com/takushun-wu/WenYuanFonts)（字形遵从《通用规范汉字表》所定义字形）
- **备考**：[GB/T 22321.1-2025《信息技术 中文编码字符集 汉字48点阵字型 第1部分：宋体》](https://openstd.samr.gov.cn/bzgk/gb/newGbInfo?hcno=421B9135604C56A2EA1C4396A941CFB4)定义字形（下文简称“GB字形”）

### 数据格式
数据库以TSV（Tab-Separated Values）格式存储，每行包含以下字段：
- 排序序号（从1开始）
- 汉字
- Unicode编码（十六进制）
- 笔画数量
- 笔画序列（使用12345表示）
- GB字形笔画序列（同上，若与主要字形不同则提供）
- GB字形排序序号（同上，若与主要字形不同则提供。用以替换第一列序号）

## 排序准则
欲比较两个汉字的笔画顺序，需按照以下步骤逐一比较，直到存在差别，能够确定顺序为止：
1. **笔画数量**：笔画少的汉字<笔画多的汉字，例：$二(2)<王(4)$；
2. **笔画序列**：将横（一，1）竖（丨，2）撇（丿，3）点（丶，4）折（乛，5）五个笔画大类对笔顺序列进行编码，按照升序排列，例：$二(11)<十(12)$；
3. **主附笔形**
    1. **横竖点**：㇐<㇀，㇑<㇚，㇔<㇏<乁<㇝，例：$干(㇑)<于(㇚)$
    2. **折笔**
        1. **折点数**：折点数升序排列，例：$㔔(0)<㔖(1)<乫(3)$
        2. **折笔内笔形**：按照折点前后笔形按横竖撇点依次比较，例：$幻(㇆)<乣(㇟)$
        3. **折点种类**：折<弯<钩，例：$㇗<㇄<㇙$
4. **笔画组合关系**
    1. **位置关系**：相离<相接<相交，例：$八<人<乂$
    2. **相接方式**：首首<首尾<尾尾<身段，例：$目<且$
    3. **身段被接笔画**：按被接笔画的横竖撇点折依次比较，例：$人(㇒)<入(㇔)$
    4. **相离情况**：上部相离<下部相离，左右相离<上下相离，例：$玊<玉,埒<埓$
    5. **笔画长短比例**：短长<长短，例：$未<末$
5. **汉字结构**：
    1. 左右<上下<包围，左中右<上中下，例：$魂(⿰)<䰟(⿱),囂(⿳)<嚻(⿲),旮(⿱)<旭(⿺)$
    2. **整字比例大小**：升序排列，例：$口<囗$
6. 编码升序排列，例：$㬵(\texttt{U+3B35})<胶(\texttt{U+80F6})$

折笔的排序还可总结为下表：

<table>
    <thead>
        <tr>
            <th><div style="text-align: center;">折点数</div></th>
            <th colspan="2"><div style="text-align: center;">折点内笔形</div></th>
        </tr>
        <tr>
            <td><strong>0</strong>：㇣</td>
            <td colspan="2"><div style="text-align: center;">N/A</div></td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="2"><strong>1</strong>：㇕㇇㇖㇗㇄㇙㇜㇛㇢㇁㇂</td>
            <td><strong>㇐</strong>：㇕㇇㇖</td>
            <td><strong>㇑</strong>：㇗㇄㇙</td>
        </tr>
        <tr>
            <td><strong>㇒</strong>：㇜㇛㇢</td>
            <td><strong>㇔</strong>：㇁㇂</td>
        </tr>
        <tr>
            <td><strong>2</strong>：㇅㇍㇊㇆㇤㇞㇥㇟(㇃)</td>
            <td><strong>㇐</strong>：㇅㇍㇊㇆㇤</td>
            <td><strong>㇑</strong>：㇞㇥㇟(㇃)</td>
        </tr>
        <tr>
            <td><strong>3</strong>：㇎㇈㇋㇠㇌㇉[^1]</td>
            <td><strong>㇐</strong>：㇎㇈㇋㇠㇌</td>
            <td><strong>㇑</strong>：㇉</td>
        </tr>
        <tr>
            <td><strong>4</strong>：㇡𠃉</td>
            <td><strong>㇐</strong>：㇡</td>
            <td><strong>㇒</strong>：𠃉</td>
        </tr>
    </tbody>
</table>

## 授权
本项目采用[CC0 1.0 Universal (CC0 1.0) Public Domain Dedication](https://creativecommons.org/publicdomain/zero/1.0/)协议授权，您可以自由地使用、修改和分发本项目的内容，无需征得许可或支付费用。

尽管本项目力求提供准确和完整的数据，可能仍存在一些错误或不一致之处。使用者在使用本项目的数据时，请自行验证和确认其准确性，并承担相应的风险。


## 数据来源及参考文献
### 数据来源
- [GF 0023-2020《通用规范汉字笔顺规范》](http://www.moe.gov.cn/jyb_sjzl/ziliao/A19/202103/W020210318300204215237.pdf)
- [或是最让人放心的汉字笔画序库-Excel表格模板下载与分享-ExcelHome技术论坛](https://club.excelhome.net/thread-649531-1-1.html)
- [CNMan/UnicodeCJK-WuBi：五笔字型超大字符集编码数据库](https://github.com/CNMan/UnicodeCJK-WuBi)
### 参考文献
- [GF 3003-1999《GB 13000.1字符集汉字字序（笔画序）规范》](http://www.moe.gov.cn/jyb_sjzl/ziliao/A19/201001/W020150902458280061291.pdf)
- [GF 2001-2001《GB 13000.1字符集汉字折笔规范》](http://www.moe.gov.cn/ewebeditor/uploadfile/2015/01/12/20150112170016626.pdf)
- [GF 0012-2009《GB 13000.1字符集汉字部首归部规范》](http://www.moe.gov.cn/jyb_sjzl/ziliao/A19/200901/t20090102_186104.html)
- [GB/T 25741-2010《信息技术 汉字编码字符集 汉字部首序和笔顺序》](https://openstd.samr.gov.cn/bzgk/gb/newGbInfo?hcno=DB34CB2C5C377E35E3766A543724B767)

[^1] GF 2001-2001中的顺序应为㇎<strong>㇋㇈(㇠)</strong>㇌㇉，而URO（GF 0012-2009）和《通用规范汉字笔顺规范》（GF 0023-2020）却为㇎<strong>㇈㇋㇠</strong>㇌㇉（因为**凡<及**，且“㇋㇠”的第二分笔画实际显示为“㇒”），因此本项目采用后者的顺序。