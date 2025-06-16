# Web Accessability

- Please follow the A11y standards ✅
- Using chrome accessibility tools 
  - When opens color pad: contrast-ratio, color-contrast, etc ✅
  - Find Enulate vision deficiencies (eg: Find Blurred vision, color blindness, etc) ✅
- 

## labels -> connect label to input -> aria-label -> connect elements to a label by id

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
  </head>
  <body>
    <form>
      <!-- <label for="name">Name</label>
      <input id="name" type="text" /> -->
      <!-- This is same with above code, just add aria-label -->
      <input id="name" type="text" aria-label="Name" />
      <!-- aria-labelledby: connect elements to a label by id -->
      <div id="text">Text</div>
      <input aria-labelledby="text" type="text" />
      <a href="#">View something (instead of using click me as text)</a>
    </form>
  </body>
</html>
```

## Add new DOM element -> notify user -> aria-live="polite"

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
  </head>
  <body>
    <form data-form>
      <input id="name" type="text" aria-label="Name" placeholder="Name" />
      <button>Save</button>
    </form>
    <div data-container aria-live="polite"></div>
    <!-- aria-live: notify user when content changes (eg: new DOM element is added, like below script example) -->
    <script>
      const form = document.querySelector("[data-form]");

      form.addEventListener("submit", (e) => {
        e.preventDefault();
        const container = document.querySelector("[data-container]");
        const div = document.createElement("div");
        div.innerText = "Saved data";
        container.appendChild(div);
      });
    </script>
  </body>
</html>
```

## keyboard -> tabindex="0" -> :focus-visible

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
  </head>
  <style>
    button:focus-visible {
      /* :focus-visible -> will help user which button is focused at moment */
      margin: 10px;
    }
  </style>
  <body>
    <button tabindex="0">One</button>
    <!-- tabindex="0" -> will help user to focus on the button when they press tab key -->
    <button>Two</button>
    <button>Three</button>
  </body>
</html>
```


## Video -> add caption (For people who is not easier to hear)

```html
<video controls aria-label="Video with caption">
  <source src="video.mp4" type="video/mp4">
  <track src="captions.vtt" kind="captions" srclang="en" label="English">
</video>
```

## Tools for helping improving accessibility

- Not sure AI tools helps, but can give it a try
- Lighthouse (Chrome DevTools), and follow the suggestions make your site better ~
- Using `chrome://accessibility` to check accessibility of your site (For accessibility debugging !!!)
