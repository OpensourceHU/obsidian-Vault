KMP是一种字符串匹配算法,其名称来自于这个算法的三个创始人的名字首字母
如果要匹配两个字符串, 为了方便叙述 设源字符串为ori 长为N 待匹配的字符串 设为patner长为M
那么暴力匹配的话 需要 $O(N*M)$
而使用KMP, 这个字符串匹配算法的复杂度将降到 $O(M+N)$ 是相当可观的优化
KMP 之所以能够在$O(M+N)$复杂度内完成查找，是因为其能在「非完全匹配」的过程中提取到有效信息进行复用，以减少「重复匹配」的消耗。

[如何更好地理解和掌握 KMP 算法? - 知乎 (zhihu.com)](https://www.zhihu.com/question/21923021/answer/281346746)

其算法核心在于 构造一个部分匹配表 Partial Match Table 
每当我们发现 原字符串 和模式串 失配时, 则可以让模式串不回原点, 而是回到一个位置, 这个位置之前的我们可以看作已经匹配成功的无需再匹配, 这样就可以加快匹配的速度
这个回到的idx 我们以next数组来记录(即PMT)
ori\[i\] != pat\[j\]时, 考察 ori\[i\] 是否等于 pat\[idx\]

```cpp
int KMP(char * t, char * p) 
{
	int i = 0; 
	int j = 0;

	while (i < (int)strlen(t) && j < (int)strlen(p))
	{
		if (j == -1 || t[i] == p[j]) 
		{
				i++;
           		j++;
		}
	 	else 
	 	{
           		j = next[j];
    	}

	if (j == strlen(p))
	   return i - j;
	else 
	   return -1;
}
```

对于求PMT,  首先要明确其含义: 在匹配串pat中, 位置i处, 前缀集合与后缀集合 中 最长字符串的长度
对于字符串 abababca，它的PMT如下表所示：

![img](日志/assets/v2-e905ece7e7d8be90afc62fe9595a9b0f_720w.png)

就像例子中所示的，如果待匹配的[模式字符串](https://www.zhihu.com/search?q=%E6%A8%A1%E5%BC%8F%E5%AD%97%E7%AC%A6%E4%B8%B2&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A281346746%7D)有8个字符，那么PMT就会有8个值。

我先解释一下字符串的前缀和后缀。如果字符串A和B，存在A=BS，其中S是任意的非空字符串，那就称B为A的前缀。例如，”Harry”的前缀包括{”H”, ”Ha”, ”Har”, ”Harr”}，我们把所有前缀组成的集合，称为字符串的前缀集合。同样可以定义后缀A=SB， 其中S是任意的非空字符串，那就称B为A的后缀，例如，”Potter”的后缀包括{”otter”, ”tter”, ”ter”, ”er”, ”r”}，然后把所有后缀组成的集合，称为字符串的后缀集合。要注意的是，字符串本身并不是自己的后缀。

有了这个定义，就可以说明PMT中的值的意义了。**PMT中的值是字符串的[前缀集合](https://www.zhihu.com/search?q=%E5%89%8D%E7%BC%80%E9%9B%86%E5%90%88&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A281346746%7D)与后缀集合的交集中最长元素的长度**。例如，对于”aba”，它的前缀集合为{”a”, ”ab”}，后缀 集合为{”ba”, ”a”}。两个集合的交集为{”a”}，那么长度最长的元素就是字符串”a”了，长 度为1，所以对于”aba”而言，它在PMT表中对应的值就是1。再比如，对于字符串”ababa”，它的前缀集合为{”a”, ”ab”, ”aba”, ”abab”}，它的后缀集合为{”baba”, ”aba”, ”ba”, ”a”}， 两个集合的交集为{”a”, ”aba”}，其中最长的元素为”aba”，长度为3。

好了，解释清楚这个表是什么之后，我们再来看如何使用这个表来加速字符串的查找，以及这样用的道理是什么。如图 1.12 所示，要在主字符串"ababababca"中查找模式字符串"abababca"。如果在 j 处字符不匹配，那么由于前边所说的模式字符串 PMT 的性质，主字符串中 i 指针之前的 PMT[j −1] 位就一定与模式字符串的第 0 位至第 PMT[j−1] 位是相同的。这是因为主字符串在 i 位失配，也就意味着主字符串从 i−j 到 i 这一段是与模式字符串的 0 到 j 这一段是完全相同的。而我们上面也解释了，模式字符串从 0 到 j−1 ，在这个例子中就是”ababab”，其前缀集合与[后缀集合](https://www.zhihu.com/search?q=%E5%90%8E%E7%BC%80%E9%9B%86%E5%90%88&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A281346746%7D)的交集的最长元素为”abab”， 长度为4。所以就可以断言，主字符串中i指针之前的 4 位一定与模式字符串的第0位至第 4 位是相同的，即长度为 4 的后缀与前缀相同。这样一来，我们就可以将这些字符段的比较省略掉。具体的做法是，保持i指针不动，然后将j指针指向模式字符串的PMT[j −1]位即可。

求next数组的程序
```cpp
void getNext(char * p, int * next)
{
	next[0] = -1;
	int i = 0, j = -1;

	while (i < (int)strlen(p))
	{
		if (j == -1 || p[i] == p[j])
		{
			++i;
			++j;
			next[i] = j;
		}	
		else
			j = next[j];
	}
}
```