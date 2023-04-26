---
layout: post
title: React Query와 불변성
tags: []
---

Structural sharing 관한 이야기

React-query는 hook의 형태를 가지고 있지만 반환하는 데이터 상태 값이 무조건 불변성을 지키지는 않는다. 더 정확히 말하면, Query가 Invalidate 되어 다시 데이터를 불러오더라도, 불러온 데이터가 이전과 같다면(Deeply) data 상태 값은 이전과 같은 레퍼런스를 지닌다.

이는 React-query의 re-render 최적화로 설명할 수 있다.

## React Query의 불필요한 렌더링 방지

React-query는 크게 두 가지 방법으로 불필요한 렌더링을 방지한다.<br><br>
첫 번째는 notifyOnChangeProps 옵션을 통해 참조하는 필드만 추적해서 렌더링 하는 것이고 (기본으로 설정되어 있음)<br>
<br>
두 번째는 이 글의 주제인 Structural sharing이다.

## Structural Sharing?

Structural Sharing은 데이터 구조의 일부분이 변경되어도 변경되지 않은 부분은 이전과 동일한 참조를 유지하도록 하는 최적화 기법이다. 이를 통해 불필요한 데이터 복사를 방지하고, 메모리 사용량을 줄일 수 있다.

{% include image.html path="postimages/structura.jpeg" path-detail="postimages/structura.jpeg" alt="" %}

그림으로 표현하면 위와 같은데, 학교라는 데이터에서 학생 2의 데이터가 변경되면, 변경된 데이터만 새로운 레퍼런스를 가지게 되고 나머지 데이터는 이전의 데이터와 같은 레퍼런스를 참조한다.

아래의 코드는 실제 React-query에서 사용하고 있는 Structural Sharing 로직이다. (이 사람은 왜 세미콜론을 안 쓸까...)
{% highlight typescript %}

    /**
        * This function returns `a` if `b` is deeply equal.
        * If not, it will replace any deeply equal children of `b` with those of `a`.
        * This can be used for structural sharing between JSON values for   example.
    */

    export function replaceEqualDeep<T>(a: unknown, b: T): T
    export function replaceEqualDeep(a: any, b: any): any {
        if (a === b) {
            return a
        }

        const array = Array.isArray(a) && Array.isArray(b)

        if (array || (isPlainObject(a) && isPlainObject(b))) {
            const aSize = array ? a.length : Object.keys(a).length
            const bItems = array ? b : Object.keys(b)
            const bSize = bItems.length
            const copy: any = array ? [] : {}

            let equalItems = 0

            for (let i = 0; i < bSize; i++) {
                const key = array ? i : bItems[i]
                copy[key] = replaceEqualDeep(a[key], b[key])
                if (copy[key] === a[key]) {
                    equalItems++
                }

            return aSize === bSize && equalItems === aSize ? a : copy

        }

        return b
    }

{% endhighlight %}

재귀적으로 모든 값들에 대해 깊은 비교를 진행하는 것이 내가 찾아봤던 Structural Sharing과는 사뭇 다른 느낌이지만, 어쨌든 값이 변하지 않으면 본래의 레퍼런스 그대로 반환하는 것을 볼 수 있다.

## Epilogue

이 사실을 모르고... React-query 데이터를 useMemo 디펜던시에 걸어 버그를 냈던 내 경우엔,
structuralSharing 옵션을 사용해 꺼버렸다!

<br>
<br>

## Resource

[https://tkdodo.eu/blog/react-query-render-optimizations](https://tkdodo.eu/blog/react-query-render-optimizations)
[https://blog.klipse.tech/javascript/2021/02/26/structural-sharing-in-javascript.html](https://blog.klipse.tech/javascript/2021/02/26/structural-sharing-in-javascript.html)
[https://github.com/TanStack/query/blob/80cecef22c/src/core/utils.ts](https://github.com/TanStack/query/blob/80cecef22c/src/core/utils.ts)
