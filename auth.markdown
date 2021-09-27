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
        const access_token = await fetch("https://github.com/login/oauth",
                {
                    code: code,
                    client_id: "{{ site.client_id }}",
                    client_secret: "{{ site.client_secret }}"
                },
                {
                    method: "POST",
                    mode: "no-cors",
                    headers: {
                        "Accept" : "application/json",
                        "Access-Control-Allow-Origin" : "*",
                        "Origin" : "{{ site.url}}"
                    }
                })
                .then(response => response.json())
                .then(data => {
                    console.log(data);
                })
                .catch(error => console.log(error));
    }

    
</script>