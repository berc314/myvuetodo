<template>
    <div>
        <input type="text" class="todo-input" placeholder="Add ToDos..." v-model="newToDo" @keyup.enter="addToDo" /> <!--ToDo-Eingabefeld mit Schriftzug und der Möglichkeit per Enter-Taste den String zu übermitteln-->
        <button @click="addToDo" class="button-Add-item">   <!--Add-Button zur Übermittlung der ToDo-Eingaben-->
            Add
        </button>

        <br />
        <!--In diesem Block werden die unmittelbar eingegebenen ToDos als "Nicht erledigt" aufgelistet-->
        <div class="text-not-finished-invisible">
            <p id="textNotFinished">
                Not finished task:
            </p>
        </div>
        <div v-for="(todo,index) in todos" :key="todo.id" class="todo-item">            <!--Liste der eingegebenen ToDos im Array todos mit dem Schlüssel id-->
            <input type="checkbox" v-model="todo.completed" @click="ereignis(index)" /> <!--Erstelle eine Checkbox zur Erledigt-Markierung. click-Ereignis: Wenn die Checkbox geklickt wird, dann siehe weiter bei der Methode ereignis(index)-->
            <div :class="{completed : todo.completed}">
                {{todo.title}}                              <!--Auflistung der ToDos, die im ToDo-Eingabefeld eingegeben wurden-->
            </div>
            <div class="remove-item" @click="removeToDo(index)">    <!--Möglichkeit zum Entfernen der eingegebenen ToDo-->
                &times;
            </div>
        </div>
    </div>

    <!--In diesem Block befinden sich die ToDos, die als "Erledigt" markiert wurden-->
    <div>
        <div class="text-finished-invisible">
            <p id="textFinished">
                Finished tasks:
            </p>
        </div>
        <div v-for="(todo,index) in todosCompleted" :key="todo.id" class="todo-item">       <!--Liste der erledigten ToDos im Array todosCompleted mit dem Schlüssel id-->
            <input type="checkbox" @click="rueck(index)" checked="checked" />               <!--Die Checkbox soll als 'checked' dargestellt werden, da erledigt. Beim Click der Checkbox soll die erledigte Aufgabe zurückgesetzt werden können bzw. in das Array todos zurückgegeben werden-->
            <div :class="{completed : todo.completed}">
                {{todo.title}}                                              <!--Auflistung der erledigten ToDos-->
            </div>
            <div class="remove-item" @click="removeFilteredToDo(index)">    <!--Möglichkeit zum Entfernen der erledigten ToDo-->
                &times;
            </div>
        </div>
    </div>
</template>

<script>
	export default {
		name: 'ToDoList',
        data() {                        /*Festlegung der benötigten Daten im data-Bereich:*/             
            return{
                newToDo: '',            /* Die Funktion data speichert die Arrays todos und todosCompleted sowie die Start-IDs idForToDo und CompletedIdToDo und einen leeren Start-Eintrag zum Befüllen des Arrays todos*/
                idForToDo: 1,
                CompletedIdToDo: 1,
                todos:[],
                todosCompleted: []
            }
        },
        methods: {
            addToDo() {                             /* Die Methode addToDo fügt die ToDos unter "Not Finished Tasks" aufgelistet */
                if (this.newToDo.length == 0) {     /* Abfangen von leeren ToDo-Einträgen */
                    return;
                }

                document.getElementById('textNotFinished').style.visibility = 'visible';    /* Die unsichtbare Überschrift "Not Finished Task" wird sichtbar*/

                this.todos.push({                   /*Konkret wird mit der push-Methode...*/
                    id: this.idForToDo,             /*...wird die id gesetzt...*/
                    title: this.newToDo,            /*...die eingegebene ToDo unter title gesetzt...*/
                    'completed': false,             /*...und completed mit dem Boolean-Wert false gesetzt, damit das ToDo nicht durchgestrichen wird*/
                })

                    this.newToDo = ''               /*Der Array-Eintrag newToDo wird im Anschluss geleert und...*/
                this.idForToDo++                    /*idForToDo wird um eins erhöht.*/
            },
            removeToDo(index) {                     /*Mit der Methode removeToDo wird das ToDo aus der Liste entfernt: */
                this.todos.splice(index, 1)         /*Konkret wird mit der Methode splice an der Stelle index der einzige Eintrag entfernt*/
                if (this.todos.length < 1) {        /*Abfrage zur Unsichtbarkeit der Überschrift: Wenn das Array todos keinen Eintrag enthält, dann stelle "Not Finished Task" nicht dar*/
                    document.getElementById("textNotFinished").style.visibility = 'hidden';
                }
            },
            ereignis(index) {

                document.getElementById('textFinished').style.visibility = 'visible';       /* Die unsichtbare Überschrift "Finished Task" wird sichtbar*/
                
                this.todosCompleted.push(this.todos[index]),    /*Mit der push-Methode soll der index-Eintrag des Arrays todos in das Array todosCompleted hinzugefügt werden*/
                    this.todos.splice(index, 1)                 /*Entferne den einzigen Array-Eintrag in todos an der Stelle index*/
                    this.CompletedIdToDo++                      /*Erhöhe die Zahl von CompletedIdToDo um eins*/
            },
            rueck(index) {
                this.todos.push(this.todosCompleted[index]),        /*Mit der push-Methode soll der index-Eintrag des Arrays todosCompleted in das Array todos zurückgesetzt werden*/
                    this.todosCompleted[index].completed = false,   /*Setze dabei den boolean-Wert completed auf false, um zu zeigen, dass die Aufgabe noch nicht erledigt ist*/
                    this.todosCompleted.splice(index, 1)            /*Entferne den einzigen Array-Eintrag in todosCompleted an der Stelle index*/

                this.idForToDo++                                    /*Erhöhe die idForToDo um eins*/
            },
            removeFilteredToDo(index) {                                                     /*Mit der Methode removeFilteredToDo wird das ToDo aus der Liste entfernt: */
                this.todosCompleted.splice(index, 1)
                if (this.todosCompleted.length < 1) {                                       /*Abfrage zur Unsichtbarkeit der Überschrift: Wenn das Array todosCompleted keinen Eintrag enthält, dann stelle "Finished Task" nicht dar*/
                    document.getElementById("textFinished").style.visibility = 'hidden';
                }

                if (this.todosCompleted.length < 1 && this.todos.length < 1) {              /*Wenn beide Listen (todos und todosCompleted) leer sind, dann sollen beide Überschriften als Unsichtbar dargestellt werden*/
                    document.getElementById("textFinished").style.visibility = 'hidden';
                    document.getElementById("textNotFinished").style.visibility = 'hidden';
                }
            }
        }
	}
</script>

<!--Hier wird das Design festgelegt-->
<style lang="scss">
    .todo-input{
        width: 100%;        /*...in Bezug auf max-width der Klasse .container*/
        padding: 10px 18px;
        font-size: 18px;
        margin-bottom: 16px;
    }

    .button-Add-item {
        background-color: #4CAF50; /* Grüner Hintergrund des Add-Buttons */
        border: none;
        color: white;
        padding: 15px 32px;
        text-align: center;
        text-decoration: none;
        display: inline-block;
        font-size: 20px;
        margin-bottom: 5%;
        border-radius: 35%;
        cursor: pointer;    /*Aus dem Pfeil wird die Hand*/
    }

    .todo-item {
        margin-bottom: 12px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        background-color: rgba(128, 128, 128,.3);
        border-radius: 12px;
        padding: 5px 15px;
    }

    .remove-item{
        cursor: pointer;
        margin-left: 14px;
        &:hover {
                    color: black;
                }
    }

    .completed{
        text-decoration: line-through;  /*Der Text soll durchgestrichen werden, um kennzuzeichnen, dass diese erledigt ist*/
        color: grey;                    /*Der Schriftzug soll grau werden*/
    }

    .text-not-finished-invisible{
        visibility: hidden;             /*Zu Beginn soll die Überschrift "Not finished task" unsichtbar sein*/
        text-align: left;
    }

    .text-finished-invisible {          /*Zu Beginn soll die Überschrift "Finished task" unsichtbar sein*/
        visibility: hidden;
        text-align: left;
    }
</style>