---
layout: post
title: "Vim auto-complete issue"
date: 2013-07-10 08:27
comments: true
categories:
---

重新安裝 MacVim 後，發現只要在行首或者空格後面按 Tab 都會自動做 auto－complete，這可不大妙，每次都要 ESC 後用空格來做 indentation。解決的方法是加入以下 script 到 .gvimrc.

{% codeblock %}
" Prevent auto-complete while the cursor at the beginning of line or not on a
" word
function! InsertTabWrapper()
    let col = col('.') - 1
    if !col || getline('.')[col - 1] !~ '\k'
        return "\<tab>"
    else
        return "\<c-p>"
    endif
endfunction

inoremap <tab> <c-r>=InsertTabWrapper()<cr>
{% endcodeblock %}
