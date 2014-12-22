---
layout: post
title: "gvim use wmctrl in ubuntu"
date: 2013-06-20 19:35:00
comments: true
categories: [Vim]
tags: [Vim]
description: "gvim use wmctrl in ubuntu"
keywords: Vim
---

  Use the wmctrl tools could maximize the windows of gvim in ubuntu. But I find a small issue for that.

  Here is the wrong script:

```vimscript
  if has("win32")
    au GUIEnter * simalt ~x
else
    au GUIEnter * call MaximizeWindow()
endif
function! MaximizeWindow()
    silent !wmctrl -r :ACTIVE: -b add, maximized_vert,maximized_horz
endfunction
```

   You could see that between "," and "maximized_vert" on line 8, there is a blank. It's the reason why the script can't works. So I remove the blank and it woks fine. So strange, because this scripts works well on my mac mini.
