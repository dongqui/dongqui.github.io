---
layout: post
title: "Priority Queue"
tags: [자료구조]
---
### 화장실 줄 설 때 제일 급한 사람이 제일 먼저 들어가는 아름다운 세상.

<br/>
보통 큐와 다르게 우선순위 큐는 들어간 순서와 관계 없이 우선 순위에 따라 데이터가 반환 된다.<br>
배열로 우선순위 큐를 구현할 경우에는 값을 중간에 넣을 때 마다 다른 데이터들을 밀거나 당겨야 하는 번거로움이 생긴다.<br>
연결 리스트 역시 우선 순위를 비교해야하는 대상의 수가 n까지 이어져 비효율이 생긴다. <br>
그래서 일반적으로 우선순위 큐는  힙(heap)을 이용해 구현한다. 힙은 완전 이진트리로, 부모 노드에는 늘 자식 노드보다 우선순위가 높은 데이터가 배치된다.

{% highlight javascript %}
function PriorityQueue() {
    this.queue = [0];
}

PriorityQueue.prototype.add = function(value) {
    this.queue.push(value);
    let currentIdx = this.queue.length - 1;
    let parentIdx = Math.floor(currentIdx / 2);
    this.movingUp(currentIdx, parentIdx);
};

PriorityQueue.prototype.pop = function() {
    this.swap(1, this.queue.length - 1);
    let returnValue = this.queue.pop();
    this.movingDown(1);
    return returnValue;
};

PriorityQueue.prototype.movingUp = function(childIdx, parentIdx) {
    while (this.isChildGreater(childIdx, parentIdx)) {
        this.swap(childIdx, parentIdx);
        childIdx = parentIdx;
        parentIdx = Math.floor(childIdx / 2);
    }
};

PriorityQueue.prototype.movingDown = function(parentIdx) {
    while(parentIdx <= this.queue.length - 1) {
        let childIdx = this.selectChildToCompare(parentIdx);
        if (!childIdx || this.queue[childIdx] < this.queue[parentIdx]) {
            break;
        }
        this.swap(childIdx, parentIdx);
        parentIdx= childIdx;
    }
};

PriorityQueue.prototype.selectChildToCompare = function(parentIdx) {
    let childOne = this.queue[parentIdx * 2];
    let childTwo = this.queue[parentIdx * 2 + 1];

    if (!childOne && !childTwo) {
        return false;
    } else if (!childTwo || childOne > childTwo) {
        return parentIdx * 2;
    } else {
        return parentIdx * 2 + 1;
    }
};

PriorityQueue.prototype.isChildGreater = function(childIdx, parentIdx) {
    if (childIdx === 1) {
        return false
    }
    let child = this.queue[childIdx];
    let parent = this.queue[parentIdx];

    return child > parent;
};

PriorityQueue.prototype.swap = function(a, b) {
    let temp = this.queue[a];
    this.queue[a] = this.queue[b];
    this.queue[b] = temp;
};
{% endhighlight %}
- [https://www.youtube.com/watch?v=2DmK_H7IdTo](https://www.youtube.com/watch?v=2DmK_H7IdTo)
- 추가: O(log n) / 삭제: O(log n)
