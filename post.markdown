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
        console.log("{{ site.token }}");

        const blob = new Blob([editor.getMarkdown()], { type : "text/plain;charset=utf-8" });
        
        console.log(blob);
        const file = new File([blob], "test.markdown")

        console.log(file);

        const fileReader = new FileReader();

        fileReader.readAsDataURL(file);
        const test = btoa(editor.getMarkdown());
        console.log(test);
        const content = btoa(file);

        console.log(content);
        fetch("https://api.github.com/repos/tkddbs2468/blog/contents/_posts/test.markdown", {
            method: "PUT",
            headers: {
                "Accept" : "application/vnd.github.v3+json",
                "Authorization" : "token {{ site.token }}"
            },
            body: {
                message: "test",
                content: content
            }
        })
        .then(response => response.json())
        .then(data => {
            console.log(data);
        })
        .catch(error => console.log(error));
        // fetch("https://api.github.com/users/tkddbs2468", {
        //     method: "GET",
        //     headers: {
        //         "Accept" : "application/vnd.github.v3+json",
        //         //"Access-Control-Allow-Origin" : "*",
        //         //"Access-Control-Allow-Headers" : "X-Requested-With",
        //         "Authorization" : "token {{ site.token }}"
        //     }
        // })
        // .then(response => response.json())
        // .then(data => {
        //     console.log(data);
        // })
        // .catch(error => console.log(error));

        fetch("https://api.github.com/repos/tkddbs2468/blog/contents/_posts", {
            method: "GET",
            headers: {
                "Accept" : "application/vnd.github.v3+json",
                "Authorization" : "token {{ site.token }}"
            },
        })
        .then(response => response.json())
        .then(data => {
            console.log(data);
        })
        .catch(error => console.log(error));
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