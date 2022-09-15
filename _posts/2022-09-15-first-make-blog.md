---
layout: post
title: Git 블로그 만들기 - jekyll
date: 2022-09-15 19:00:00 +0800
last_modified_at: 2022-09-15 19:30:00 +0800
tags: []
toc:  true
---
Welcome to **Not Pure Poole**! This is an example post to show the layout.
{: .message }

# github를 활용해서 블로그 만들기

## 1. github repository 생성

github 아이디 생성이후 로그인이후 <a href="github.com/new">[repository 생성]</a>를 누른다.
<img src="/images/first-make-blog/1.png" width="50%" height="50%">
> 1. 빨간 글씨로 적혀있는 부분에 자신의 Owner이름.github.io 라고 작성한다.
> 2. Public 로 체크 하고 Add a README file 를 체크한다.

이후 생성하면된다. 잘 동작하는지 확인하기 위해 "자신의 이름.github.io"로 접근해서 화면이 뜬다면 성공한 것이다.

## 2. 자신에게 맞는 jekyll 테마 찾기

<a href="http://jekyll-themes.com/free/">http://jekyll-themes.com/free/</a>에서 찾아보는 것을 추천한다. 여러가지가 존재하고 demo를 통해 동작방식을 확인할 수 있다.

필자는 <a href="https://jekyll-themes.com/not-pure-poole/">https://jekyll-themes.com/not-pure-poole/</a>를 사용했다.
<img src="/images/first-make-blog/1.png" width="50%" height="50%">

> 1. 사진에 보이는 DOWNLOAD 를 눌려 다운로드 한다.

## 3. GitHub Desktop 활용하기 (Window 10)

Git Hub Desktop은 처음 접하는 Git을 쉽게 사용할 수 있도록 도와주는 앱이다.

그중 로컬 파일과 git repository를 연동하는 작업을 할것이다.

<a href="https://desktop.github.com/">https://desktop.github.com/</a>로 들어가서 Download for Windows (64 bit) 눌려 설치를 한다.

설치이후

<img src="/images/first-make-blog/3.png" width="50%" height="50%">

그림처럼 검색하여 앱을 실행한다.

<img src="/images/first-make-blog/4.png" width="50%" height="50%">

실행하면 다음과 같은 화면이 등장한다. 
> 1. 좌측 상단에 있는 current repository에 있는 ▼를 클릭한다. 
> 2. 이후 Add ▼ 를 눌려 Clone repository를 누른다.

<img src="/images/first-make-blog/5.png" width="50%" height="50%">

해당 화면이뜨게 되는데 연동할 로컬 파일을 선택하고 처음 생성했던 자신owner.github.io 를 선택한다.
필자의 경우 선택하게 되면 Local path란에 C:\00gitblog\sangwoong12.github.io 라고 나타난다. 마지막으로 clone을 누르면 된다.



## 4. 다운받은 jekyll 테마 적용하기

마지막으로 다운받은 jekyll 테마를 적용해보겠다.

<img src="/images/first-make-blog/6.png" width="50%" height="50%">

다운받은 파일을 Github Desktop에서 clone 해준 파일에 압축을 해제한다.
필자의 경우 위의 사진처럼 압축을 풀어 주었다.

git에 변동사항을 적용시키기 위해 다시 Github Desktop을 보게되면 
## Inline HTML elements

HTML defines a long list of available inline tags, a complete list of which can be found on the [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/HTML/Element).

- **To bold text**, use `<strong>`.
- *To italicize text*, use `<em>`.
- <mark>To highlight</mark>, use `<mark>`.
- Abbreviations, like <abbr title="HyperText Markup Langage">HTML</abbr> should use `<abbr>`, with an optional `title` attribute for the full phrase.
- Citations, like <cite>&mdash; Mark Otto</cite>, should use `<cite>`.
- <del>Deleted</del> text should use `<del>` and <ins>inserted</ins> text should use `<ins>`.
- Superscript <sup>text</sup> uses `<sup>` and subscript <sub>text</sub> uses `<sub>`.

Most of these elements are styled by browsers with few modifications on our part.

## Footnotes

Footnotes are supported as part of the Markdown syntax. Here's one in action. Clicking this number[^fn-sample_footnote] will lead you to a footnote. The syntax looks like:

{% highlight text %}
Clicking this number[^fn-sample_footnote]
{% endhighlight %}

Each footnote needs the `^fn-` prefix and a unique ID to be referenced for the footnoted content. The syntax for that list looks something like this:

{% highlight text %}
[^fn-sample_footnote]: Handy! Now click the return link to go back.
{% endhighlight %}

You can place the footnoted content wherever you like. Markdown parsers should properly place it at the bottom of the post.

## Heading

Vivamus sagittis lacus vel augue rutrum faucibus dolor auctor. Duis mollis, est non commodo luctus, nisi erat porttitor ligula, eget lacinia odio sem nec elit. Morbi leo risus, porta ac consectetur ac, vestibulum at eros.

### Code

Inline code is available with the `<code>` element. Snippets of multiple lines of code are supported through Rouge. Longer lines will automatically scroll horizontally when needed. You may also use code fencing (triple backticks) for rendering code.

{% highlight js %}
// Example can be run directly in your JavaScript console

// Create a function that takes two arguments and returns the sum of those arguments
var adder = new Function("a", "b", "return a + b");

// Call the function
adder(2, 6);
// > 8
{% endhighlight %}

You may also optionally show code snippets with line numbers. Add `linenos` to the Rouge tags.

{% highlight js linenos %}
// Example can be run directly in your JavaScript console

// Create a function that takes two arguments and returns the sum of those arguments
var adder = new Function("a", "b", "return a + b");

// Call the function
adder(2, 6);
// > 8
{% endhighlight %}

Aenean lacinia bibendum nulla sed consectetur. Etiam porta sem malesuada magna mollis euismod. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut fermentum massa.

### Lists

Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Aenean lacinia bibendum nulla sed consectetur. Etiam porta sem malesuada magna mollis euismod. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut fermentum massa justo sit amet risus.

- Praesent commodo cursus magna, vel scelerisque nisl consectetur et.
- Donec id elit non mi porta gravida at eget metus.
- Nulla vitae elit libero, a pharetra augue.

Donec ullamcorper nulla non metus auctor fringilla. Nulla vitae elit libero, a pharetra augue.

1. Vestibulum id ligula porta felis euismod semper.
2. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus.
3. Maecenas sed diam eget risus varius blandit sit amet non magna.

Cras mattis consectetur purus sit amet fermentum. Sed posuere consectetur est at lobortis.

<dl>
  <dt>HyperText Markup Language (HTML)</dt>
  <dd>The language used to describe and define the content of a Web page</dd>

  <dt>Cascading Style Sheets (CSS)</dt>
  <dd>Used to describe the appearance of Web content</dd>

  <dt>JavaScript (JS)</dt>
  <dd>The programming language used to build advanced Web sites and applications</dd>
</dl>

Integer posuere erat a ante venenatis dapibus posuere velit aliquet. Morbi leo risus, porta ac consectetur ac, vestibulum at eros. Nullam quis risus eget urna mollis ornare vel eu leo.

### Images

Quisque consequat sapien eget quam rhoncus, sit amet laoreet diam tempus. Aliquam aliquam metus erat, a pulvinar turpis suscipit at.

![placeholder](http://placehold.it/800x400 "Large example image")
![placeholder](http://placehold.it/400x200 "Medium example image")
![placeholder](http://placehold.it/200x200 "Small example image")

Align to the center by adding `class="align-center"`:

![placeholder](http://placehold.it/400x200 "Medium example image"){: .align-center}

### Tables

Aenean lacinia bibendum nulla sed consectetur. Lorem ipsum dolor sit amet, consectetur adipiscing elit.

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Upvotes</th>
      <th>Downvotes</th>
    </tr>
  </thead>
  <tfoot>
    <tr>
      <td>Totals</td>
      <td>21</td>
      <td>23</td>
    </tr>
  </tfoot>
  <tbody>
    <tr>
      <td>Alice</td>
      <td>10</td>
      <td>11</td>
    </tr>
    <tr>
      <td>Bob</td>
      <td>4</td>
      <td>3</td>
    </tr>
    <tr>
      <td>Charlie</td>
      <td>7</td>
      <td>9</td>
    </tr>
  </tbody>
</table>

Nullam id dolor id nibh ultricies vehicula ut id elit. Sed posuere consectetur est at lobortis. Nullam quis risus eget urna mollis ornare vel eu leo.

-----

Want to see something else added? <a href="https://github.com/vszhub/not-pure-poole/issues/new">Open an issue.</a>

[^fn-sample_footnote]: Handy! Now click the return link to go back.
