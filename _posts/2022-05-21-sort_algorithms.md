---
title: 回忆排序算法 
tags: 算法
key: sort_algorithms
---

# 导语

工作后很长一段时间没有整理算法了，还是得捡起来。

老样子，还是从排序算法开始吧，下面是常见的七种排序算法的实现。

## 冒泡排序

时间复杂度O(N^2)，空间复杂度O(1), 稳定

```c++
void bubble_sort(int data[], int size){
    for (int i=size-1; i>=0; i--){
        for(int j=0; j<i; j++){
            if (data[j] > data[j+1]){
                int tmp = data[j];
                data[j] = data[j+1];
                data[j+1] = tmp;
            }
        }
    }
}
```

## 选择排序

时间复杂度O(N^2)，空间复杂度O(1)，直接选择排序算法是不稳定的

```c++
void select_sort(int data[], int size){
    for(int i=0; i< size-1; i++){
        int min = data[i];
        int min_index = i;
        for(int j = i+1; j < size; j++){
            if (data[j] < min){
                min = data[j];
                min_index = j;
            }
        }
        if (min_index != i){
            data[min_index] = data[i];
            data[i] = min;
        }
    }
}
```

## 插入排序

时间复杂度O(N^2)，空间复杂度O(1), 稳定

```c++
void insert_sort(int data[], int size){
    for (int i=0; i< size-1; i++){
        int j = i+1;
        int x = data[j];
        for (; j>0 && data[j-1] > x; j--){
            data[j] = data[j-1];
        }
        data[j] = x;
    }    
}
```

## 希尔排序

插入排序的变种，通过比较一定间隔的元素进行插入排序，并且不断缩小间隔，直到比较相邻元素。使用Hibbard增量的希尔排序最坏时间复杂度为O(N^1.5),  空间复杂度O (1)，不稳定

```c++
void shell_sort(int data[], int size){
    for(int step = size / 2; step > 0; step /= 2){
        for(int i = step; i < size; i++){
            int j = i;
            int x = data[j];
            for(; j>=step && data[j-step] > x; j-=step)
                data[j] = data[j-step];
            data[j] = x;
        }
    }
}
```

## 快速排序

时间复杂度O(NlogN ~ N^2)，空间复杂度O(1)，不稳定， 这里我找到了两种快速排序中交换的方法，写法有一些差异

```c++
void quick_sort(int data[], int size){
    int l = 0, r = size -1;
    //_quick_sort(data, l, r);
    _quick_sortv1(data, l, r);
}

void _quick_sort(int data[], int l, int r){
    if (l < r){
        int x = data[r]; // base
        int small = l -1;
        for(int i = l; i < r; i++){
            if(data[i] < x){
                ++small;
                swap(data, small, i);
            }
        }
        ++small;
        swap(data, small, r);
        _quick_sort(data, l, small-1);
        _quick_sort(data, small+1, r);
    }
}

void _quick_sortv1(int data[], int l, int r){
    if (l < r){
        int x = data[l];
        int i = l, j = r;
        while(i < j){
            while((i< j) && (data[j]>=x))
                j--;
            data[i] = data[j];
            while((i<j) && (data[i]<=x))
                i++;
            data[j] = data[i];
            data[i] = x;
        }
        _quick_sortv1(data, l, i-1);
        _quick_sortv1(data, i+1, r);
    }
}
```

## 堆排序

时间复杂度O(NlogN)，空间复杂度O(1)，不稳定，堆排序涉及到一些堆的基本知识，包括建堆算法（O(N)复杂度），堆的调整算法等

```c++
void adjust_heap(int data[], int root, int size){
    int parent = root;
    int child = parent * 2  +1;
    int x = data[parent];
    while(child < size){
        if((child + 1 < size) && data[child+1] > data[child]){
            child ++;
        }
        if (data[child] > x){
            data[parent] = data[child];
            parent = child;
            child = parent * 2 + 1;
        }
        else{
            break;
        }
    }
    data[parent] = x;
}

void heap_sort(int data[], int size){
    for (int i = size / 2 - 1; i >= 0; i--){
        adjust_heap(data, i, size);
    } 
    for (int i = size -1; i >= 0 ; i --){
        int max = data[0];
        data[0] = data[i];
        data[i] = max;
        adjust_heap(data, 0, i);
    }
}
```

## 归并排序

时间复杂度O(NlogN), 空间复杂度O(N)，稳定

```c++
void merge(int data[], int l, int mid, int r){
   // left [l, mid], right[mid+1, r];
   vector<int> tmp;
   int p1 = l, p2 = mid+1;
   while(p1<=mid && p2<=r){
        if(data[p1] <= data[p2]){
           tmp.push_back(data[p1]);
           p1++;
        } else{
            tmp.push_back(data[p2]);
            p2++;
        }
   }
   while(p1<=mid){
        tmp.push_back(data[p1]);
        p1++;
   }
   while(p2<=r){
       tmp.push_back(data[p2]);
       p2++;
   }
   int k=l;
   for(auto x: tmp){
       data[k++] = x;
   }
}

void _merge_sort(int data[], int l, int r){
    if (l < r){
        int mid = (l + r) / 2;
        _merge_sort(data, l, mid);
        _merge_sort(data, mid+1, r);
        merge(data, l, mid, r);
    }
}

void merge_sort(int data[], int size){
    int l = 0, r = size -1;
    _merge_sort(data, l, r);
}
```
