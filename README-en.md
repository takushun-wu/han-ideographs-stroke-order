[点击阅读简体中文版说明文件](README.md)

# han-ideographs-stroke-order
Han Ideographs Stroke Order Database / A database of Han ideographs stroke order

## Overview
This project provides a database containing stroke order information for Han ideographs, aiming to facilitate developers and researchers in accessing and utilizing stroke order data. Each entry in the database includes the Unicode encoding, number of strokes, and stroke sequence of the corresponding Han character.


## Specification Parameters
### Han Character Range
| Block Name                       | Unicode Range |  Count | Notes |
|----------------------------------|:----------:|-----:|---|
|CJK Radicals Supplement           | 2E80..2EFF |   115||
|Kangxi Radicals                   | 2F00..2FFF |   214||
|CJK Symbols and Punctuation       | 3000..303F |     1|〇|
|CJK Unified Ideographs Extension A| 3400..4DBF | 6,592||
|CJK Unified Ideographs            | 4E00..9FFF |20,992||
|CJK Compatibility Ideographs      | F900..FAFF |    21|GBK defined non-compatible and duplicate ideographs|
|CJK Unified Ideographs Extension B|20000..2A6DF|    46|_TGH_, GBK and Adobe-GB1 radicals|
|CJK Unified Ideographs Extension C|2A700..2B73F|    44|_TGH_|
|CJK Unified Ideographs Extension D|2B740..2B81F|     8|ditto|
|CJK Unified Ideographs Extension E|2B820..2CEAF|   108|ditto|
|                                  |    **Total**|**28,141**||

### Reference Fonts
Given that different regular fonts have minor glyph differences, this project uses the following two sources of font glyphs as references:

- **Primary**: [WenYuan Serif SC v0.904](https://github.com/takushun-wu/WenYuanFonts) [glyphs follow the definition in the _General Standard Chinese Characters Table (aka TGH)_]
- **Secondary**: [GB/T 22321.1-2025 Information technology - Chinese coded character set - 48 dot matrix font of Chinese ideogram - Part 1: Song ti (in Chinese)](https://openstd.samr.gov.cn/bzgk/gb/newGbInfo?hcno=421B9135604C56A2EA1C4396A941CFB4) defined glyphs (hereafter referred to as _GB Glyphs_)

### Data Format
The database is stored in TSV (Tab-Separated Values) format, with each row containing the following fields:
- Sort Order (starting from 1)
- Han Ideograph
- Unicode Encoding (hexadecimal)
- Number of Strokes
- Stroke Sequence (using 12345 to represent)
- GB Glyph Stroke Sequence (same as above, provided if different from the primary glyph)
- GB Glyph Sort Order (same as above, provided if different from the primary glyph. Used to replace the first column's order)
- GB Glyph Sort Order (same as above, provided if different from the primary glyph. Used to replace the first column's order)

## Sorting Criteria
To compare the stroke orders of two Han ideographs, the following steps are taken in sequence until a difference is found, allowing the order to be determined:
1. **Number of Strokes**: A character with fewer strokes comes before a character with more strokes, e.g., $二(2)<王(4)$;
2. **Stroke Sequence**: The five major stroke types (horizontal ㇐, vertical ㇑, throw ㇒, dot ㇔, bend ㇖) are encoded using 12345 respectively and arranged in ascending order, e.g., $二(11)<十(12)$;
3. **Primary and Secondary Stroke Forms**
    1. **Horizontal, Vertical, Dot**: ㇐<㇀，㇑<㇚，㇔<㇏<乁<㇝，e.g., $干(㇑)<于(㇚)$
    2. **Bend Strokes**
        1. **Number of Bend Points**: Ascending order of the number of bend points, e.g., $㔔(0)<㔖(1)<乫(3)$
        2. **Stroke Forms within Bend Strokes**: Compare the stroke forms before and after the bend point in the order of horizontal, vertical, throw, dot, e.g., $幻(㇆)<乣(㇟)$
        3. **Types of Bend Points**: Bend < Curve < Hook, e.g., $㇗<㇄<㇙$
4. **Stroke Combination Relationships**
    1. **Positional Relationships**: Separate < Connected < Intersecting, e.g., $八<人<乂$
    2. **Connection Methods**: Head-to-head < Head-to-tail < Tail-to-tail < Body, e.g., $目<且$
    3. **Strokes Connected to the Body**: Compare the connected strokes in the order of horizontal, vertical, throw, dot, bend, e.g., $人(㇒)<入(㇔)$
    4. **Separation Situations**: Upper separation < Lower separation, Left-right separation < Up-down separation, e.g., $玊<玉,埒<埓$
    5. **Stroke Length Ratios**: Short-long < Long-short, e.g., $未<末$
5. **Character Structure**:
    1. Left-right < Up-down < Enclosing, Left-middle-right < Up-middle-down, e.g., $魂(⿰)<䰟(⿱),囂(⿳)<嚻(⿲),旮(⿱)<旭(⿺)$
    2. **Overall Character Proportions**: Ascending order, e.g., $口<囗$
6. Ascending order of Unicode encoding, e.g., $㬵(\texttt{U+3B35})<胶(\texttt{U+80F6})$

The sorting of bend strokes can also be summarized in the following table:

<table>
    <thead>
        <tr>
            <th><div style="text-align: center;">Number of Bend Points</div></th>
            <th colspan="2"><div style="text-align: center;">Stroke Forms within Bend Strokes</div></th>
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
            <td><strong>3</strong>：㇎㇈㇋㇠㇌㇉<sup><a href="#fn1" id="ref1">[1]</a></sup></td>
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

## License
This project is licensed under the [CC0 1.0 Universal (CC0 1.0) Public Domain Dedication](https://creativecommons.org/publicdomain/zero/1.0/) license, which allows you to freely use, modify, and distribute the content of this project without needing to seek permission or pay any fees.

Although this project strives to provide accurate and complete data, there may still be some errors or inconsistencies. Users are advised to verify and confirm the accuracy of the data when using it, and to assume the corresponding risks.

## Data Sources and References
### Data Sources
- [GF 0023-2020 Stroke Orders of The Commonly Used Standard Chinese Characters (in Chinese)](http://www.moe.gov.cn/jyb_sjzl/ziliao/A19/202103/W020210318300204215237.pdf)
- [或是最让人放心的汉字笔画序库-Excel表格模板下载与分享-ExcelHome技术论坛 (in Chinese)](https://club.excelhome.net/thread-649531-1-1.html)
- [CNMan/UnicodeCJK-WuBi：五笔字型超大字符集编码数据库 (in Chinese)](https://github.com/CNMan/UnicodeCJK-WuBi)
### References
- [GF 3003-1999《GB 13000.1字符集汉字字序（笔画序）规范》(in Chinese)](http://www.moe.gov.cn/jyb_sjzl/ziliao/A19/201001/W020150902458280061291.pdf)
- [GF 2001-2001 Chinese Character Turning Stroke Standard of GB 13000.1 Character Set (in Chinese)](http://www.moe.gov.cn/ewebeditor/uploadfile/2015/01/12/20150112170016626.pdf)
- [GF 0012-2009 Specification for Identifying Indexing Components of GB 13000.1 Chinese Characters Set (in Chinese)](http://www.moe.gov.cn/jyb_sjzl/ziliao/A19/200901/t20090102_186104.html)
- [GB/T 25741-2010 Information technology - Chinese ideograms coded character set - Radical order and stroke order of Chinese ideograms (in Chinese)](https://openstd.samr.gov.cn/bzgk/gb/newGbInfo?hcno=DB34CB2C5C377E35E3766A543724B767)

<div class="footnotes">
    <ol>
        <li id="fn1">The order in GF 2001-2001 should be ㇎<strong>㇋㇈(㇠)</strong>㇌㇉, while URO (GF 0012-2009) and the _Stroke Orders of TGH_ (GF 0023-2020) have it as ㇎<strong>㇈㇋㇠</strong>㇌㇉ (because <strong>凡&lt;及</strong>, and the second stroke of "㇋㇠" is actually displayed as "㇒"), so this project adopts the latter order.</li>
    </ol>
</div>