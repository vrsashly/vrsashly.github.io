---
title: "This is a test title"
summary: "Just for fun!"
date: 2024-11-11
tags:
    - C
    - Db
---

This post shows how to implement a simple hash table of arbitrary length,
allowing to store all values c knows and doing so while being as minimal as
possible. It does however not include collision handling, to implement this,
simply swap the `Map.buckets` array with an array of linked list and insert
into the linked lists instead of only into the bucket array and you should be
good to go.

```c
#include <assert.h>

typedef struct Map { size_t size; size_t cap; void **buckets; } Map;
const size_t BASE = 0x811c9dc5;
const size_t PRIME = 0x01000193;
size_t hash(Map *m, char *str) {
    size_t initial = BASE;
    while(*str) {
        initial ^= *str++;
        initial *= PRIME;
    }
    return initial & (m->cap - 1);
}

Map init(size_t cap) {
    Map m = {0,cap};
    m.buckets = malloc(sizeof(void*)*m.cap);
    assert(m.buckets != NULL);
    return m;
}

void put(Map *m, char *str, void *value) {
    m->size++;
    m->buckets[hash(m, str)] = value;
}

void* get(Map *m, char *str) {
    return m->buckets[hash(m, str)];
}
```
