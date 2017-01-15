<template>
    <div class="todo">
        <h1>{{title}}</h1>
        <p>
            <label>{{labels.addNew}}</label>
            <input v-model="newItem" v-on:keyup.enter="addNewItem()"> 
        </p>
        <ul>
            <li v-for="(item, index) in items" @click="toggleTodoItemStatus(item)" @dblclick="deleteTodoItem(index)" :class="{completed: item.completed}">
                {{item.label}}
            </li>
        </ul>
    </div>
</template>

<script>
    import storage from './storage'

    export default {
        name: 'todo-list',
        data() {
            return {
                title: 'My TODO List',
                labels: {
                    addNew: 'Add new TODO item: '
                },
                items: [],
                newItem: ''
            }
        },
        created: function() {
            this.items = JSON.parse(storage.get('todo-list-items'));
        },
        methods: {
            toggleTodoItemStatus: function(item) {
                item.completed = !item.completed;
            },
            addNewItem: function() {
                this.items.push({label: this.newItem, completed: false});
                this.newItem = '';
            },
            deleteTodoItem: function(index) {
                this.items.splice(index, 1);
            }
        },
        watch: {
            items: {
                handler: function(val, oldVal) {
                    storage.set('todo-list-items', JSON.stringify(val));
                },
                deep: true
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin: 0 10px;
}

.completed {
    text-decoration: underline;
}

</style>