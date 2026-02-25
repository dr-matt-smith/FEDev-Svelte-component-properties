<script>
    let props = $props();

    let homeClass = "";
    let aboutClass = "";
    let contactClass = "";

    switch(props.page) {
        case "home":
            homeClass = "current_page";
            break;
        case "about":
            aboutClass = "current_page";
            break;
        case "contact":
            contactClass = "current_page";
            break;
    }
</script>

<nav>
    <a href="/" class="{homeClass}">HOME</a>
    <a href="/about" class="{aboutClass}">ABOUT</a>
    <a href="/contact" class="{contactClass}">CONTACT</a>
</nav>

<style>
    nav {
        display: flex;
        justify-content: center;
        background-color: #333;
    }
    nav a {
        color: white;
        padding: 14px 20px;
        text-decoration: none;
        text-align: center;
    }

    .current_page {
        text-decoration: underline;
        color: yellow;
    }
</style>