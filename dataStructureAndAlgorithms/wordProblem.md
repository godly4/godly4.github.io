---
layout: default
date: 2017-08-16
---
{{ page.date | date: '%B %d, %Y' }}
# 题目

> 编写一个程序求解字谜游戏问题。

## 解法：书上给出了2种比较直观的算法。
> 第一种：对单词表中的每个单词，我们检查每一个有序三元组（行，列，方向），验证是否有单词存在。但坏处是这将导致大量嵌套的for循环。

> 第二种：对于每一个尚未进行到字谜最后的有序四元组（行，列，方向，字符数）我们可以测试所指的单词是否在单词表中。这也导致使用大量嵌套的for循环。如果在任意单词中的最大字符数已知的情况下，那么该算法有可能节省一些时间。

```java
/**
 * @Author dlgao
 * @Date 16/08/2017
 * @Description
 */
public class WordProblem
{
    private char[][] puzzle = {
            {'t', 'h', 'i', 's'},
            {'w', 'a', 't', 's'},
            {'o', 'a', 'h', 'g'},
            {'f', 'g', 'd', 't'},
    };
    private String[] dict = {"this", "two", "fat", "that"};

    private void getWord(int x, int y, int n, int direct)
    {
        StringBuilder sb = new StringBuilder();
        for (int i = 1; i <= n; i++)
        {
            if (x < 0 || x >= 4 || y < 0 || y >= 4)
            {
                return;
            }
            sb.append(puzzle[x][y]);
            for (int j = 0; j < dict.length; j++)
            {
                if (sb.toString().equalsIgnoreCase(dict[j]))
                {
                    System.out.println("Get word '" + dict[j] + "'");
                    return;
                }
            }
            switch (direct)
            {
                case 0:
                    y++;
                    break;
                case 1:
                    y--;
                    break;
                case 2:
                    x++;
                    break;
                case 3:
                    x--;
                    break;
                case 4:
                    x--;
                    y++;
                    break;
                case 5:
                    x++;
                    y++;
                    break;
                case 6:
                    x++;
                    y--;
                    break;
                case 7:
                    x--;
                    y--;
                    break;
            }
        }
    }

    public static void main(String[] args)
    {
        int i, j, n, direct;
        WordProblem wordProblem = new WordProblem();
        for (i = 0; i < 4; i++)
        {
            for (j = 0; j < 4; j++)
            {
                //　总共8个方向
                for (direct = 0; direct < 8; direct++)
                {
                    // 最大字符长度为4
                    for (n = 1; n <= 4; n++)
                    {
                        wordProblem.getWord(i, j, n, direct);
                    }
                }
            }
        }
    }
}
```
