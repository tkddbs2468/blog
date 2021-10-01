---
layout: page
title: Post
permalink: /post/
---

---
<h2>TITLE : <input type="text" id="title" placeholder="제목을 입력하세요."></h2>
<h2>CATEGORIES : <input type="text" id="title" placeholder="띄어쓰기로 구분하세요."></h2>
<h2>TAGS : <input type="text" id="title" placeholder="띄어쓰기로 구분하세요."></h2>
<br>
<div id="editor"></div>
<input type="submit" value="포스팅" onclick="onSubmit(event)">

<link rel="stylesheet" href="{{ site.baseurl | prepend: site.url }}/assets/toastui/toastui-editor.min.css">
<script src="{{ site.baseurl | prepend: site.url }}//assets/toastui/toastui-editor-all.min.js"></script>

<style>
input[type="submit"] {
  background-color: lightgray;
  border: none;
  color: black;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  box-shadow: 0 12px 16px 0 rgba(0,0,0,0.24),0 17px 50px 0 rgba(0,0,0,0.19);
}
</style>

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
            localStorage.setItem("file", reader.result);    
        }
        reader.onerror = error => reject(error);
    });

    const onSubmit = async (event) => {
        event.preventDefault();
        
        const token = atob("{{ site:token }}");

        const markdown = editor.getMarkdown();

        const title = document.querySelector("#title").value;
        if(title === "") {
            alert("제목을 입력하세요.");
            return false; 
        }

        let isPosting = confirm("포스팅하시겠습니까?");
        if(!isPosting) {
            return false;
        } else {

        }

        const date = new Date();
        
        const categories = document.querySelector("#categories").value;
        const tags = document.querySelector("#tags").value;
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
        description += "categories: " + categories + "\n";
        description += "tags: " + tags + "\n";
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
        localStorage.removeItem("file");
        const content = dataURI.split(',')[1];

        const parameters = {
            message: fileName + " Upload",
            content: content,
        }
        await fetch("https://api.github.com/repos/tkddbs2468/blog/contents/_posts/" + fileName, {
            method: "PUT",
            headers: {
                "Accept" : "application/vnd.github.v3+json",
                "Authorization" : "token " + token
            },
            body: JSON.stringify(parameters)
        })
        .then(response => response.json())
        .then(data => {
            console.log(data);
        })
        .catch(error => console.log(error));

        // location.href="{{ site.baseurl | prepend: site.url }}";
    }

</script>