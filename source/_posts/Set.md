---
title: Set
date: 2016-05-20 23:53:28
tags:
 - Algorithmics
 - Data Structures
 - Set
 - Algorithms
 - Treap
categories:
 - Algorithmics.DataStructures
---
# Description #
这是用Treap实现的Set。

用的是非旋转的Treap，并维护了size域。可以实现常见的大多数平衡树维护有序表的操作。

模板参数里面的T是数据类型，C是比较器类型。

插入和删除直接用insert和erase就好，都返回修改后的树的根节点；select函数选择第k小的元素；count函数计算出比一个值小的元素个数（或当d=1时计算出比一个值大的元素的个数），并同时返回其中最大（最小）的元素的节点；find函数找到一个值对应的节点；clear函数清空一棵树；size函数返回一棵树的大小（会特判空树的情况）。

注意频繁的插入和删除会降低效率，这时候可以考虑实现一个内存池。
# Code #
``` cpp
#include<bits/stdc++.h>
using namespace std;
template<class T,class C>struct Set{
    struct node{
        node(T u){
            c[0]=c[1]=0,v=u,s=1;
            f=rand()*1.0/RAND_MAX*2e9;
        }
        T v;
        node*c[2];
        int s,f;
    }*j,*k;
    int size(node*x){
        return x?x->s:0;
    }
    void update(node*x){
        x->s=1;
        for(int i=0;i<2;++i)
            x->s+=size(x->c[i]);
    }
    node*merge(node*x,node*y){
        if(!x||!y)
            return x?x:y;
        if(x->f<y->f)
            x->c[1]=merge(x->c[1],y),y=x;
        else
            y->c[0]=merge(x,y->c[0]);
        update(y);
        return y;
    }
    void split(node*x,int t){
        if(x){
            int s=size(x->c[0]);
            if(s>=t)
                split(x->c[0],t),
                x->c[0]=k,k=x;
            else
                split(x->c[1],t-s-1),
                x->c[1]=j,j=x;
            update(x);
        }else
            j=k=0;
    }
    void clear(node*x){
        if(x){
            clear(x->c[0]);
            clear(x->c[1]);
            delete x;
        }
    }
    node*find(node*z,T a){
        node*r=0;
        while(z)
            if(C()(a,z->v))
                z=z->c[0];
            else if(C()(z->v,a))
                z=z->c[1];
            else
                break;
        return r;
    }
    node*select(node*z,int a){
        for(;;)
            if(size(z->c[0])>=a)
                z=z->c[0];
            else if(size(z->c[0])+1<a)
                a-=size(z->c[0])+1,z=z->c[1];
            else
                return z;
    }
    pair<node*,int>count(node*z,T a,int d){
        int c=0;
        node*r=0;
        while(z)
            if(C()(d?a:z->v,d?z->v:a))
                r=z,c+=size(z->c[d])+1,
                z=z->c[!d];
            else
                z=z->c[d];
        return make_pair(r,c);
    }
    node*erase(node*x,T v){
        split(x,count(x,v,0).second);
        node*y=j;split(k,1);delete j;
        return merge(y,k);
    }
    node*insert(node*x,T v){
        split(x,count(x,v,0).second);
        return merge(merge(j,new node(v)),k);
    }
};
```
