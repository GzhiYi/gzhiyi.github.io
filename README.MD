### 自定义
1. 文章如何显示阅读全文按钮
在markdown内嵌入
```bash
<!--more-->
```
这一行上面的内容为预览。
2. 点击阅读全文不自动定位到#more
进入`themes/next/layout/_macro/post.swig`，定位到post.excerpt这个判断语句内，将
```html
<div class="post-button text-center">
  <a class="btn" href="{{ url_for(post.path) }}{% if theme.scroll_to_more %}#{{ __('post.more') }}{% endif %}" rel="contents">
    {{ __('post.read_more') }} &raquo;
  </a>
</div>
```
改为：
```html
<div class="post-button text-center">
  <a class="btn" href="{{ url_for(post.path) }}" rel="contents">
    {{ __('post.read_more') }} &raquo;
  </a>
</div>
```