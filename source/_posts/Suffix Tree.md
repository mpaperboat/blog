---
title: Suffix Tree
date: 2016-05-01 21:24:28
tags:
 - Algorithmics
 - String Algorithms
 - Suffix Tree
 - Algorithms
 - Suffix Automaton
categories:
 - Algorithmics.StringAlgorithms
---
# Description #
这是基于后缀自动机构建的后缀树。

具体的构建方法是将字符逆序添加到后缀自动机里面，顺便计算出后缀树需要的参数。

模板参数里的T是字符的类型；N是字符串可能的最大长度；M是字符集的大小；D是字符集的偏移（如小写字母集的偏移是'a'）。

成员变量中的nc是后缀树的节点总数；pr是父亲指针；ch是孩子指针；el是孩子边字符串的起始位置（闭的）；er是孩子边字符串的终止位置（开的）；tr是代表节点代表的字符串集合首部添加一个字符后的字符串集合的节点（来自后缀自动机）；dp是节点的深度；id是节点代表的的字符串集合的一个出现位置；sf表示一个节点是否是一个后缀。

所谓的一个节点表示的字符串集合是指从根到其父亲的字符串加上父亲边上的字符串的非空前缀。

注意节点个数可能有2N个，数组不要开小了。所有的成员变量都是静态的，不要定义在栈里面。
# Code #
``` cpp
#include<cstring>
template<class T,int N,int M,T D>struct SuffixTree{
    int node(){
        pr[++nc]=dp[nc]=sf[nc]=0;
        memset(tr[nc],0,4*M);
        return nc;
    }
    void build(const T*s,int n){
        nc=0,node();
        for(int i=n-1,c,p=1,q,np,nq;i>=0;--i,p=np){
            dp[np=node()]=dp[p]+1,id[np]=i+1,sf[np]=1;
            for(c=s[i]-D;p&&!tr[p][c];p=pr[p])
                tr[p][c]=np;
            if(p&&dp[q=tr[p][c]]!=dp[p]+1){
                dp[nq=node()]=dp[p]+1,pr[nq]=pr[q],id[nq]=i+1;
                memcpy(tr[pr[q]=pr[np]=nq],tr[q],4*M);
                for(;p&&tr[p][c]==q;p=pr[p])
                    tr[p][c]=nq;
            }else
                pr[np]=p?q:1;
        }
        for(int i=2,j,c;i<=nc;++i)
            c=s[id[i]+dp[j=pr[i]]-1]-D,
            el[j][c]=s+id[i]+dp[j]-1,
            er[j][c]=s+id[i]+dp[ch[j][c]=i]-1;
    }
    const T*el[2*N][M],*er[2*N][M];
    int nc,pr[2*N],tr[2*N][M],dp[2*N],id[2*N],sf[2*N],ch[2*N][M];
};
```
