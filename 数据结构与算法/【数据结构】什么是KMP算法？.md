# 引言
> **Knuth-Morris-Pratt**字符串查找算法（简称为**KMP算法**）可在一个字符串S内查找一个词W的出现位置。一个词在不匹配时本身就包含足够的信息来确定下一个匹配可能的开始位置，此算法利用这一特性以避免重新检查先前匹配的字符。（引自 [WIKI-KMP算法](https://zh.wikipedia.org/wiki/KMP%E7%AE%97%E6%B3%95)）

​

# 实现
## 获取next数组
```java

    public static int[] getNext(char[] w) {
        int[] next = new int[w.length];
        // 初始化
        Arrays.fill(next, -1);
        int k = -1;
        int j = 0;

        while (j < w.length - 1) {
            if (k == -1 || w[k] == w[j]) {
                ++j;
                ++k;
                // 优化 aaaaaaaaaaaaaaaaaaaaaaaaaab 中查找 aaaaaaaaaaaaaaab
                if (w[k] == w[j]) {
                    next[j] = next[k];
                } else {
                    next[j] = k;
                }
//                next[j] = k;
            } else {
                k = next[k];
            }
        }

        return next;
    }
```
## 进行匹配
```java
  /**
     * 在主串中查找是否存在模式串并返回首次匹配的索引
     * @param p 主串
     * @param t 模式串
     * @return 返回初次匹配的索引, 如果没有匹配则返回<b>-1</b>
     */
    public static int kmp(char[] p, char[] t) {
        int i = 0;
        int j = 0;
        int[] next = getNext(t);
        while (j <= t.length - 1 && i <= p.length - 1) {
            if (j == -1 || p[i] == t[j]) {
                i++;
                j++;
            } else {
                j = next[j];
            }
        }
        if (j > t.length - 1) {
            return i - j;
        }
        return -1;
    }
```
