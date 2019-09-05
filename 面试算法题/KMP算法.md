#### 字符串朴素匹配算法，时间复杂度 O（n*m）

#### KMP：O（m+n）



#### 暴力搜索

![1567683082305](C:\Users\ADMINI~1\AppData\Local\Temp\1567683082305.png)



#### 移动一位，再次匹配，也就会做很多无用功

![1567683126929](C:\Users\ADMINI~1\AppData\Local\Temp\1567683126929.png)





#### KMP 

![1567683207354](C:\Users\ADMINI~1\AppData\Local\Temp\1567683207354.png)



![1567683306957](C:\Users\ADMINI~1\AppData\Local\Temp\1567683306957.png)



#### 公共前后缀不能取字串本身，那样就没有意义了，因为，不能向右移动

![1567678123955](C:\Users\ADMINI~1\AppData\Local\Temp\1567678123955.png)



#### 模式串的公共前后缀

![1567677509857](C:\Users\ADMINI~1\AppData\Local\Temp\1567677509857.png)



#### 向后移动模式串，使得模式串的前缀到达模式串的后缀位置。

![1567677586163](C:\Users\ADMINI~1\AppData\Local\Temp\1567677586163.png)



#### 这样就可以保证当前指针所指向的元素的左边是匹配的。（因为公共前后缀是一样的）

![1567677656274](C:\Users\ADMINI~1\AppData\Local\Temp\1567677656274.png)



#### 模式串长度超过主串长度，所以不匹配，退出

![1567677963917](C:\Users\ADMINI~1\AppData\Local\Temp\1567677963917.png)



#### 模式串可以和任何主串发生KMP算法，所以模式串的每一个位置都有可能发生不匹配。

#### 我们定义一个数组，数组长度为模式串长度加1，0号位置不存储元素

![1567679654729](C:\Users\ADMINI~1\AppData\Local\Temp\1567679654729.png)



#### 假如第一个位置就发生了不匹配，那么我们怎么把模式串的移动一位然后重新开始比较，翻译为不含有串的移动的表达方式，因为串在内存种不会像我们绘图一样移动，即使我们要移动，也需要花费时间复杂度，所以我们需要寻找另外一种表达方式。



![1567679758071](C:\Users\ADMINI~1\AppData\Local\Temp\1567679758071.png)



#### =====》模式串一号位置字符，与主串下一个位置开始比较。

![1567680156162](C:\Users\ADMINI~1\AppData\Local\Temp\1567680156162.png)



#### 如果2号位置发生不匹配（前提是1号位置已经匹配了），那么我们观察发现，2号位置之前的最长公共前后缀，长度为0（因为我们这儿求的最长公共前后缀不包括本身字符串）我们在公共前缀长度上加1，等于0+1=1，所以我们将1号位置和主串当前位置进行匹配。

（注意：这儿X左边得元素可能是由于作者写错了，应该是A，而不是B）

![1567680397351](C:\Users\ADMINI~1\AppData\Local\Temp\1567680397351.png)



#### 移动之后

![1567680462180](C:\Users\ADMINI~1\AppData\Local\Temp\1567680462180.png)





![1567680502355](C:\Users\ADMINI~1\AppData\Local\Temp\1567680502355.png)





#### 公共前后缀长度为1，所以2号位置和主串当前位置进行匹配

![1567680551812](C:\Users\ADMINI~1\AppData\Local\Temp\1567680551812.png)



#### ===》也就是这样移动后，进行匹配

![1567680601888](C:\Users\ADMINI~1\AppData\Local\Temp\1567680601888.png)



#### 如果当前位置发生不匹配，公共前后缀长度为n，那么下一次匹配得规则是n+1号位置和主串当前位置进行比较



#### 只有第一句话表述和其他不一样，我们需要把它单独提出来，我们将第一句话设为0，其他设为i。

![1567681116775](C:\Users\ADMINI~1\AppData\Local\Temp\1567681116775.png)



#### 根据模式串提供得这个next数组，我们在以后匹配过程中，发生任何一次不匹配，我们都知道下一次该怎么做了，所以这个叫做next数组。比如，我们7号位置发生不匹配，我们就知道下一步得操作：将模式串得2号位置与主串当前位置进行比较。

![1567681178226](C:\Users\ADMINI~1\AppData\Local\Temp\1567681178226.png)



#### 下面我们还需要用算法来实现next数组得求解

![1567683434150](C:\Users\ADMINI~1\AppData\Local\Temp\1567683434150.png)





#### 暴力算法

```C++
typedef struct{
    char str[maxSize+1];
    int length;
} Str

typedef struct{
    char *ch;
    int length;
} Str

Str S;
S.length = L;
S.ch = (char *) malloc((L+1)*sizeof(char));
S.ch[length范围内得位置] = 某字符变量
某字符变量 = S.ch[length范围内得位置]

定长得不用我们自己释放空间，系统自动释放，而变长需要我们手动释放存储空间
free(S.ch)
    



int naive(Str str, Str substr){
    int i=1,j=1,k=i;
    while(i<=str.length && j<=substr.length){
        if(str.ch[i]==substr.ch[j]){
            ++i;
            ++j;
        }
        else{
            j=1;
            i = ++k;
        }
    }
    if (j>substr.length){
        return k;
    }
    else
    	return 0;
}
```



![1567689243126](C:\Users\ADMINI~1\AppData\Local\Temp\1567689243126.png)



![1567689281322](C:\Users\ADMINI~1\AppData\Local\Temp\1567689281322.png)



#### SK往SK+1跳，我们利用的就是next进行跳，可以翻译为t重新指向新的值。这儿，可能需要跳多次t

![1567689318453](C:\Users\ADMINI~1\AppData\Local\Temp\1567689318453.png)



![1567689393720](C:\Users\ADMINI~1\AppData\Local\Temp\1567689393720.png)





```C++

int KMP(Str str, Str substr, int next[])
{
    int i=1,j=1;
    # 因为我们不需要回溯，所以不需要k这个变量了
    while(i<=str.length && j<=substr.length)
    {
        # j=0,进入if后，i从下一个位置开始，j从1的位置开始，确实有点叼
    	if(j==0 || str.ch[i]==substr.ch[j])
        {
            ++i;
            ++j;
        }
        else
        {
            j = next[j];
        }
    }
    if(j>substr.length)
        # 计算与模式串匹配的首字符位置
        return i-substr.length;
    else
        return 0;
}




求next数组
void getNext(Str substr, int next[])
{
    int j = 1,len = 0;
    next[j] = 0;
    while(j<substr.length)
    {
        # len==0,表示我们从头开始进行匹配
        if(len==0 || substr.ch[j] == substr.ch[len])
        {
            next[j+1] = len+1;
            ++len;
            ++j;
        }
        else
        {
         	len = next[len];   
        } 
    }
}
```





#### 海外留学生版

```C++
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void func(char str[], int next[], int n)
{
    next[0] = 0;
    int len = 0;
    int i = 1;
    while(i<n)
    {
        if(str[i] == str[len])
        {
            len++;
            next[i] = len;
            i++;
        }
        else
        {
            if (len>0)
            	len = str[len-1];
            else
                next[i] = len;
            	i++;
        }
    }
}


void help(int next[], int n)
{
    int i;
    for(i=n-1;i>0;i--)
    {
        next[i] = next[i-1];
    }
    next[0] = -1
}

void KMP(char text[], char substr[])
{
    int m = strlen(text);
    int n = strlen(substr)
    int * next = malloc(sizeof(int)*n);
    help(substr, n);
    
    // text[i]  len(text) = m
    // substr[j] len(substr) = n
    int i=0;
    int j=0;
    while(i<m)
    {
        // 打印匹配字符串
        if(j==n-1 && text[i]==substr[j])
        {
            printf("find, %d\n",i-j);
            j = next[j];
        }
        
        if(text[i] == substr[j])
        {
            i++;
            j++;
        }
        else
        {
            j = next[j];
            if(j==-1)
            {
                i++;
                j++;
            }
        }
    }
}

int main(void)
{
    char text[] = "ABABABA";
    char substr[] = "ABA";
    KMP(text, substr);
}

```





![1567693506236](C:\Users\ADMINI~1\AppData\Local\Temp\1567693506236.png)



```python
def func(arr):
    j = 1
    n = len(arr)
    next = [0] * (n)
    num = 0
    while j < n:
        if arr[j] == arr[num]:
            next[j] = num+1
            num += 1
            j += 1
        else:
            if num>0:
                num = next[num-1]
            else:
                next[j] = num
                j += 1
    print(next)
    # [0, 0, 1, 2, 0, 1, 2, 3, 1]
    return next

# 我们向后移动一个位置，更有利于我们用KMP算法
def help(next):
    for i in range(len(next)-1, 0,-1):
        next[i] = next[i-1]
    next[0] = -1
    # [-1, 0, 0, 1, 2, 0, 1, 2, 3]
    print(next)

def KMP(old, new):
    n = len(new)
    next = func(new)
    help(next)
    m = len(old)
    i=0
    j=0
    while i<m:
        if j==n-1 and old[i] == new[j]:
            print('find')
            print(i-j)
            j = next[j]
        if old[i] == new[j]:
            i+=1
            j+=1
        else:
            j = next[j]
            if j==-1:
                i+=1
                j+=1


arr = list('ABABCABAA')
old = 'ABABCfABAAABABCABgAA'
new = 'ABABCABAA'
# print(arr)
# func(arr)
KMP(old, new)

```

