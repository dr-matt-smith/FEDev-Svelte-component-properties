# Customising Svelte components with 'props'

Svelte components can be passed values ('props') to customise their display.

The steps below illustrate how to pass props to components to highlight the current page of a navigation bar for a 3-page website.

## Step 1 - setup basic Svelte project
create a new Svelte project

this should give you the default page at `/src/routes/+page.svelte`

![default home page](/screenshots/1_default_home_page.png)


## Step 2 - define a navigation bar component

![nav bar](/screenshots/2_navbar.png)


Create the following navbar component in  `/src/lib/components/Nav.svelte`:

```html
<nav>
    <a href="/">HOME</a>
    <a href="/about">ABOUT</a>
    <a href="/contact">CONTACT</a>
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
</style>
```

NOTE:
- the navbar CSS was inspired by this tutorial page: 
  - https://www.geeksforgeeks.org/css/how-to-create-a-navigation-bar-with-css-flexbox/

## Step 3 - import the navbar into the home page

![home page with nav bar](/screenshots/3_homepage_with_navbar.png)

Update the home page layout by editing `/src/routes/+page.svelte`.

Do the following:

- Add code to import the navbar component in the `<script>` section of the home page `/src/routes/+page.svelte`.

- Then add a `Nav` component at the top of HTML content of the page:

```html
<script>
  import Nav from '$lib/components/Nav.svelte';
</script>

<Nav />

<h1>Welcome to SvelteKit</h1>
<p>Visit <a href="https://svelte.dev/docs/kit">svelte.dev/docs/kit</a> to read the documentation</p>
```

## Step 4 - create the about and contact pages

![about and contact pages](/screenshots/4_about_and_contact_pages.png)

copy your home page twice:
- for about page into `/src/routes/about/+page.svelte`
- for contact page into `/src/routes/contact/+page.svelte`

Edit the details of your ABOUT page `/src/routes/about/+page.svelte` to be:

```html
<script>
  import Nav from '$lib/components/Nav.svelte';
</script>

<Nav />

<h1>About this website</h1>
<p>
  this site demonstrates simple navbar component
</p>
```

Edit the details of your CONTACT page `/src/routes/contact/+page.svelte` to be:

```html
<script>
  import Nav from '$lib/components/Nav.svelte';
</script>

<Nav />

<h1>Contact Us</h1>
<p>
  find us at TU Dublin, Blanchardstown Campus
</p>
```

## Step 5 - update nav component to allow current page highlight

For the nav component, add a new CSS style to make an element with class `active_link` styled in yellow text on a grey background.

Update the CSS style section of `/src/lib/components/Nav.svelte` to the following:

```html
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

  .active_link {
    background-color: grey;
    color: yellow;
  }
</style>
```

Now, we need some logic to decide how to set the home, about or contact link to have class `active_link` according to which page is being displayed.

The simplest approach I've found is to declare JavaScript variable for each nav link, which defaults to an empty string, but can be set to `active_link`.

So we can declare these 3 variables in the `<script>` section of our components Svelte file:

```html
<script>
    let homeClass = "";
    let aboutClass = "";
    let contactClass = "";
</script>
```

In our HTML we can add a `class` attribute to each link, whose value is one of the above variables, for each of our 3 navigation links:

```html
<nav>
  <a href="/" class="{homeClass}">HOME</a>
  <a href="/about" class="{aboutClass}">ABOUT</a>
  <a href="/contact" class="{contactClass}">CONTACT</a>
</nav>
```

Now we need to be able to receive the name of the page this component is being displayed in, as some variable that we can test. If the page is the home page, we'll set `homeClass` to contain `active_link` and so on.

To access any values ('props' - short for properties) passed to this component we write `let props = $props()`. We can now access variables inside `props`. So if we assume we are passed a `page` property, we can test for whether this property equals `home`, `about` or `contact`.

A SWITCH statement is the most appropriate selected statement to use, for different actions against difference values of a variable. So the `<script>` section of our components Svelte file now looks as follows: 

```html
<script>
    let props = $props();

    let homeClass = "";
    let aboutClass = "";
    let contactClass = "";

    switch(props.page) {
        case "home":
            homeClass = "active_link";
            break;
        case "about":
            aboutClass = "active_link";
            break;
        case "contact":
            contactClass = "active_link";
            break;
    }
</script>
```

The full listing for `/src/lib/components/Nav.svelte` is now:

```html
<script>
  let props = $props();

  let homeClass = "";
  let aboutClass = "";
  let contactClass = "";

  switch(props.page) {
    case "home":
      homeClass = "active_link";
      break;
    case "about":
      aboutClass = "active_link";
      break;
    case "contact":
      contactClass = "active_link";
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

  .active_link {
    text-decoration: underline;
    color: yellow;
  }
</style>
```

## Step 6 - pages pass in their name when using `Nav` component

![current page nav style](/screenshots/5_current_page_nav_style.png)

All the work has been done in our component, all we have to do now is ensure that each page passes in its name when using the component.

So our home page `/src/routes/+page.svelte` should pass in `home` by writing `<Nav page="home"/>`:

```html

<script>
  import Nav from '$lib/components/Nav.svelte';
</script>

<Nav page="home"/>

<h1>Welcome to SvelteKit</h1>
<p>Visit <a href="https://svelte.dev/docs/kit">svelte.dev/docs/kit</a> to read the documentation</p>
```

Our about page `/src/routes/about/+page.svelte` needs to pass `about`:

```html
<script>
  import Nav from '$lib/components/Nav.svelte';
</script>

<Nav page="about"/>

<h1>About this website</h1>
<p>
  this site demonstrates simple navbar component
</p>
```

And the contact page `/src/routes/contact/+page.svelte` needs to pass `contact`:

```html
<script>
  import Nav from '$lib/components/Nav.svelte';
</script>

<Nav page="contact"/>

<h1>Contact Us</h1>
<p>
  find us at TU Dublin, Blanchardstown Campus
</p>
```

## Step 7 - move the logic into Svelte IF-ELSE blocks

We could move the logic from JavaScript to Svelte IF-statements. The general form of a Svelte IF-statement is:

```html
  {#if <condition> }
    HTML output if TRUE
  {:else}
    HTML output if FALSE
  {/if}
```

So all we need in our `<script>` section is to get `props`, and the logic for whether or not to set each nav links class to `active` can be achievd by wrapping a Svelte IF-ELSE-statement around each element.

So our updated nav component `/src/lib/components/Nav.svelte` will now be as follows:

```html
<script>
  let props = $props();
</script>

<nav>
  {#if props.page == "home" }
      <a href="/" class="active_link">HOME</a>
  {:else}
      <a href="/">HOME</a>
  {/if}

  {#if props.about }
      <a href="/about" class="active_link">ABOUT</a>
  {:else}
    <a href="/about">ABOUT</a>
  {/if}

  {#if props.contact }
    <a href="/contact" class="active_link">CONTACT</a>
  {:else}
    <a href="/contact">CONTACT</a>
  {/if}
</nav>
```

While the above is very little JavaScript code, and we've done away with the style variables for each individual link, our HTML is heavily mixed in with all those Svelte IF-ELSE-statements. There is a better solution when we want to switch on/off a CSS property, the use of the CSS `:active` psueudoclass. Which we'll explore next ...

## Step 8 - use Boolean test to set CSS `:active` element property

Since we have Boolean (true/false) tests, such as `props.page == "home"`, we can use this Boolean true/false result to set the `:active` property of our link, which will us to style it with a CSS rule for `a.active`. 

So we replace the CSS style `active_link` with the CSS pseudoclass for `<a>` elements `.active` as follows:

```html
<script>
  let props = $props();
</script>

<nav>
  <a href="/" class:active={props.page == "home"}>HOME</a>
  <a href="/about" class:active={props.page == "about"}>ABOUT</a>
  <a href="/contact" class:active={props.page == "contact"}>CONTACT</a>
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

  a.active {
    text-decoration: underline;
    color: yellow;
  }
</style>
```

## Step 9: Svelte-recommended way to highlight current page - no props needed!

While the above has been a useful introduction to customising compoennts by opassing them 'props', there is actually an easier way to highlight the current page, since we can get the important part of the HTTP Request URL string using `page.url.pathname` from the `$app/state` Svelte library.
- for example, 
  - if we are visiting `http://localhost:XXX/about` 
  - then `page.url.pathname` gives us `/about`


So, in face, we don't actually need to pass any props to our navigation component, since the component can compare the URL string with the links (`/`, `/about`, `/contact`) to render the appropriate nav link active:

```html
<script>
  import { page } from '$app/state';
</script>

<nav>
  <a href="/" class:active={page.url.pathname === "/"}>HOME</a>
  <a href="/about" class:active={page.url.pathname === "/about"}>ABOUT</a>
  <a href="/contact" class:active={page.url.pathname === "/contact"}>CONTACT</a>
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

  a.active {
    background-color: grey;
    color: yellow;
  }
</style>
```