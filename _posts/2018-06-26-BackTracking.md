---
layout: post
title: "N - Queens"
tags: [알고리즘]

---

Row을 이동하면서 모든 경우의 수를 고려한다.<br>
만약 조건에 부합하지 않으면 다음 Row로 넘어가지 않고 올려 놓은 queen을 회수 한 후 다음 검사를 진행한다.

{% highlight python %}
window.countNQueensSolutions = function(n) {
    var solutionCount = 0;
    let board = new Board({n : n});

    if (n === 0) {
    return 1;
    }
    recursion(0);
    function recursion(row) {
        for (let col = 0; col < n; col++) {
        board.togglePiece(row, col);
        if (board.hasAnyQueenConflictsOn(row, col)) {
            board.togglePiece(row, col);
        } else if (row === n - 1) {
            solutionCount += 1;
            board.togglePiece(row, col);
        } else {
            recursion(row + 1);
            board.togglePiece(row, col);
        }
      }
    };
return solutionCount;
};
{% endhighlight %}


