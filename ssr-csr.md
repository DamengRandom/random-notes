# SSR and CSR Basic Concept

Server side rendering (SSR) and Client side rendering (CSR) are two different ways to render a web page.


## SSR

- SSR is a process where the server sends the HTML to the client. The client then receives the HTML and displays it.

- SSR is mainly for rendering the `static contents`, we want the fast initial loading and the SEO friendly. 

- SSR will have a complied version of the HTML (which contains HTML, CSS and JS), so it will be `faster to load`.

(Eg: blog site)

## CSR

- CSR is a process where the client sends the HTML to the server. The server then sends the HTML to the client. The client then receives the HTML and displays it.

- CSR is mainly for rendering the `dynamic contents`, we want the interactive and the smooth user experience, eg: form interactions and so on.

- CSR will need to load HTML, then CSS and eventually load the JS and execute it and finally display the page, thats why its slower than SSR.

(Eg: e-commerce site needs user to fill the form and checkout, so it needs CSR)


## CSR and SSR Comparison (Short table view)

| Feature             |          SSR          |      CSR    |
|---------------------|-----------------------|-------------|
| Initial Load        |          Fast         | Slow        |
| SEO                 | Good                  | Bad         |
| User Experience     | Not Interactive       | Interactive | 
| Bandwidth           | Small                 | Large       |
| Time to Interactive | Fast                  | Slow        |
| Bundle Size         | Small                 | Large       |


## Why Server side rendering is better than client side rendering for SEO?

One code example can explain:

For CSR:

```html
<html>
  <head><title>My Shop</title></head>
  <body>
    <div id="root"></div>
    <script src="/bundle.js"></script>
  </body>
</html>
<!-- Googlebot sees: nothing inside <div id="root">. -->
```

For SSR:

```html
<html>
  <head>
    <title>My Shop - Fresh Coffee Beans</title>
    <meta name="description" content="Buy premium coffee beans online.">
  </head>
  <body>
    <h1>Premium Coffee Beans</h1>
    <p>Roasted daily, delivered fresh.</p>
  </body>
</html>
<!-- Googlebot instantly sees all meaningful content → great SEO. -->
```

As we can clearly see it, meta data tags have been rendered from SSR process, but CSR didn't ...
