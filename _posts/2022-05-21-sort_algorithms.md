---
title: 回忆排序算法 
tags: 算法
key: sort_algorithms
---

# 导语

工作后很长一段时间没有整理算法了，还是得捡起来。

老样子，还是从排序算法开始吧，下面是常见的七种排序算法的实现。

## 冒泡排序

时间复杂度O(N^2)，空间复杂度O(1), 稳定。

冒泡排序的思想是通过相邻两个数的比较，将大的数依次从数组左边移动至右边。

```cpp
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

时间复杂度O(N^2)，空间复杂度O(1)，直接选择排序算法是不稳定的。

直接选择排序是通过两次遍历选出最小的数依次排列，直接选择排序是不稳定的，比如数组[5,3,5,2,7], 第一次遍历时就会交换第一个5和2的顺序，导致不稳定。

```cpp
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

时间复杂度O(N^2)，空间复杂度O(1), 稳定。

如果一个已经排好序的数组此时插入一个数，可以通过与数组中每个数比较插入，这个就是插入排序的原理。

```cpp
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

插入排序的变种，通过比较一定间隔的元素进行插入排序，并且不断缩小间隔，直到比较相邻元素。使用Hibbard增量的希尔排序最坏时间复杂度为O(N^1.5),  空间复杂度O (1)，不稳定。

```cpp
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

时间复杂度O(NlogN ~ N^2)，空间复杂度O(1)，不稳定， 这里我找到了两种快速排序中交换的方法，写法有一些差异。

快速排序的基本思想是：采取分而治之的思想，把大的拆分为小的，每一趟排序，把比选定值小的数字放在它的左边，比它大的值放在右边；重复以上步骤，直到每个区间只有一个数，此时数组已经排序完成。

快速排序最重要的是partition函数功能的实现，也就是将比选定基数小的值放在他的左边，比选定基数大的值放在它的右边的功能函数。

下面代码中的`_quick_sort`是通过选择最右边的数位基数x，small为比x小的片段的尾部，如果data[i]比x小，则将data[i]和data[small+1]交换，因为如果small+1不和i相等的话，small+1是一个比x大的数，i遍历完后，将small和最后一个数交换。

另外一种partition函数函数的实现`_quick_sortv1`，则是通过双指针，从数组右侧找到一个比基数x小的数，放在左侧，然后再从数组左侧找到一个比基数x大的数，放在右侧，直至两个指针相遇。

```cpp
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

## 快速排序的非递归写法

所有的尾递归都可以改成循环，但是需要使用到数据结构`stack`，通过将边界l和r依次压入栈中，循环遍历栈来实现遍历。

```cpp
int partition(int data[], int l, int r){
    int x = data[l];
    int i = l, j = r;
    while(i < j ){
        while((i < j) && (data[j] >= x))
            j--;
        data[i] = data[j];
        while((i < j) && (data[i] <= x))
            i++;
        data[j] = data[i];
        data[i] = x;
    }
    return i;

}

void _quick_sortv2(int data[], int l, int r){
    if (l < r){
        stack<pair<int, int>> s;
        s.push({l, r});
        while(!s.empty()){
            pair<int, int> p = s.top();
            s.pop();
            int i = p.first;
            int j = p.second;
            int k = partition(data, i, j);
            if (k > i){
                s.push({i, k-1});
            }
            if (k < j){
                s.push({k+1, j});
            }
        }
    }
}
```

## 堆排序

时间复杂度O(NlogN)，空间复杂度O(1)，不稳定，堆排序涉及到一些堆的基本知识，包括建堆算法（O(N)复杂度），堆的调整算法等。

堆排序的思想是对于给定的n个记录，初始时把这些记录看作成一棵顺序的二叉树，然后将其调整为一个大顶堆，将堆的最后一个元素与堆顶的元素进行交换后，堆的最后一个元素即为最大元素；接着将前(n-1)个元素重新调整为一个大顶堆，再将堆顶元素与当前堆的最后一个元素进行交换后得到次大的记录，重复该过程知道调整的堆中只剩下一个元素为止。此时已经得到一个有序的数组。

另外堆排序中堆的构建复杂度为O(N)，排序中反复重建堆的复杂度为O(NlogN)。

```cpp
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

时间复杂度O(NlogN), 空间复杂度O(N)，稳定。

```cpp
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
