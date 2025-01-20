```JavaScript
// URL (required), options (optional)
fetch('https://url.com/some/url')
  .then(function(response) {
    // Successful response :)
  })
  .catch(function(err) {
    // Error :(
  });
```
For security reasons, by default, browsers restrict HTTP requests to outside sources (which is exactly what we’re trying to do here). There’s a very small amount of setup that we need to do to make fetching work.
With fetch, you are able to easily supply a JavaScript object for options. It comes right after the URL as a second parameter to the fetch function:
```javascript
fetch('url.url.com/api', {
  mode: 'cors'
});
```
Adding the `{mode: 'cors'}` after the URL, as shown above, will solve our problems for now. In the future, however, you may want to look further into the implications of this restriction.

### Let’s do this
For now, we’re going to keep all of this in a single HTML file. So go ahead and create one with a single blank image tag and an empty script tag in the body.
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <img src="#">
  <script>
  </script>
</body>
</html>
```
In the script tag, let’s start by selecting the image and assigning it to a variable so that we can change the URL once we’ve received it from the Giphy API.
```html
<script>
  const img = document.querySelector('img');
</script>
```
Adding fetch with our URL from above is also relatively easy:
```html
<script>
  const img = document.querySelector('img');
  fetch('https://api.giphy.com/v1/gifs/translate?api_key=YOUR_KEY_HERE&s=cats', {mode: 'cors'})
    .then(function(response) {
      console.log(response.json());
    });
</script>
```
You should now be able to open the HTML file in your browser, and while you won’t see anything on the page, you _should_ have something logged in the console. The trickiest part of this whole process is deciphering how to get to the data you desire from the server’s response. In this case, inspecting the browser’s console will reveal that what’s being returned is _another_ Promise… to get the data we need another `.then()` function.
```html
<script>
  const img = document.querySelector('img');
  fetch('https://api.giphy.com/v1/gifs/translate?api_key=YOUR_KEY_HERE&s=cats', {mode: 'cors'})
    .then(function(response) {
      return response.json();
    })
    .then(function(response) {
      console.log(response);
    });
</script>
```
Now we have a JavaScript object and if you inspect it closely enough you’ll find that the data we need (an image URL) is nested rather deeply inside the object:
![response](https://cdn.statically.io/gh/TheOdinProject/curriculum/284f0cdc998be7e4751e29e8458323ad5d320303/javascript/async-apis/APIs/imgs/00.png)
To get to the data we need to drill down through the layers of the object until we find what we want!
```html
<script>
  const img = document.querySelector('img');
  fetch('https://api.giphy.com/v1/gifs/translate?api_key=YOUR_KEY_HERE&s=cats', {mode: 'cors'})
    .then(function(response) {
      return response.json();
    })
    .then(function(response) {
      console.log(response.data.images.original.url);
    });
</script>
```
Running the file should now log the URL of the image. All that’s left to do is set the source of the image that’s on the page to the URL we’ve just accessed:
```html
<script>
  const img = document.querySelector('img');
  fetch('https://api.giphy.com/v1/gifs/translate?api_key=YOUR_KEY_HERE&s=cats', {mode: 'cors'})
    .then(function(response) {
      return response.json();
    })
    .then(function(response) {
      img.src = response.data.images.original.url;
    });
</script>
```
If all goes well, you should see a new image on the page every time you refresh!

