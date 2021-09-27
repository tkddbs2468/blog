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
        const { access_token } = await fetch("https://github.com/login/oauth",
                {
                    code,
                    {{ client_id }},
                    {{ client_secret }}
                },
                {
                    method: "POST",
                    headers: {
                        "Accept" : "application/json",
                    }
                })
                .then(response => response.json())
                .then(data => {
                    console.log(data);
                })
                .catch(error => console.log(error));
    }

    
</script>