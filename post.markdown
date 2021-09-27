---
layout: page
title: Post
permalink: /post/
---

---

<div id="editor"></div>
<input type="submit" onclick="onSubmit(event)">

<link rel="stylesheet" href="{{ site.baseurl | prepend: site.url }}/assets/toastui/toastui-editor.min.css">
<script src="{{ site.baseurl | prepend: site.url }}//assets/toastui/toastui-editor-all.min.js"></script>

<script>
    
    const editorDiv = document.querySelector("#editor");

    const Editor = toastui.Editor;

    const editor = new Editor({
        el: editorDiv,
        height:"600px",
        initialEditType: "markdown",
        previewStyle: "vertical"
    });
    
    const onSubmit = (event) => {
        event.preventDefault();
        console.log(editor.getMarkdown());

        fetch("https://api.githuib.com/users/tkddbs2468", {
            headers: {
                "Accept" : "application/vnd.github.v3+json",
                "Access-Control-Allow-Origin" : "*",
                "Authentication" : "{{ site.key }}"
            }
        })
        .then(response => response.json())
        .then(data => {
            console.log(data);
        });
    }

</script>


<!--
This is the base Jekyll theme. You can find out more info about customizing your Jekyll theme, as well as basic Jekyll usage documentation at [jekyllrb.com](https://jekyllrb.com/)

You can find the source code for Minima at GitHub:
[jekyll][jekyll-organization] /
[minima](https://github.com/jekyll/minima)

You can find the source code for Jekyll at GitHub:
[jekyll][jekyll-organization] /
[jekyll](https://github.com/jekyll/jekyll)


[jekyll-organization]: https://github.com/jekyll
-->