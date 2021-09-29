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
        
    const toBase64 = file => new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.readAsDataURL(file);
        reader.onload = () => {
            resolve(reader.result);
            console.log(reader);
            localStorage.setItem("file", reader.result);    
        }
        reader.onerror = error => reject(error);
    });

    const onSubmit = async (event) => {
        event.preventDefault();

        const markdown = editor.getMarkdown();
        
        console.log("{{ site.token }}");

        const blob = new Blob([markdown], { type : "text/plain;charset=utf-8" });
        
        await toBase64(blob);
        const dataURI = localStorage.getItem("file");
        const content = dataURI.split(',')[1];

        console.log(content);

        const parameters = {
            message: "test",
            content: content,
        }
        await fetch("https://api.github.com/repos/tkddbs2468/blog/contents/_posts/" + "2021-09-13-test!!.markdown", {
            method: "PUT",
            headers: {
                "Accept" : "application/vnd.github.v3+json",
                "Authorization" : "token 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"
            },
            body: JSON.stringify(parameters)
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
                "Authorization" : "token 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 "
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