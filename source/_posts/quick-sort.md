---
title: 快排
date: 2018-08-30 11:38:06
categories: "算法"
tags: #算法
     - 算法
     - 排序
---
```java
public class QuickSort {
    public static void main(String[] args) {
        int[] a = {2,7,3,6,9,1};
        QuickSort quickSort = new QuickSort();
        quickSort.quickSort(a);
        for (int i : a) {
            System.out.println(i);
        }
    }

    void quickSort(int[] array){
        qSort(array,0,array.length-1);
    }

    void qSort(int[] array, int low, int high){
        if (low<high){//长度大于1
            int pivotloc = partition(array,low,high);
            qSort(array,low,pivotloc-1);
            qSort(array,pivotloc+1,high);
        }
    }

    int partition(int[] array, int low, int high){
        int pivotkey = array[low];
        while (low<high){
            while (low<high&&array[high]>=pivotkey)
                --high;
            array[low] = array[high];
            while (low<high&&array[low]<=pivotkey)
                ++low;
            array[high]=array[low];
        }
        array[low]=pivotkey;
        return low;
    }
}

```