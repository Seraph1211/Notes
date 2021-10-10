# Q29 两数相除

#### 题目描述：

不使用乘法、除法以及mod运算符，实现两整数相除，结果截去小数部分。输入保证除数(divisor)不为0，如果结果溢出，则返回Integer.MAX_VALUE

#### 思路：

作差法求解。

将被除数(dividend)与除数(divisor)不断作差，直至被除数小于除数。

除法结果用long型数据保存，以避免溢出。

每次作差之后被除数翻倍，以提高作差效率，以避免超时。

#### Java代码实现：

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

