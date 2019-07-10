---
title: Sharding-JDBC 源码分析 -- SQL解析（1）之 词法分析
date: 2019-07-09 10:06:46
category: 源码
tags: 
 - 源码
 - 数据中间件
 - sharding-jdbc
 - 词法分析
---

# 前言
学习数据中间件的时候，我们避免不了要学习SQL解析引擎，就从最基础的词法分析一步一步解读下去。
首先我们从github checkout [sharding-jdbc](https://github.com/apache/incubator-shardingsphere)的源码,
我基于3.1.0这个标签checkout了一份，并且做了一些注释，所以这些文章我都是基于这个版本的源码做分析，感兴趣的同学可以去checkout我带注释的源码看看[https://github.com/JJS857012499/incubator-shardingsphere](https://github.com/JJS857012499/incubator-shardingsphere)

<!-- more -->

# Lexer 词法分析器
lexer词法分析器位于sharding-core项目的io.shardingsphere.core.parsing.lexer包下

lexer.java 部分源码
```java
public class Lexer {

    /**
     * 输入sql语句
     */
    @Getter
    private final String input;

    /**
     * 词法标记字典
     */
    private final Dictionary dictionary;

    /**
     * 解析SQL的offer
     */
    private int offset;

    /**
     * 当前的词法标记
     */
    @Getter
    private Token currentToken;

    /**
     * 分析下个词法标记
     * Analyse next token.
     */
    public final void nextToken() {
        skipIgnoredToken();
        if (isVariableBegin()) {
            //变量
            currentToken = new Tokenizer(input, dictionary, offset).scanVariable();
        } else if (isNCharBegin()) {
            // N
            currentToken = new Tokenizer(input, dictionary, ++offset).scanChars();
        } else if (isIdentifierBegin()) {
            // Keyword + Literals.IDENTIFIER，判断是单词、'`' 、'_'、'$'
            currentToken = new Tokenizer(input, dictionary, offset).scanIdentifier();
        } else if (isHexDecimalBegin()) {
            // 16进制
            currentToken = new Tokenizer(input, dictionary, offset).scanHexDecimal();
        } else if (isNumberBegin()) {
            // 数字
            currentToken = new Tokenizer(input, dictionary, offset).scanNumber();
        } else if (isSymbolBegin()) {
            // 符号
            currentToken = new Tokenizer(input, dictionary, offset).scanSymbol();
        } else if (isCharsBegin()) {
            // 字符串
            currentToken = new Tokenizer(input, dictionary, offset).scanChars();
        } else if (isEnd()) {
            // 结束
            currentToken = new Token(Assist.END, "", offset);
        } else {
            //sql 语法错误
            throw new SQLParsingException(this, Assist.ERROR);
        }
        offset = currentToken.getEndPosition();
    }

    /**
     * 跳过忽略的词法标记
     * 1、空格
     * 2、sql hint
     * 3、注释
     */
    private void skipIgnoredToken() {
        // 空格
        offset = new Tokenizer(input, dictionary, offset).skipWhitespace();
        // hint （例如mysql的的 sql_no_cache 等等）
        while (isHintBegin()) {
            offset = new Tokenizer(input, dictionary, offset).skipHint();
            offset = new Tokenizer(input, dictionary, offset).skipWhitespace();
        }
        // 注释
        while (isCommentBegin()) {
            offset = new Tokenizer(input, dictionary, offset).skipComment();
            offset = new Tokenizer(input, dictionary, offset).skipWhitespace();
        }
    }
}

```
Lexer 词法分析器有四个字段，分别是输入的SQL语句（input）、词法标记字典（dictionary）、解析SQL的偏移量（offer）和当前的词法标记（currentToken）。
Lexer词法分析器的原理：顺序解析SQL,将字符串拆解成N个词法。
实现方式：通过不断调用Lexer#nextToken()方法，不断分析当前偏移量（offer）下的Token（词法标记），在#nextToken()方法中，我们可以看到他通过调用#skipIgnoredToken()方法跳过忽略的token，通过isXX()方法判断下一个Token类型后，交给Tokenizer标记器进行分词处理并返回Token。

# Token 词法标记
Token有三个属性
* TokenType type ：词法标记类型
* String literals ：词法字面量
* int endPosition ：词法字面量（literals） 在SQL的结束位置

## TokenType 词法标记类型
TokenType 词法标记类型有四大类
* Keywork: 词法关键字标记， SQL关键字
* Literals: 词法字面量标记， 数字类型、词法关键字（表名，字段等）、变量
* Symbol： 词法符号标记， 运算符以及括号、中括号、花括号
* Assist：词法辅助标记，结束符号，错误
![logo](1562642225.jpg)

## Keyword 词法关键字标记
不同的数据方言会有不同的关键字，所以Keyword定义为接口，他的实现有DefaultKeyword（默认关键字标记）和不同数据库关键字标记（h2、mysql、oracle、postgresql、sqlserver等等）。
![Keywork](1562643150.jpg)
![Lexer](1562651407.jpg)

我们看看MySQLLexer类
```java
public final class MySQLLexer extends Lexer {

    /**
     * 词法标记字典，因为是mysql，所有加载的是Mysql关键字标记
     */
    private static Dictionary dictionary = new Dictionary(MySQLKeyword.values());
    
    public MySQLLexer(final String input) {
        super(input, dictionary);
    }
    
    @Override
    protected boolean isHintBegin() {
        return '/' == getCurrentChar(0) && '*' == getCurrentChar(1) && '!' == getCurrentChar(2);
    }
    
    @Override
    protected boolean isCommentBegin() {
        return '#' == getCurrentChar(0) || super.isCommentBegin();
    }
    
    @Override
    protected boolean isVariableBegin() {
        return '@' == getCurrentChar(0);
    }
}
```

Dictionary.java类
```java
public final class Dictionary {

    /**
     * 词法标记字典集合
     */
    private final Map<String, Keyword> tokens = new HashMap<>(1024);

    /**
     *
     * @param dialectKeywords
     */
    public Dictionary(final Keyword... dialectKeywords) {
        fill(dialectKeywords);
    }

    /**
     * 填充标记关键字
     * 加入默认标记关键字后再加入指定的标记关键字
     * @param dialectKeywords
     */
    private void fill(final Keyword... dialectKeywords) {
        //加入默认标记关键字
        for (DefaultKeyword each : DefaultKeyword.values()) {
            tokens.put(each.name(), each);
        }
        //加入指定的标记关键字
        for (Keyword each : dialectKeywords) {
            tokens.put(each.toString(), each);
        }
    }
    
    TokenType findTokenType(final String literals, final TokenType defaultTokenType) {
        String key = null == literals ? null : literals.toUpperCase();
        return tokens.containsKey(key) ? tokens.get(key) : defaultTokenType;
    }
    
    TokenType findTokenType(final String literals) {
        String key = null == literals ? null : literals.toUpperCase();
        if (tokens.containsKey(key)) {
            return tokens.get(key);
        }
        throw new IllegalArgumentException();
    }
}
```
别的数据库词法分析器也是类似，这里就不一一枚举了，如果没有自己想要的数据库分析器，我们可以继承Lexer实现。
Keyword + Literals.IDENTIFIER的解析是一起的，我们继续往下看。

## Literals 词法字面量标记
词法字面量标记 分为6种
* INT: 整数
* FLOAT: 浮点数
* HEX: 16进制
* chars: 字符串
* IDENTIFIER: 词法关键词
* VARIABLE: 变量

### Literals.VARIABLE 词法变量
词法变量

```java

    // MySQLLexer.java
    @Override
    protected boolean isVariableBegin() {
        return '@' == getCurrentChar(0);
    }
    
    //Lexer.java
    /**
     * 扫描词法变量
     * 例如：
     * select * from commodity.mall_commodity_info where PRODUCT_ID=@id 的 @id
     * select @@version 的 @@version
     * scan variable.
     *
     * @return variable token
     */
    public Token scanVariable() {
        int length = 1;
        if ('@' == charAt(offset + 1)) {
            length++;
        }
        while (isVariableChar(charAt(offset + length))) {
            length++;
        }
        return new Token(Literals.VARIABLE, input.substring(offset, offset + length), offset + length);
    }

    private boolean isVariableChar(final char ch) {
        return isIdentifierChar(ch) || '.' == ch;
    }
    
```

### Literals.IDENTIFIER 词法关键词
词法关键词，例如：数据库名、表名、字段等等
Keyword + Literals.IDENTIFIER的解析核心代码
```java
    //Lexer.java
    private boolean isIdentifierBegin() {
        return isIdentifierBegin(getCurrentChar(0));
    }

    protected boolean isIdentifierBegin(final char ch) {
        return CharType.isAlphabet(ch) || '`' == ch || '_' == ch || '$' == ch;
    }

    //CharType.java
    /**
    * 是否是字母
    */
    public static boolean isAlphabet(final char ch) {
        return ch >= 'A' && ch <= 'Z' || ch >= 'a' && ch <= 'z';
    }
    
    // Tokenizer.java
    /**
     * 扫描 标识符
     * scan identifier.
     *
     * @return identifier token
     */
    public Token scanIdentifier() {

        // `字段`， 例如 select `id` from table_xx 中的 `id`
        if ('`' == charAt(offset)) {
            int length = getLengthUntilTerminatedChar('`');
            return new Token(Literals.IDENTIFIER, input.substring(offset, offset + length), offset + length);
        }
        if ('"' == charAt(offset)) {
            int length = getLengthUntilTerminatedChar('"');
            return new Token(Literals.IDENTIFIER, input.substring(offset, offset + length), offset + length);
        }
        if ('[' == charAt(offset)) {
            int length = getLengthUntilTerminatedChar(']');
            return new Token(Literals.IDENTIFIER, input.substring(offset, offset + length), offset + length);
        }

        int length = 0;
        while (isIdentifierChar(charAt(offset + length))) {
            length++;
        }
        String literals = input.substring(offset, offset + length);
        if (isAmbiguousIdentifier(literals)) {
            // 模棱两可的， order 或者 group
            //如果空格后面跟着 by 关键字 则TokenType是dictionary里面的KeyWord， 否则Literals.IDENTIFIER
            return new Token(processAmbiguousIdentifier(offset + length, literals), literals, offset + length);
        }
        // TokenType：根据literals 获取 dictionary里面的Keyword， 否则为默认Literals.IDENTIFIER
        return new Token(dictionary.findTokenType(literals, Literals.IDENTIFIER), literals, offset + length);
    }


    /**
     * 计算到结束符的长度
     *
     * @param terminatedChar
     * @return
     * @see #hasEscapeChar,  select * from table_xx as `a``b` 这里连续的"``"并非结束符，如果此时传递的是"'",则会误判，所以这里加了hasEscapeChar判断
     */
    private int getLengthUntilTerminatedChar(final char terminatedChar) {
        int length = 1;
        while (terminatedChar != charAt(offset + length) || hasEscapeChar(terminatedChar, offset + length)) {
            if (offset + length >= input.length()) {
                throw new UnterminatedCharException(terminatedChar);
            }
            if (hasEscapeChar(terminatedChar, offset + length)) {
                length++;
            }
            length++;
        }
        return length + 1;
    }

    /**
     * 是否有 Escape 字符
     *
     * @param charIdentifier
     * @param offset
     * @return
     */
    private boolean hasEscapeChar(final char charIdentifier, final int offset) {
        return charIdentifier == charAt(offset) && charIdentifier == charAt(offset + 1);
    }

```

### Literals.HEX 16进制
16进制,表示0x开头，后面跟着 -（负号）、 A~F、a~f和数字
```java
    
    //Lexer.java
    private boolean isHexDecimalBegin() {
        return '0' == getCurrentChar(0) && 'x' == getCurrentChar(1);
    }
    
    /**
     * scan hex decimal.
     *
     * @return hex decimal token
     */
    public Token scanHexDecimal() {
        int length = HEX_BEGIN_SYMBOL_LENGTH;
        //处理负号
        if ('-' == charAt(offset + length)) {
            length++;
        }
        while (isHex(charAt(offset + length))) {
            length++;
        }
        return new Token(Literals.HEX, input.substring(offset, offset + length), offset + length);
    }
    
    private boolean isHex(final char ch) {
        return ch >= 'A' && ch <= 'F' || ch >= 'a' && ch <= 'f' || CharType.isDigital(ch);
    }
     
```

### Literals.INT 数字
数字，和Literals.FLOAT一起分析
有如下情况
* 123
* -123

### Literals.FLOAT 浮点数
浮点数，有如下情况
* .1 :表示 0.1
* -. :表示 -0.1
* -1 :表示 -1
* 3e2:表示科学计数法的3*10^2

```java

    //Lexer.java
    private boolean isNumberBegin() {
        return CharType.isDigital(getCurrentChar(0))
                || ('.' == getCurrentChar(0) && CharType.isDigital(getCurrentChar(1)) && !isIdentifierBegin(getCurrentChar(-1))
                || ('-' == getCurrentChar(0) && ('.' == getCurrentChar(1) || CharType.isDigital(getCurrentChar(1)))));
    }
    
    //Tokenizer.java
    /**
     * scan number.
     *
     * @return number token
     */
    public Token scanNumber() {
        int length = 0;
        if ('-' == charAt(offset + length)) {
            length++;
        }
        length += getDigitalLength(offset + length);
        boolean isFloat = false;
        if ('.' == charAt(offset + length)) {
            isFloat = true;
            length++;
            length += getDigitalLength(offset + length);
        }
        if (isScientificNotation(offset + length)) {
            isFloat = true;
            length++;
            if ('+' == charAt(offset + length) || '-' == charAt(offset + length)) {
                length++;
            }
            length += getDigitalLength(offset + length);
        }
        if (isBinaryNumber(offset + length)) {
            isFloat = true;
            length++;
        }
        return new Token(isFloat ? Literals.FLOAT : Literals.INT, input.substring(offset, offset + length), offset + length);
    }
    
    private int getDigitalLength(final int offset) {
        int result = 0;
        while (CharType.isDigital(charAt(offset + result))) {
            result++;
        }
        return result;
    }

    private boolean isScientificNotation(final int offset) {
        char current = charAt(offset);
        return 'e' == current || 'E' == current;
    }

    private boolean isBinaryNumber(final int offset) {
        char current = charAt(offset);
        return 'f' == current || 'F' == current || 'd' == current || 'D' == current;
    }

```

### Literals.CHARS 字符串
字符串，表示以单引号或者双引号开始的字符串
例如 ：select * from commodity.mall_commodity_info where PRODUCT_ID="102dc49d5c8e4d79a21e35a619a4ece" 的"102dc49d5c8e4d79a21e35a619a4ece"

```java
    
    //Lexer.java
    protected boolean isCharsBegin() {
        return '\'' == getCurrentChar(0) || '\"' == getCurrentChar(0);
    }

    //Tokenizer.java
    /**
     * scan chars.
     *
     * @return chars token
     */
    public Token scanChars() {
        return scanChars(charAt(offset));
    }

    private Token scanChars(final char terminatedChar) {
        int length = getLengthUntilTerminatedChar(terminatedChar);
        return new Token(Literals.CHARS, input.substring(offset + 1, offset + length - 1), offset + length);
    }

```
### Literals.SYMBOL 
符号，例如 < > = << % 等等

```java
    
    //Lexer.java
    private boolean isSymbolBegin() {
        return CharType.isSymbol(getCurrentChar(0));
    }

    //Tokenizer.java
    /**
     * scan symbol.
     *
     * @return symbol token
     */
    public Token scanSymbol() {
        int length = 0;
        while (CharType.isSymbol(charAt(offset + length))) {
            length++;
        }
        String literals = input.substring(offset, offset + length);
        Symbol symbol;
        while (null == (symbol = Symbol.literalsOf(literals))) {
            literals = input.substring(offset, offset + --length);
        }
        return new Token(symbol, literals, offset + length);
    }

```

## Assist 词法辅助标记
Assist分为两种
* END : 结束符
* ERROR: 错误符

# LexerEngine 词法分析器引擎
这个类主要是对lexer类做了一下封装，有个成员变量Lexer lexer，由构造函数传入不同数据库方言实现的Lexer，
我们先看看他主要的方法#skipParentheses，为后面的SQL语句分析留个印象。
```java
@RequiredArgsConstructor
public final class LexerEngine {
    
    private final Lexer lexer;
        
    /**
     * 跳过括号内的所有语法标记
     * skip all tokens that inside parentheses.
     *
     * @param sqlStatement SQL statement
     * @return skipped string 跳过的字符串
     */
    public String skipParentheses(final SQLStatement sqlStatement) {
        StringBuilder result = new StringBuilder("");
        int count = 0; //记录多重括号问题，左括号加一，右括号减一，当count为0的时候才是闭合的情况
        if (Symbol.LEFT_PAREN == lexer.getCurrentToken().getType()) {
            final int beginPosition = lexer.getCurrentToken().getEndPosition();
            result.append(Symbol.LEFT_PAREN.getLiterals());
            lexer.nextToken();
            while (true) {
                if (equalAny(Symbol.QUESTION)) {
                    sqlStatement.increaseParametersIndex();
                }

                //结束符，或者当前是右括号并且已经闭合的情况，跳出循环
                if (Assist.END == lexer.getCurrentToken().getType() || (Symbol.RIGHT_PAREN == lexer.getCurrentToken().getType() && 0 == count)) {
                    break;
                }

                //左括号加一，右括号减一
                if (Symbol.LEFT_PAREN == lexer.getCurrentToken().getType()) {
                    count++;
                } else if (Symbol.RIGHT_PAREN == lexer.getCurrentToken().getType()) {
                    count--;
                }
                lexer.nextToken();
            }
            result.append(lexer.getInput().substring(beginPosition, lexer.getCurrentToken().getEndPosition()));
            lexer.nextToken();
        }
        return result.toString();
    }
}
```

# LexerEngineFactory LexerEngine 工厂类
这个没什么好说，就是LexerEngine 工厂类,根据不同的DatebaseType 生成不同的LexerEngine实例。



