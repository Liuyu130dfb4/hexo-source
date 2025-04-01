---
title: LeetCode 146 - LRU 缓存机制
date: 2025-03-31 11:00:00
tags: 
- 哈希表
- 双向链表
- 缓存
categories:
- algorithms
---

## 问题描述

设计并实现一个满足 **最近最少使用**（LRU）缓存机制的数据结构。它应该支持以下操作：

1. `get(key)`：如果缓存中存在 `key`，返回其对应的值，否则返回 -1。
2. `put(key, value)`：将 `key` 和 `value` 存入缓存。如果缓存容量已满，则在插入新元素之前，移除最近最少使用的元素。

---

## 初步想法

刚拿到这道题很懵逼，啥都没有，达到什么样的结果也不明确。其实这个问题通过测试类里面的函数来测试的，我只需要保证`get`和`put`方法正确就行了，其他的东西完全可以自由发挥。
---

## 算法思路

为了实现 O(1) 时间复杂度的 `get` 和 `put` 操作，我们可以结合 **哈希表** 和 **双向链表**：

1. **哈希表**：用于快速定位缓存中的键值对。
2. **双向链表**：用于维护缓存的访问顺序，最近使用的元素放在链表头部，最少使用的元素放在链表尾部。

### 操作细节

- **get(key)**：
  - 如果 `key` 存在于哈希表中，将对应的节点移动到链表头部，并返回值。
  - 如果 `key` 不存在，返回 -1。

- **put(key, value)**：
  - 如果 `key` 已存在，更新值并将节点移动到链表头部。
  - 如果 `key` 不存在：
    - 如果缓存已满，移除链表尾部节点（最少使用的元素）。
    - 插入新节点到链表头部，并更新哈希表。

---


## 代码实现

```c++
cstruct DLinkedNode {
    int key, value;
    DLinkedNode* prev;
    DLinkedNode* next;
    DLinkedNode(): key(0), value(0), prev(nullptr), next(nullptr) {}
    DLinkedNode(int _key, int _value): key(_key), value(_value), prev(nullptr), next(nullptr) {}
};

class LRUCache {
public:
    unordered_map<int,DLinkedNode*>map;
    DLinkedNode* head;
    DLinkedNode* tail;
    int size;
    int N;

    LRUCache(int capacity) 
    {
        size=0;
        N=capacity;

        head=tail=nullptr;
    }
    
    int get(int key) 
    {
        //如果key存在
        if(map.find(key)!=map.end())
        {
            auto node=map[key];
            //把key->TDLinkedNode* 放在开头,其他保持顺序
            //把key->TDLinkedNode* 放在开头,其他保持顺序

            //cout<<"把"<<node->key<<" "<<node->value<<"放在开头,其他保持顺序"<<endl;

            node_to_head(node);

            return node->value;
        }

        return -1;
    }
    
    void put(int key, int value) 
    {

        //如果key存在
        if(map.find(key)!=map.end())
        {
            auto node=map[key];

            //更改key->TDLinkedNode*里面的value值
            node->value=value;
            //把TDLinkedNode* 放在开头,其他保持顺序
            node_to_head(node);
        }
        else//如果不存在
        {
            //在开头插入{key,value}
            insert_to_head(key,value);
            map[key]=head;
            
            if(tail==nullptr)tail=head;

            //cout<<"insert key="<<key<<" value="<<value<<", size="<<size<<endl;

            if(size>N)
            {
                delete_tail( );
                //cout<<"delete"<<tail->key<<" size="<<size<<endl;
            }
        }


    }

    void node_to_head(DLinkedNode* node)
    {

        auto lst=node->prev, nex=node->next;

        //如果在开头
        if(lst==nullptr)return;

        lst->next=nex;
        
    
        if(nex!=nullptr)nex->prev=lst;


        node->next=head;
        head->prev=node;
        node->prev=nullptr;
        //cout<<"把"<<node->value<<"放在"<<head->value<<"前面"<<endl;



        head=node;

        //如果node再结尾
        if(nex==nullptr)tail=lst;
    }

    void delete_tail( )
    {

        //cout<<"删除"<<tail->key<<" "<<tail->value<<endl;

        //删除map
        map.erase(tail->key);


        //删除链表里面
        auto temp=tail;

        cout<<((tail->prev)==nullptr)<<endl;
        tail->prev->next=nullptr;
        tail=tail->prev;

        delete temp;

        size--;
    }


    
    void insert_to_head(int key,int value)
    {
        
        //cout<<"插入"<<key<<" "<<value<<endl;


        DLinkedNode* node=new DLinkedNode(key,value);

        map[key]=node;


        //若是第一个节点
        if(head==nullptr)
        {
            head=tail=node;
            size++;
            return;
        }
 
        node->next=head;
        head->prev=node;

        head=node;
        size++;
        
    }
};
```

---

## 复杂度分析

- **时间复杂度**：O(1)。
  - `get` 和 `put` 操作都只需哈希表查找和链表操作。
- **空间复杂度**：O(capacity)。
  - 哈希表和链表的空间与缓存容量成正比。

---

## 总结

通过结合哈希表和双向链表，我们可以高效地实现 LRU 缓存机制，满足题目要求。
