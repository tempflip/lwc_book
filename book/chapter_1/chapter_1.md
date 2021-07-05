# Quick start : The To-Do List

The *Hello World!* of frontend frameworks is probably the To-Do list. Its an easy application, but already contents some essential parts:

* Data binding to the template
* User input and user interaction
* Rendering of a list


In LWC you define templates and controllers in different files. Template is essentially a HTML file, while the controller is a Javascript class. Lets analyze the following HTML template:

* The whole *template* is inside a main `<template>` tag
* You *render in a loop* every item on the To-Do List after each other. You do this with inserting another `<template>` tag with two attributes: `for:each` refers to the backend property (the list to render, `todoList`); and `for:item` defines the name of the given element in the local context (`el`).
* At the `<button>` tag you can see how *user interaction* works: standard event attributes refer to a controller method. You see this also at the `oninput` property of the `<input>` tag.
* At the {el.text} property you can see how you *bind conroller data* to the template.
* Finally, there are two variants of buttons (enabled and disabled), which are rendered conditionally, using the `if:true`/`if:false` attributes.

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
        <button if:true={addItemDisabled} disabled onclick={onAddItem}>Add Item</button>
        <button if:false={addItemDisabled} onclick={onAddItem}>Add Item</button>
    </div>
    
</template>
```

* You render the list of your To-Do items in a loop. In or


```
import { LightningElement } from 'lwc';

export default class ToDo extends LightningElement {
    newItemText = '';
    todo = [
        { text: 'Buy food'}
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
