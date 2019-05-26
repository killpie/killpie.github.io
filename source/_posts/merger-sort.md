---
title: 归并排序
date: 2018-08-30 11:34:35
categories: "算法"
tags: #算法
     - 算法
     - 排序
---

```java
public class MSort {
    public static void main(String[] args) {
        MSort m = new MSort();
        int[] r = {7,2,5,2,8,267,0};
         m.mSort(r,0,r.length-1);
        //MSort.sort(r,0,r.length-1);
        for (int i : r) {
            System.out.println(i);
        }
    }
    void merge(int[] r, int low, int mid, int high){
        int i = low;
        int j = mid+1;
        int k = low;

        int[] t = new int[high+1];
        while (i<=mid&&j<=high){
            t[k++]=r[i]<=r[j]?r[i++]:r[j++];
        }

        while (i<=mid)
            t[k++]=r[i++];
        while (j<=high)
            t[k++]=r[j++];
        for (int l = low; l <= high; l++) {
            r[l]=t[l];
        }
    }

    void mSort(int[] r,int low, int high){
        if (low==high)
            return;
        else {
            int mid = (low+high)/2;
            mSort(r,low,mid);
            mSort(r,mid+1,high);
            merge(r,low,mid,high);
        }
    }



    private static void sort(int[] array, int start, int end) {
        if (start >= end)
            return;

        int mid = (start + end) >> 1;
        // 递归实现归并排序
        sort(array, start, mid);
        sort(array, mid + 1, end);
        mergerSort(array, start, mid, end);
    }

    // 将两个有序序列归并为一个有序序列(二路归并)
    private static void mergerSort(int[] array, int start, int mid, int end) {
        // TODO Auto-generated method stub
        int[] arr = new int[end + 1]; // 定义一个临时数组，用来存储排序后的结果
        int low = start; // 临时数组的索引
        int left = start;

        int center = mid + 1;
        // 取出最小值放入临时数组中
        while (start <= mid && center <= end) {
            arr[low++] = array[start] > array[center] ? array[center++] : array[start++];
        }

        // 若还有段序列不为空，则将其加入临时数组末尾

        while (start <= mid) {
            arr[low++] = array[start++];
        }
        while (center <= end) {
            arr[low++] = array[center++];
        }

        // 将临时数组中的值copy到原数组中
        for (int i = left; i <= end; i++) {
            array[i] = arr[i];
        }
    }

}
```