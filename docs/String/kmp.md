[讲课链接](https://www.bilibili.com/video/BV1C3411d7iH/?spm_id_from=333.999.0.0&vd_source=57a1e2bae1f3574849d8f90b75cb25a2)

## 前置知识
border：一个字符串的border是其真子串中，既是前缀又是后缀，而且长度最长的那个子串

部分匹配表PMT：字符串前缀的border长度

文本串text string：查找的文本范围

模式串pattern string：需要查找的字符串，通常长度远小于文本串

字符串最小循环节长度=len-PMT[len-1]
## 板子
```cpp
char txt[N],str[N];//0-Index
int pmt[N];//P[0]~P[i] 这一段字符串，使得真前缀等于真后缀的最长长度
void getNext()
{
    int len=strlen(str);
    pmt[0]=0;
	for(int i=1,j=0;i<len;++i)
	{
		while(j && str[i]!=str[j])
            j=pmt[j-1];
		if(str[i] == str[j])
            ++j;
		pmt[i] = j;
	}
}
void KMP()
{
    int len1=strlen(txt),len2=strlen(str);
    for(int i=0,j=0;i<len1;++i)
	{
		while(j && txt[i]!=str[j])
            j=pmt[j-1];
		if(txt[i] == str[j])
            ++j;
		if(j == len2)
		{
            //允许重复匹配
			j=pmt[j-1];
            //不允许重复匹配
            j=0;
			cout<<i-len2+1<<"\n";
		}
	}
}
```
