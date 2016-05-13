---
title: Suffix Tree
date: 2016-05-01 21:24:28
tags:
 - Algorithm
 - Library
 - Suffix Tree
categories:
 - Algorithm.Library
---
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
