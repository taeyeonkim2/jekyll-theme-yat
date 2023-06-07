---
layout: post
title: Html 기초!
subtitle: A awesome static site generator.
author: Jeffrey
categories: jekyll
banner:
  video: "assets/images/banners/typing.gif"
  loop: true
  volume: 0.8
  start_at: 8.5
  image: "assets/images/banners/typing.gif"
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold; text-decoration: underline"
  subheading_style: "color: gold"
tags: jekyll theme yat
sidebar: []
---

HTML 기초 내용, 작성 규칙

HTML 기초 내용 <br/>
- HTML은 웹개발의 기본 영역이기에 간단하게 배울 수 있다.
- 쉽고 직관적인 편이라 초보자의 첫 스타트로 배우기 괜찮다.
<hr>

__1. 기본 준비__
 - Html을 편하게 작성하기 위해 Visual studio code 설치 

__2. Html 문서의 구조__
 - head : html의 제목영역, css등을 작성할 수 있는 공간
 - body : 본문 영역; 화면 부분을 작성하는 공간
 - 태그 : html에서 사용하는 명령어로, <태그>로 나타냄

__3. Html 작성 규칙__
- 태그 이름은 되도록 소문자 사용
- 연속된 공백은 하나의 공백과 같음.(공백100개 작성해도 1개라고 처리)
- 주석 : `<!-- 주석내용 -->`
- 태그는 쌍으로 사용(태그 시작과 태그 종료를 명확하게 사용)
  ex) `<p>`: 문단 열기 `</p>` : 문단 닫기
## section 1

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

## section 2

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]: https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

$ a \* b = c ^ b $

$ 2^{\frac{n-1}{3}} $

$ \int_a^b f(x)\,dx. $

```cpp
#include <iostream>
using namespace std;

int main() {
  cout << "Hello World!";
  return 0;
}
// prints 'Hi, Tom' to STDOUT.
```

```python
class Person:
  def __init__(self, name, age):
    self.name = name
    self.age = age

p1 = Person("John", 36)

print(p1.name)
print(p1.age)
```
