---
layout: page
title: Post
permalink: /post/
---

---
<input type="text" id="title">
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

        const title = document.querySelector("#title").value;
        const date = new Date();
        
        // const categories = document.querySelector("#categories").value;
        // const tags = document.querySelector("#tags").value;
        const dateString = date.toISOString().substr(0, 10);
        const dateTimeString = date.toISOString().substr(0, 19).replace("T", " ");
        const timezoneOffset = date.getTimezoneOffset() / 60 * 100 * (-1);
        const timezoneSign = Math.sign(timezoneOffset) >= 0 ? "+" : "-";
        
        const timezone = timezoneSign + ("0" + timezoneOffset.toString().replace("+","").replace("-","").replace(".", "")).slice(-4);
        const fileName = `${dateString}-${title}.markdown`;

        let description = "";
        description += "---" + "\n";
        description += "layout: post" + "\n";
        description += "title: " + title + "\n";
        description += "date: " + dateTimeString + " " + timezone + "\n";
        // description += "categories: " + categories + "\n";
        // description += "tags: " + tags + "\n";
        description += "---" + "\n"

        // ---
        // layout: post
        // title:  "test"
        // date:   2021-09-29 22:24:36 +0900
        // categories: jekyll update
        // tags: ex1 ex2
        // ---

        console.log(description + markdown);
        const blob = new Blob([description + markdown], { type : "text/plain;charset=utf-8" });
        
        await toBase64(blob);
        const dataURI = localStorage.getItem("file");
        const content = dataURI.split(',')[1];

        console.log(content);

        const parameters = {
            message: fileName + " Upload",
            content: content,
        }
        await fetch("https://api.github.com/repos/tkddbs2468/blog/contents/_post/" + fileName, {
            method: "PUT",
            headers: {
                "Accept" : "application/vnd.github.v3+json",
                "Authorization" : "token {{ site:token }}"
            },
            body: JSON.stringify(parameters)
        })
        .then(response => response.json())
        .then(data => {
            console.log(data);
        })
        .catch(error => console.log(error));

        // fetch("https://api.github.com/repos/tkddbs2468/blog/contents/_posts", {
        //     method: "GET",
        //     headers: {
        //         "Accept" : "application/vnd.github.v3+json",
        //         "Authorization" : "token {{ site:token }} "
        //     },
        // })
        // .then(response => response.json())
        // .then(data => {
        //     console.log(data);
        // })
        // .catch(error => console.log(error));
    }

</script>