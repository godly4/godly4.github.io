---
layout: default
date: 2017-08-16
---
{{ page.date | date: '%B %d, %Y' }}
# 题目

> 编写一个程序解决选择问题。令k=N/2。画出表格显示你的程序对于N为不同值的运行时间

## 解法1：冒泡排序后，直接取出第k个值
```java
import java.time.Duration;
import java.time.Instant;
import java.util.Arrays;
import java.util.Random;
import java.util.Scanner;

/**
 * @Author dlgao
 * @Date 16/08/2017
 * @Description
 */
public class SelectProblem
{
    private void getKNumberBubble(int[] array, int k)
    {
        Instant begin = Instant.now();
        for (int i = 1; i < array.length; i++)
        {
            for (int j = 0; j < array.length - i; j++)
            {
                if (array[j] < array[j + 1])
                {
                    int tmp = array[j];
                    array[j] = array[j + 1];
                    array[j + 1] = tmp;
                }
            }
        }
        System.out.println("第" + k + "个值为: " + array[k - 1]);
        Instant end = Instant.now();
        Duration duration = Duration.between(begin, end);
        System.out.println("耗时: " + duration.toMillis() + "毫秒");
    }

    public static void main(String[] args) throws Exception
    {
        int N = new Scanner(System.in).nextInt();
        int[] array = new int[N];
        for (int i = 0; i < N; i++)
        {
            array[i] = new Random().nextInt(N);
        }
        SelectProblem problem = new SelectProblem();
        System.out.println(Arrays.toString(array));
        problem.getKNumberBubble(array.clone(), N / 2);
    }
}
```
## 解法2：设置k维数组，每次更新该数组，尔后取最后一位的值。
```java
// 插入排序优化版?
private void getKNumberArray(int[] array, int k)
{
    Instant begin = Instant.now();
    int[] newArray = new int[k];
    newArray[0] = array[0];
    int i, j;
    for (i = 1; i < k; i++)
    {
        // 若小于第k-1位则后面的for循环直接就跳过了，因而这里需要先赋值
        newArray[i] = array[i];
        for (j = i; j > 0; j--)
        {
            // j-1会覆盖掉j处的值，所以需要使用array[i]来保存该值
            if (array[i] > newArray[j - 1])
            {
                newArray[j] = newArray[j - 1];
                newArray[j - 1] = array[i];
            }
        }
    }
    for (i = k; i < array.length; i++)
    {
        if (array[i] <= newArray[k - 1])
            continue;
        newArray[k - 1] = array[i];
        for (j = k - 1; j > 0; j--)
        {
            if (array[i] > newArray[j - 1])
            {
                newArray[j] = newArray[j - 1];
                newArray[j - 1] = array[i];
            }
        }
    }
    System.out.println("第" + k + "个值为: " + newArray[k - 1]);
    Instant end = Instant.now();
    Duration duration = Duration.between(begin, end);
    System.out.println("耗时: " + duration.toMillis() + "毫秒");
}
```
