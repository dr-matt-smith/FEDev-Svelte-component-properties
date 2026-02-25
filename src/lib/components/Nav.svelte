<script>
//    let props = $props();
    import { page } from '$app/state';


</script>

<nav>
    <a href="/" class:active={page.url.pathname === "/"}>HOME</a>
    <a href="/about" class:active={page.url.pathname === "/about"}>ABOUT</a>
    <a href="/contact" class:active={page.url.pathname === "/contact"}>CONTACT</a>
</nav>

<!--<nav>-->
<!--    <a href="/" class:active={props.page == "home"}>HOME</a>-->
<!--    <a href="/about" class:active={props.page == "about"}>ABOUT</a>-->
<!--    <a href="/contact" class:active={props.page == "contact"}>CONTACT</a>-->
<!--</nav>-->


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

    a.active {
        background-color: grey;
        color: yellow;
    }
</style>