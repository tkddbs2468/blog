---
permalink: /auth
---

<script>
    const url = new URL(location.href);

    const urlParams = url.searchParams;

    const code = urlParams.get("code");

    console.log(code);
    // const { access_token } = await fetch("https://github.com/login/oauth",
    //     {
    //         code
    //     },
    //     {
    //         method: "POST",
    //         headers: {
    //             "Authorization" : "{{ site.key }}",
    //             "Accept" : "application/vnd.github.v3+json",
    //             "Access-Control-Allow-Origin" : "*",
    //             "Access-Control-Allow-Headers" : "X-Requested-With",
    //         }
    //     })
    //     .then(response => response.json())
    //     .then(data => {
    //         console.log(data);
    //     })
    //     .catch(error => console.log(error));
</script>