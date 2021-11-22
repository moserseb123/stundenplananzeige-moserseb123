<template>
    <div>
        <div id="block">
            <h2>Klasse: {{ timetableParams.schoolClass }}</h2>

            <h2>am {{ timetableParams.datum }}</h2>

            <table>
                <thead>
                <tr>
                    <th>
                        Beginn
                    </th>
                    <th>
                        Ende
                    </th>
                    <th>
                        Fach
                    </th>
                </tr>
                </thead>
                <tbody>
                <tr v-for="(item, index) in timetableParams.timeTable" :key="index">
                    <td>{{item.beginn}}</td>
                    <td>{{item.end}}</td>
                    <td>{{item.fach}}</td>
                    <td>
                        <button @click="onUpPressed(index)">UP</button>
                        <button @click="onDownPressed(index)">DOWN</button>
                        <button @click="onRemovePressed(index)">DEL</button>
                    </td>
                </tr>
                </tbody>
                <tfoot>
                <tr>
                    <td>
                        <input
                                type= "text"
                                v-model= "newEntry.beginn"
                                placeholder= "Beginn (erforderlich) "/>
                    </td>
                    <td>
                        <input
                                type= "text"
                                v-model= "newEntry.end"
                                placeholder= "Ende (erforderlich) "
                        />
                    </td>
                    <td>
                        <input
                                type= "text"
                                v-model= "newEntry.fach"
                                placeholder= "Fach (erforderlich) "
                        />
                    </td>
                    <td>
                        <button @click="unitAdd(newEntry)">Add</button>
                    </td>
                </tr>
                </tfoot>
            </table>
        </div>
    </div>
</template>

<script>
    export default {
        name: "TimeTable",
        props: {
            timetableParams: {
                Object,
                required: true
            },
        },

        setup(props, context) {

            //Neues Objekt erstellt
            let newEntry = {
                beginn:"",
                end:"",
                fach:""
            }

            function onUpPressed(index) {
                console.log("onUpPressed, Index: "+index);
                context.emit('lessonMoveUp',index);
            }

            function onDownPressed(index) {
                console.log("onDownPressed, Index: "+index);
                context.emit('lessonMoveDown',index);
            }

            function onRemovePressed(index) {
                console.log("onRemovePressed, Index: "+index);
                context.emit('removeLesson',index);
            }

            //Ruft eine Funktion in der Main App auf (App.vue)
            function unitAdd(newEntry) {
                console.log("unitAdd");

                //Interrupt wird erzeugt und sendet Objekt an App.vue weiter
                context.emit('addUnit', newEntry);
            }

            return {
                onUpPressed,
                onDownPressed,
                onRemovePressed,
                unitAdd,
                newEntry
            }
        }
    }
</script>

<style scoped>

    li {
        list-style: none;
    }

    #block {
        text-align: left;
        margin-left: 5em;
        margin-top: 5em;
    }

    table {
        width: 100%;
    }
</style>