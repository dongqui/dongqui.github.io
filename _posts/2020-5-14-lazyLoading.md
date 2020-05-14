---
layout: post
title: "The Complete Guide to Lazy Loading Images"
tags: [web]
---
이 글은 [The Complete Guide to Lazy Loading Images](https://www.smashingmagazine.com/2016/08/ways-to-reduce-content-shifting-on-page-load/)을 번역/요약 한 글입니다.

### Images Lazy Loading
- 이미지를 불러올 때 한 번에 다 부르지 않고, 필요 할 때 적절하게 가져옴.
    - 웹 페이지 로드가 훨씬 빠르다
    - 다운로드 수도 적으니 당연히 비용 절약
    
### Lazy Loading Techniques for Images

    {% highlight html %}
        <img src="/path/to/some/image.jpg" />
    {% endhighlight %}

위와 같이 src attribute를 이용하면 이미지가 몇 개든간에 브라우저는 다 로드해버린다.
    {% highlight html %}
        <img data-src="https://ik.imagekit.io/demo/default-image.jpg" />
    {% endhighlight %}
따라서, src 대신에 data-src를 이용해서 일단 브라우저가 이미지를 로드하는 것을 막는다.
 
 그 다음, image가 viewport내에 있는지 확인 후, src를 이용해 이미지를 로드하도록 한다. <br>
 -  resize (창 크기), scroll(스크롤), orientationChange(가로/세로 변환) 이벤트를 감지해서 image의 위치가 viewport내에 있는지 계산 후 로드 하는 방법
 -  IntersectionObserver API를 통해서 이미지가 뷰포트에 들어가는 것을 감지하는 방법

전자의 경우 각각의 이벤트를 바인딩하거나, throtlling, 위치 계산 등의 이유로 퍼포먼스/복잡도 측면에서 후자에 비해 확실히 구리지만 후자의 경우 API를 지원하지 않는 브라우저가 있음.
    
{% highlight javascript %}
      document.addEventListener("DOMContentLoaded", function() {
        var lazyloadImages;    
        if ("IntersectionObserver" in window) {
          lazyloadImages = document.querySelectorAll(".lazy");
          var imageObserver = new IntersectionObserver(function(entries, observer) {
            entries.forEach(function(entry) {
              if (entry.isIntersecting) {
                var image = entry.target;
                image.src = image.dataset.src;
                image.classList.remove("lazy");
                imageObserver.unobserve(image);
              }
            });
          });
          lazyloadImages.forEach(function(image) {
            imageObserver.observe(image);
          });
        } else {  
          var lazyloadThrottleTimeout;
          lazyloadImages = document.querySelectorAll(".lazy");
          function lazyload () {
            if(lazyloadThrottleTimeout) {
              clearTimeout(lazyloadThrottleTimeout);
            }    
            lazyloadThrottleTimeout = setTimeout(function() {
              var scrollTop = window.pageYOffset;
              lazyloadImages.forEach(function(img) {
                  if(img.offsetTop < (window.innerHeight + scrollTop)) {
                    img.src = img.dataset.src;
                    img.classList.remove('lazy');
                  }
              });
              if(lazyloadImages.length == 0) { 
                document.removeEventListener("scroll", lazyload);
                window.removeEventListener("resize", lazyload);
                window.removeEventListener("orientationChange", lazyload);
              }
            }, 20);
          }
          document.addEventListener("scroll", lazyload);
          window.addEventListener("resize", lazyload);
          window.addEventListener("orientationChange", lazyload);
        }
      })
  {% endhighlight %}



## Lazy Loading CSS Background Images
background-image를 lazy loading하는 것도 크게 다르지 않다.
{% highlight css %}
    #bg-image.lazy {
       background-image: none;
       background-color: #F1F1FA;
    }
    #bg-image {
      background-image: url("https://ik.imagekit.io/demo/img/image10.jpeg?tr=w-600,h-400");
      max-width: 600px;
      height: 400px;
    }
{% endhighlight %}
  위와 같이 일단 CSS 규칙을 적어 놓고 로드가 필요할 때 자바스트립트로 lazy class를 때면 됨. <br>
  (id 혼자 쓰이는 거보다 class & id로 쓰이는게 우선순위가 높다.)
  
## Creating a Better User Experience

- Placeholder
    - 이미지 로드 전에 해당 이미지와 비슷한 계열의 배경색을 놓는다.
- Low Quality Image Placeholder (LQIP)
    - 이미지 로드 전에 해당 이미지를 블러 처리한 느낌의 저퀄리티 이미지를 놓는다.
- Add Buffer Time for Images to Load
    - viewport와 image 위치를 계산할 때 margin을 줘서 미리미리 로드하도록 하자.
- Avoid Content Reflow
    - 이미지를 감싸는 컨테이너 사이즈를 고정하지 않으면 이미지가 로드되는 순간, 다른 엘리먼트들을 밀어내서 Reflow가 일어남. <br>
    참고) [ goes into great dept](https://www.smashingmagazine.com/2016/08/ways-to-reduce-content-shifting-on-page-load/)
- Avoid Lazy Loading Every Image
    - 첫 번째 페이지 요소들은 Lazy Loading 할 필요 없다. 유저 경험을 더 구리게 할 수 있음.
    - 디바이스 사이즈에 따라 로드되어야 할 이미지 수도 다르다는 것을 인지하자.
    
## Testing Lazy Load
개발자 도구에서 Network > Images로 이미지가 lazy하게 잘 로드 되는지 확인 가능.

  <br>
  <br>

## Resource
[The Complete Guide to Lazy Loading Images](https://www.smashingmagazine.com/2016/08/ways-to-reduce-content-shifting-on-page-load/)
