# Q29 两数相除

#### 题目描述

不使用乘法、除法以及mod运算符，实现两整数相除，结果截去小数部分。输入保证除数(divisor)不为0，如果结果溢出，则返回Integer.MAX_VALUE

#### 解题思路

作差法求解。

将被除数(dividend)与除数(divisor)不断作差，直至被除数小于除数。

除法结果用long型数据保存，以避免溢出。

每次作差之后被除数翻倍，以提高作差效率，以避免超时。

#### Java代码实现

```java
package com.peng.leetcode;

/**
 * 不使用乘法、除法和mod运算符，实现两整数相除，结果截去小数部分
 * 输入保证除数不为0
 */
public class Q29 {


    public static void main(String[] args) {
        System.out.println(divide(-2147483648, 1));

    }


    /**
     * 思路：通过作差法实现除法，使用long型数据来解决溢出问题，通过被减数（被除数）的翻倍与还原避免超时
     * @param dividend
     * @param divisor
     * @return
     */
    public static int divide(int dividend, int divisor) {

        long result = 0;

        boolean flag = false;  //true表示结果为正，false表示结果为负

        if((dividend > 0 && divisor > 0) || (dividend < 0 && divisor < 0)) {
            flag = true;
        }

        long a = dividend;
        long b = divisor;

        a = Math.abs(a);
        b = Math.abs(b);

        int k = 1;
        long c = b;

        while (a >= b) {
            result += k;
            a -= b;

            if(a >= b*2) {  //每做差一次就将减数翻倍，提高做差效率，避免超时
                b *= 2;
                k *= 2;
            } else {  //不够减时，则将减数还原为原来的大小
                b = c;
                k = 1;
            }

        }

        if(!flag) {
            result = -result;
        }

        if(result < Integer.MIN_VALUE || result > Integer.MAX_VALUE) {
            return Integer.MAX_VALUE;
        }

        return (int)result;
    }

}

```



# Q30 串联所有单词的子串

#### 题目描述

给定一个字符串 `s` 和一些 **长度相同** 的单词 `words` **。**找出 `s` 中**恰好**可以由 `words` 中所有单词串联形成的子串的起始位置。

注意子串要与 `words` 中的单词完全匹配，**中间不能有其他字符**，不必考虑words中单词的顺序，words中可能包含相同的单词。

#### 解题思路

如果子串是恰好由words中所有单词串联而成，则说明子串和words数组的构成成分一样（即words数组中包含n个单词hello，则子串中也包含n个hello）。

通过words中的单词及其出现次数作为key-value构建一个Map，再通过依次截取的子串中的单词及其出现次数作为key-value构建一个Map，通过比较两个Map是否相等即可判断该子串是否恰好由words中所有单词串联而成。

#### Java代码实现

```java
package com.peng.leetcode;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

/**
 * 给定一个字符串 s 和一些 长度相同 的单词 words 。找出 s 中恰好可以由 words 中所有单词串联形成的子串的起始位置。
 */
public class Q30 {

    public static List<Integer> findSubstring(String s, String[] words) {

        List<Integer> result = new ArrayList<>();

        //处理非法输入
        if(s == null || s.length() == 0 || words == null || words.length == 0) {
            return result;
        }

        //初始化作为参照的Map
        HashMap<String, Integer> mapWords = new HashMap<>();
        for(int i = 0; i < words.length; i++) {
            mapWords.put(words[i], mapWords.getOrDefault(words[i], 0) + 1);
        }

        int wordsLength = words.length * words[0].length();

        for(int i = 0; i <= s.length() - wordsLength; i++) {
            HashMap<String, Integer> mapS = new HashMap<>();  //待比较的Map

            for(int j = i; j <= i + wordsLength - words[0].length(); j += words[0].length()) {
                String str = s.substring(j, j + words[0].length());
                mapS.put(str, mapS.getOrDefault(str, 0) + 1);  
            }

            if(mapS.equals(mapWords)) {
                result.add(i);
            }
        }

        return result;
    }

}

```



