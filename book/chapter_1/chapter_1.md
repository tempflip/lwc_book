# Quick start : The To-Do List

The *Hello World!* of the frontend frameworks is probably the *To-Do list app*. It's an easy application, but already has many essential parts:

* Data binding to a template
* Rendering of a list
* User input and user interaction

In LWC you define templates and controllers in different files. Template is essentially a HTML file, while the controller is a Javascript class. Lets analyze the following HTML template:

* The whole **template** is inside a main `<template>` tag
* You **render in a loop** every item on the To-Do List after each other. You do this with inserting another `<template>` tag with two attributes: `for:each` refers to the backend property (the list to render, `todoList`); and `for:item` defines the name of the given element in the local context (`el`). So you want to refer to a given element on the To-Do list, you make it with `{el}`. For example, `{el.id}` and `{el.text}`
* At the `<button>` tag you can see how **user interaction** works: standard event attributes refer to a controller method. When user clicks on button, `onRemove()` method is called. You see this also at the `oninput` property of the `<input>` tag.
* At the {el.text} property you can see how you **bind data** to the template. Remember, there `{el}` here is a local, loop-scope property; but you can see a template-scope property at `{newItemText}`.
* Finally, there are two variants of buttons (enabled and disabled), which are **rendered conditionally**, using the `if:true`/`if:false` attributes.

So to sum it up, this template renders the `{todoList}`, and waiting for user interaction, which can be `{onAddItem}` and `{onRemove}`.

```
<template>

    <h1>To-Do List</h1>

    <template for:each={todoList} for:item="el">
        <div class="item" key={el.id}>
            <button value={el.id} onclick={onRemove}>Remove</button>
            {el.text}
        </div>
    </template>

    <div class="item">
        <input value={newItemText} oninput={onNewItemChange} class="new-item" />
        <button if:true={addItemDisabled} disabled>Add Item</button>
        <button if:false={addItemDisabled} onclick={onAddItem}>Add Item</button>
    </div>
    
</template>
```

Now let's look into the controller. This simple controller shows you the following things:
* Create properties, which you **bind** to the template
* **Getters**
* **Event listeners** for user interaction
* The **one-way data binding** principle

The controller should always export a class, which is a child of the `LightningElement` base class.

```
import { LightningElement } from 'lwc';

export default class ToDo extends LightningElement {
    newItemText = '';
    todo = [
        { text: 'Buy Pizza'}
    ];

    get todoList() {
        return this.todo.map((el, index) => ({...el, id : index}));
    }

    get addItemDisabled() {
        return !this.newItemText;
    }

    onNewItemChange(ev) {
        this.newItemText = ev.target.value;
    }

    onAddItem() {
        this.todo.push({text : this.newItemText});
        this.newItemText = '';
    }

    onRemove(ev) {
        this.todo = this.todoList.filter(el => ev.target.value != el.id);
    }
}
```
