---
layout: post
title: "Basic sorting algorithms"
tags: [알고리즘]
---

<strong> Bubble / Insertion / Merge/ Quick / Selection </strong>

그냥 한 번쯤은 구현 해보고 싶었다.
<br>
## Bubble sort
{% highlight javascript %}
function bubbleSort(a) {
	for (let i = 0; i < a.length - 1; i++) {
            for (let x = 0 ; x < a.length - 1 - i; x++) {
                if (a[x] > a[x + 1]) {
                    let temp = a[x];
                    a[x] = a[x + 1];
                    a[x + 1] = temp;
                }
            }
        }
}
{% endhighlight %}
- 인접한 두 수를 비교한다. 간단하게 구현 할 수 있으나, 비효율적임.
- O(n^2)

<br><br>

## Insertion
{% highlight javascript %}
function insertionSort(a) {
    for (let i = 1 ; i < a.length; i++) {
        let selection = a[i];
        let x = i;
        while(1) {
            x--;
            if (x < 0 || selection > a[x]) {
                x++;
                break;
            }
            a[x + 1] = a[x];
        }
        a[x] = selection;
    }
console.log(a);
}
{% endhighlight %}
- [https://www.youtube.com/watch?v=JU767SDMDvA](https://www.youtube.com/watch?v=JU767SDMDvA)
- 수가 적거나, 어느정도 정렬되어 있는 경우에 사용하는게 좋다.
- O(n^2)

<br><br>

## Merge
{% highlight javascript %}
function mergeSort(a) {
    if (a.length <= 1) {
        return a;
    }
    let half = Math.floor(a.length / 2);
    let arrayOne = mergeSort(a.slice(0, half));
    let arrayTwo = mergeSort(a.slice(half, a.length));

    return merge(arrayOne, arrayTwo);
}
function merge(a, b) {
    let mergedArr = [];
    while (a.length && b.length) {
        if (a[0] < b[0]) {
            mergedArr.push(a.shift());
        } else {
            mergedArr.push(b.shift());
        }
    }

    while (a.length || b.length) {
        if (a.length) {
            mergedArr.push(a.shift());
        }
        if (b.length) {
            mergedArr.push(b.shift());
        }
    }

    return mergedArr;
}
{% endhighlight %}
- [https://www.youtube.com/watch?v=4VqmGXwpLqc](https://www.youtube.com/watch?v=4VqmGXwpLqc)
- 대표적인 분할정복 알고리즘. 재귀적으로 잘게 쪼겐 후에 정렬하며 다시 병합한다.
- O(nlogn)

<br><br>

## Quick
{% highlight javascript %}
function quickSort(arr) {
    quickSortHelper(arr, 0, arr.length - 1);
    return arr;
}

function quickSortHelper(arr, left, right) {
    if (left >= right) {
        return ;
    }

    let partitionIdx = partition(arr, left, right);
    quickSortHelper(arr, left, partitionIdx - 1);
    quickSortHelper(arr, partitionIdx + 1, right);

}

function partition(arr, left, right) {
    let pivot = arr[right];
    let pivotIdx = left;

    for (; left < right; left++) {
        let current = arr[left];
        if (current < pivot) {
            swap(arr, left, pivotIdx);
            pivotIdx++;
        }
    }

    swap(arr, pivotIdx, right);

    return pivotIdx;
}

function swap(arr, a, b) {
    let temp = arr[a];
    arr[a] = arr[b];
    arr[b] = temp;
}
{% endhighlight %}
- [https://www.youtube.com/watch?v=7BDzle2n47c](https://www.youtube.com/watch?v=7BDzle2n47c)
- 추가적인 메모리가 필요없고 속도가 빠른편에 속한다. 정렬의 기준이 되는 pivot 값이 재수없게 가장 큰 수나 가장 작은 수로 계속 정해지거나, 이미 정렬된 상태라면 O(n^2)까지 걸릴 수 있다.
  세 수를 뽑아 가운데 수를 pivot으로 정하는 등의 방법으로 리스크를 줄일 수 있다.
- O(nlogn)

<br><br>

## Selection
{% highlight javascript %}
function selectionSort(arr) {
    for (let i = 0; i < arr.length; i++) {
        let miniIdx = i;
        for (let x = i; x < arr.length; x++) {
            if (arr[miniIdx] > arr[x]) {
                miniIdx = x;
            }
        }
        swap(arr, miniIdx, i);
    }
    return arr;
}

function swap(arr, a, b) {
    let temp = arr[a];
    arr[a] = arr[b];
    arr[b] = temp;
}
{% endhighlight %}
- [https://www.youtube.com/watch?v=g-PGLbMth_g](https://www.youtube.com/watch?v=g-PGLbMth_g)
- 간단하게 구현 가능하지만 비교적 느림.
- O(n^2)

