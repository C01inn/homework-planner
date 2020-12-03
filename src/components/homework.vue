<template>
    <div id="homework-table">
        <button @click="showForm()" style="margin-bottom: 3rem; margin-top: 1rem;" class="waves-effect waves-light btn orange darken-3">Show / Hide Form</button>
        <div id="add" class="container">          
                <form id="forms" @submit.prevent="addAssignment()">
                    
                    
                    <div class="row">
                        <div class="input-field col s12">
                            <input id="assignment" type="text" class="validate" required>
                            <label for="assignment">Assignment</label>
                        </div>
                        <div class="input-field col s12">
                            <input id="class" type="text" class="validate" required>
                            <label for="class">Class</label>
                        </div>
                        <div class="">
                            <input type="datetime-local" placeholder="Due Date" id="due-time" name="meeting-time" min="2020-11-03T00:00" required>
                        </div>
                        <div class="input-field col s12">
                            <input id="notes" type="text" class="validate">
                            <label for="notes">Notes</label>
                        </div>
                        <button type="submit" class="waves-effect waves-light btn">Add Assignment</button>

                    </div>
                    

                    
                </form>
        </div>


        <table>
            <thead>
                <tr>
                <th>Assignment</th>
                <th>Class</th>
                <th>Due Date</th>
                <th>Notes</th>
                <th>Remove</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="assignment in work" v-bind:key="assignment" class="table-row">
                    <td data-label="Assignment">{{assignment.name}}</td>
                    <td data-label="Class">{{assignment.class}}</td>
                    <td v-if="isPast(assignment.due)" data-label="Due Date" style="color: red;" :id="assignment.id" class="table-date">{{assignment.due}}</td>
                    <td v-else data-label="Due Date" class="table-date" :id="assignment.id">{{assignment.due}}</td>
                    <td data-label="Notes">{{assignment.notes}}</td>
                    <td data-label="remove" class="remove">
                        <div @click="removeAssignment(assignment)" class="remove-img" style="height: 64px;"/>
                    </td>
                </tr>
            </tbody>
        </table>


    </div>
</template>

<script>
import M from 'materialize-css'

export default {
    name: "Homework",
    data() {
        return {
            work: []
        }
    },
    methods: {
        showForm() {
            if (document.querySelector("#add").style.display === 'none')
                document.querySelector("#add").style.display = 'block'
            else
                document.querySelector("#add").style.display = 'none'
        },
        removeAll() {
            localStorage.setItem("work", JSON.stringify([]))
            this.work = this.sortData(this.getAssignments())

        },
        addAssignment() {
            var name = document.getElementById("assignment").value;
            var classname = document.getElementById("class").value;
            var datetime = document.getElementById("due-time").value;
            var notes = document.getElementById("notes").value;

            if (notes === "" || notes === null) {
                notes = "None"
            }

            var newWork = {
                name: name,
                class: classname,
                due: datetime,
                notes: notes,
                id: Date.now()
            }
            var oldWork = JSON.parse(localStorage.getItem("work"))
            if (oldWork === null) {
                oldWork = []
            }
            oldWork.push(newWork)
            localStorage.setItem("work", JSON.stringify(oldWork))
            this.work = this.sortData(this.getAssignments())
            this.formatDates()

            document.getElementById("assignment").value = ''
            document.getElementById("class").value = ''
            document.getElementById("due-time").value = ''
            document.getElementById("notes").value = ''
        },
        getAssignments() {
            var work = JSON.parse(localStorage.getItem("work"))
            return work
        },
        removeAssignment(assignment) {
            var oldWork = this.getAssignments()
            oldWork.splice(oldWork.indexOf(assignment))

            localStorage.setItem("work", JSON.stringify(oldWork))

            this.work = this.sortData(this.getAssignments())

            
        },
        sortData(data) {
            data.sort((a, b) => {
                var diff = new Date(Date.parse(a.due)) - new Date(Date.parse(b.due));
                return diff/(Math.abs(diff)||1);
            });
            return data
        },
        isPast(datee) {
            console.log(datee)
            if (Date.parse(datee)-Date.parse(new Date())<0) {
                return true
            }
            return false
        },
        formatDates() {
            var tableDates = document.getElementsByClassName("table-date")
            tableDates.forEach(timee => {
                if (timee.innerHTML.includes("T")) {
                    var options = {
                        weekday: 'long',
                        year: 'numeric',
                        month: 'long',
                        day: 'numeric',
                        hour: 'numeric',
                        minute: 'numeric'
                    }
                    let time = timee.innerHTML
        
                    
                    var newTime = new Date(time)
                    newTime = newTime.toLocaleDateString("en-US", options)

                    document.getElementById(timee.id).innerHTML = newTime    
                }
                
                
            })
        }
    },
    mounted() {
        // initialize materialize css
        M.AutoInit()
        
        document.addEventListener('DOMContentLoaded', function() {
            var elems = document.querySelectorAll('.datepicker');
            var instances = M.Datepicker.init(elems, options);
        });

        // set form dispaly to none
        document.getElementById('add').style.display = 'none';

        this.work = this.sortData(this.getAssignments());
        this.formatDates();
        setInterval(this.formatDates, 100);

    }
}
</script>
<style scoped>
/* https://codepen.io/jreece/pen/vEqPgL */

@import url("../assets/css/table.css");
@import url("../assets/css/form.css");

.row {
    width: 60%;
}
table {
    margin-bottom: 10rem;
}

#forms {
    margin-right: auto;
    margin-left: auto;
}

#add {
    display: none;
    margin-right: auto;
    margin-left: auto;
    margin-bottom: 2rem;
}

/* for mobile */
@media only screen and (max-width: 769px) {
    .row {
        width: 93%;
    }
    form input {
        width: 100%;
    }
}



</style>