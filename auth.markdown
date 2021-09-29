---
permalink: /auth
---

<script>
    const url = new URL(location.href);

    const urlParams = url.searchParams;

    const code = urlParams.get("code");

    console.log(code);

    // location.href="https://github.com/login/oauth/access_token?client_id={{ site.client_id }}&client_secret={{ site.client_secret }}&code=" + code;

    const form = document.createElement("form");
    form.setAttribute("method", "POST");
    form.setAttribute("action", "https://github.com/login/oauth/access_token");
    
    const codeInput = document.createElement("input");
    codeInput.setAttribute("type", "hidden");
    codeInput.setAttribute("name", "code");
    codeInput.setAttribute("value", code);
    form.appendChild(codeInput);

    const clientIdInput = document.createElement("input");
    clientIdInput.setAttribute("type", "hidden");
    clientIdInput.setAttribute("name", "client_id");
    clientIdInput.setAttribute("value", "{{ site.client_id }}");
    form.appendChild(clientIdInput);

    const clientSecretInput = document.createElement("input");
    clientSecretInput.setAttribute("type", "hidden");
    clientSecretInput.setAttribute("name", "client_secret");
    clientSecretInput.setAttribute("value", "{{ site.client_secret }}");
    form.appendChild(clientSecretInput);

    document.body.appendChild(form);
    
    form.addEventListener("submit", function(event) {
        event.preventDefault();
        console.log("123");
    })

    form.submit();

    console.log(form);

    //const token = getToken(code);
    //console.log(token);

    


    // fetch("https://api.github.com/user", {
    //             method: "GET",
    //             headers: {
    //                 "Accept" : "application/vnd.github.v3+json",
    //                 //"Access-Control-Allow-Origin" : "*",
    //                 //"Access-Control-Allow-Headers" : "X-Requested-With",
    //                 "Authorization" : "token {{ site.token }}"
    //             }
    //         })
    //         .then(response => response.json())
    //         .then(data => {
    //             console.log(data);
    //         })
    //         .catch(error => console.log(error));

    async function getToken(code) {
        const access_token = await fetch("https://github.com/login/oauth/access_token",
                {
                    method: "POST",
                    headers: {
                        // "Accept-Language" : "*",
                        // "Content-Langeuage" : "en-US",
                        // "Content-Type" : "application/x-www-form-urlencoeded",
                        // "Access-Control-Allow-Origin" : "*",
                        // "Access-Control-Allow-Headers" : "GET, POST",
                        // "Access-Control-Allow-Methods" : "Origin, Content-Type, X-Auth-Token",
                        // "Origin" : "{{ site.url}}",
                        "Accept" : "application/json",
                    },
                    body : {
                        code: code,
                        client_id: "{{ site.client_id }}",
                        client_secret: "{{ site.client_secret }}"
                    },
                })
                .then(response => console.log(response))
                .catch(error => console.log(error));
    }

    
</script>