## Open widget with the query 'Where is my order'

Imagine you have a page with a search bar. The problem you want to solve is to open the widget with the query the user typed in the search bar.

```html
<html>
  <head>
    <script src="https://widget-beta.stg.karehq.com/kare_loader.js?appId=441f5fb0-db0d-46f2-80ed-3459fddddd4d"></script>
    <script>
      window.onload = ()=>{
          window.kare.open({query:"user query"})
      }
    </script>
  </head>
  <body>
    <div id="container">
      List of Content found {...list of items}
    </div>
    ... more content
  </body>
</html>
```
