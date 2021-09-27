---
permalink: /auth
---

<script>
    const url = new URL(location.href);

    const urlParams = url.searchParams;

    const code = urlParams.get("code");

    console.log(code);
    const token = getToken(code);
    console.log(token);


    async function getToken(code) {
        const access_token = await fetch("https://github.com/login/oauth/access_token",
                {
                    method: "POST",
                    mode: "no-cors",
                    headers: {
                        "Accept" : "application/json",
                        "Content-Type" : "application/json",
                        "Access-Control-Allow-Origin" : "*",
                        "Origin" : "{{ site.url}}",
                        "Accept-Encoding" : "UTF-8"
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