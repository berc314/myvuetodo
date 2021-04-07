<template>
    <div id="app" class="container"> <!--Alles was unter id="app" aufgelistet ist, soll reaktiv sein und mit der Vue-Instanz (siehe main.js) interagieren-->
        <h1>My ToDo List</h1>
        <ToDoList />            <!--Aufruf der ToDoList in der Datei ToDoList.vue-->
    </div>
</template>

<script>

    import ToDoList from './components/ToDoList'    /* Import der Datei ToDoList */

    export default {

        name: 'App',
        components: {
            ToDoList,
        },
    }

</script>

<!--Festlegung der grundlegenden Styles mit den jeweiligen Klassen-->
<style>

    * {
        box-sizing: border-box;
    }

    .container {
        max-width: 300px;
        margin: 0 auto;
    }

    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        color: black;
        margin-top: 100px;
        font-size: 20px;
        text-align: center;
    }
</style>

