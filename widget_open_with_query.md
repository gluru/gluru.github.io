## Open widget with the query 'Where is my order'

Imagine you have a page where the user can see a list of his orders. And you want to provide an easy way for the user to ask for more details about every order. 

You could add a button next to every order that when clicked executes a piece of javascript to open the widget and automatically sending that specific query.


```html
<body>
... content
<div id="order_item">
    Order #ord-123-2020 on delivery 
    <button 
        onclick="window.kare.open({query:'Where is my order #ord-123-2020'})"
    >
        Click to get more information
    </button>
</div>
... more content
</body>
```