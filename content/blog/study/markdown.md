---
title: 'π MarkDown λ¬Έλ² μ λ¦¬'
date: 2021-06-10 01:03:31
category: 'study'
thumbnail: 'https://gatsby-blog-images.s3.ap-northeast-2.amazonaws.com/thumb_markdown.png'
description: '.md λ¬Έμ μμ±λ²μ μμλ³΄μ'
tags: ['λ§ν¬λ€μ΄', 'MarkDown', '.md']
draft: false
---

![MarkDown logo](https://gatsby-blog-images.s3.ap-northeast-2.amazonaws.com/thumb_markdown.png)


# 1.λ§ν¬λ€μ΄μ΄λ?

λ§ν¬μ μΈμ΄μ μΌμ’μΌλ‘, μΌλ° νμ€νΈ λ¬Έμμ μμμ νΈμ§νλ λ° μ£Όλ‘ μ°μΈλ€.


HTML λ± λ€λ₯Έ λ¬Έμννλ‘μ λ³νμ΄ μ©μ΄νλ€.


π‘ μ ννλ νμ€μ΄ μ‘΄μ¬νμ§ μκΈ° λλ¬Έμ μλν°μ λ°λΌ κ²°κ³Όλ¬Όμ΄ λ¬λΌμ§ μ μλ€.

# 2.λ§ν¬λ€μ΄ λ¬Έλ²
## 2.1 μ λͺ©
μ λͺ©μ `<h1>`~`<h6>` νκ·Έλ‘ λ³νλλ€.

```MarkDown
# μ λͺ© 1
## μ λͺ© 2
### μ λͺ© 3
#### μ λͺ© 4
##### μ λͺ© 5
###### μ λͺ© 6
```

μ λͺ©1κ³Ό μ λͺ©2λ μλμ κ°μ΄ μ¬μ©ν μλ μλ€.


```MarkDown
μ λͺ© 1
=====

μ λͺ© 2
-----
```

## 2.2 κ°μ‘°(Emphasis)

 | μλ―Έ | λ§ν¬λ€μ΄ | μ€νκ²°κ³Ό
---|:---:|---:
`κΈ°μΈμ¬μ°κΈ°(italic)` | \*λ³ν νΉμ _μΈλλ°λ‘ κ°μΈμ€λ€ | *μ΄ν€λ¦­*
`λκ»κ²(bold)` | \**λλΈλ³ν νΉμ __λλΈμΈλλ° | **bold**
`μ·¨μμ (del)` | ~~λ¬Όκ²°νμ(tilde)  | ~~μ·¨μμ ~~
`λ°μ€` | `<u></u>` | <u>λ°μ€</u>
`μνμ ` | *** or --- or ___  |


## 2.3 λͺ©λ‘(List)

```MarkDown
1. μμκ° νμν λͺ©λ‘
2. μμκ° νμν λͺ©λ‘
    - μμκ° νμνμ§ μμ λͺ©λ‘(μλΈ)
    - μμκ° νμνμ§ μμ λͺ©λ‘(μλΈ)
3. μμκ° νμν λͺ©λ‘
    1. μμκ° νμν λͺ©λ‘(μλΈ)
    2. μμκ° νμν λͺ©λ‘(μλΈ)

- μμκ° νμνμ§ μμ λͺ©λ‘μ μ¬μ© κ°λ₯ν κΈ°νΈ
    - λμ¬ (hypen)
    * λ³ν (asterisks)
    + λνκΈ° (plus sign)
```

## 2.4 λ§ν¬(Link)

λ§ν¬λ `<a>`λ‘ λ³νλλ€.


μ’λ₯ | λ§ν¬λ€μ΄ | μ€νκ²°κ³Ό
--- | --- | ---
μΈλΌμΈ λ§ν¬ | `[λ§ν¬](https://www.google.co.kr "λΆμ°μ€λͺ")` | [Google](https://www.google.co.kr "κ΅¬κΈ")
μ°Έμ‘° λ§ν¬ | `[λ§ν¬1][1] & [1]: https://google.com "λΆμ°μ€λͺ")` | κ²μμμ§μ [Google][1]μ΄ μλ€. 
url λ§ν¬ | `<google.com/> & <example@example.com> ` | <https://google.com>
λ΄λΆ λ§ν¬ | `[λ§ν¬](#id)` | [λ§ν¬](https://google.com "κ΅¬κΈ")


<!-- μ°Έμ‘° λ§ν¬ μ§μ  -->
[1]:https://google.com "κ΅¬κΈ"


## 2.5 μ΄λ―Έμ§(images)

`<img>`λ‘ λ³νλλ€.


```bash
![μ΄λ―Έμ§ μ΄λ¦](μ΄λ―Έμ§ μ£Όμ "λΆμ° μ€λͺ")
```
μ’λ₯ | λ§ν¬λ€μ΄ | μ€νκ²°κ³Ό
--- | --- | ---
μ΄λ―Έμ§ | `![gatsby](./images/hello.png)` | ![gatsby](./images/hello.png)
μ΄λ―Έμ§μ λ§ν¬ κ±ΈκΈ° | `[![gatsby](./images/hello.png)](https://gatsby.com)` | [![gatsby](./images/hello.png)](https://gatsby.com)


> λ§ν¬λ€μ΄ μ΄λ―Έμ§ μ½λ `![]()`λ₯Ό λ§ν¬ μ½λ`[]()`λ‘ λ¬Άμ΄μ£Όλ©΄ μ΄λ―Έμ§μ λ§ν¬λ₯Ό κ±Έ μ μλ€.

## 2.6 λ°±ν±(`)μ μ΄μ©ν μ½λ(Code) κ°μ‘°

`<code>`λ‘ λ³νλλ€.

μ’λ₯ | λ§ν¬λ€μ΄ | μ€νκ²°κ³Ό
--- | --- | ---
μΈλΌμΈ | \`background\` | `background`
λΈλ‘ | \`\`\`html(μ½λ μ’λ₯ κΈ°μ¬) (μ½λ μμ±) \`\`\` | 


```MarkDown
    ```html
    <a href="https://www.google.co.kr/" target="_blank">GOOGLE</a>
    ```

    ```css {2}
    .list > li {
    position: absolute;
    top: 40px;
    }
    ```
```

```css {2}
    .list > li {
    position: absolute;
    top: 40px;
    }
```

## 2.7 ν(Table)

`<table>`λ‘ λ³νλλ€.

### - ν κ΅¬μ‘°

```MarkDown
| First Header  | Second Header | Third Header         |
| :------------ | :-----------: | -------------------: |
| First row     | Data          | Very long data entry |
| Second row    | **Cell**      | *Cell*               |
| Third row     | Cell that spans across two columns  ||
```

<!-- | First Header  | Second Header | Third Header         |
| :------------ | :-----------: | -------------------: |
| First row     | Data          | Very long data entry |
| Second row    | **Cell**      | *Cell*               |
| Third row     | Cell that spans across two columns  || -->

### - μ΄ λ³ν©(Column spanning)
```MarkDown
| Column 1 | Column 2 | Column 3 | Column 4 |
| -------- | :------: | -------- | -------- |
| No span  | Span across three columns    |||
```


> ν λ΄λΆμμ μ€ λ°κΏμ νκΈ° μν΄μλ `<br/>`μ μ¬μ©νλ€.


## 2.8 μΈμ©λ¬Έ(blockQuote)

`<blockquote>`λ‘ λ³νλλ€.

`>` κΈ°νΈλ₯Ό μ¬μ©νλ€.


> λ¨μ λ§μ΄λ κΈμμ μ§μ  λλ κ°μ μΌλ‘ λ°μ¨ λ¬Έμ₯.  
>  _(λ€μ΄λ² κ΅­μ΄ μ¬μ )_


> μΈμ©λ¬Έμ μμ±νμΈμ!
>> μ€μ²©λ μΈμ©λ¬Έ(nested blockquote)μ λ§λ€ μ μμ΅λλ€.
>>> μ€μ€μ²©λ μΈμ©λ¬Έ 1
>>> μ€μ€μ²©λ μΈμ©λ¬Έ 2
>>> μ€μ€μ²©λ μΈμ©λ¬Έ 3

## 2.9 κ°μ£Ό(Footnotes)
`[^λ΄μ©]`


λ³Έλ¬Έμ νΉμ  λ¬Έκ΅¬λ₯Ό λ³΄μΆ© μ€λͺνκΈ° μν μ©λ. μ£Όλ‘ λ΄μ©μ μΆμ²λ₯Ό λ°ν λ μ¬μ©.

```MarkDown
μλ¦­ λ μ΄λ¨Όλλ νμ΄μ¬μ λ°°μ΄μ§ νλ£¨λ§μ μνλ νλ‘κ·Έλ¨μ μμ±ν  μ μμλ€κ³  νλ€. [^myfootnote]

[^myfootnote]: μλ¦­ λ μ΄λ¨Όλλ νλ‘κ·Έλλ° κ²½νμ΄ λ§μ κ΅¬λ£¨ νλ‘κ·Έλλ¨Έμ΄λ€.  
λ³΄ν΅ μ¬λμ νμ΄μ¬μ λ°°μ°κ³  μ¬μ©νλ λ° 1μ£ΌμΌ μ λμ μ μ μκ°μ΄ νμν  κ²μ΄λ€.
```

ex) μλ¦­ λ μ΄λ¨Όλλ νμ΄μ¬μ λ°°μ΄μ§ νλ£¨λ§μ μνλ νλ‘κ·Έλ¨μ μμ±ν  μ μμλ€κ³  νλ€. [^myfootnote]

[^myfootnote]: μλ¦­ λ μ΄λ¨Όλλ νλ‘κ·Έλλ° κ²½νμ΄ λ§μ κ΅¬λ£¨ νλ‘κ·Έλλ¨Έμ΄λ€. λ³΄ν΅ μ¬λμ νμ΄μ¬μ λ°°μ°κ³  μ¬μ©νλ λ° 1μ£ΌμΌ μ λμ μ μ μκ°μ΄ νμν  κ²μ΄λ€.

## 2.10 μνμ 

κ° κΈ°νΈλ₯Ό 3κ° μ΄μ μλ ₯νμΈμ

`-`(Hyphens)

`*`(Asterisks)

`_`(Underscores)

## 2.11 μ€λ°κΏ

`<br>` νΉμ λμ΄μ°κΈ° 2λ²


## References

https://eungbean.github.io/2018/06/11/How-to-use-markdown/


https://heropy.blog/2017/09/30/markdown/

<br/>
<br/>
<br/>
<br/>

κ°μ£Ό [^myfootnote]