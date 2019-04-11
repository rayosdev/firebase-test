<template>
    <div id="dashboard">
        <h3>Dashboard</h3>
        <ul class="collection width-header">
            <li class="collection-header">
                <h4>Employees</h4>
            </li>
            <li 
                v-for="employee in employees" 
                v-bind:key="employee.id" 
                class="collection-item"
            >
            <div class="" :load="log(employee)"></div>
               <div class="chip">{{employee.dept}}</div>{{employee.employe_id}} {{employee.name}}
               <router-link class="secondary-content" 
                    v-bind:to="{name: 'view-employee', params :{employee_id: employee.employe_id}}"
                >
                   <i class="fa fa-eye"></i>
               </router-link>
            </li>
        </ul>
        <div class="fixed-action-btn">
            <router-link to="/new" class="btn-floating btn-large red">
                <i class="fa fa-plus"></i>
            </router-link>
        </div>
    </div>
</template>

<script>
import db from './firebaseInit'
export default {
    name: 'dashboard',
    data(){
        return{
            employees: []
        }
    },
    created(){
        db.collection('employes').orderBy('dept').get()
            .then(querySnapshot => {
                querySnapshot.forEach(doc => {
                    // console.log(doc.id)
                    // console.log(doc.data())
                    const data = {
                        id: doc.id,
                        employee_id: doc.data().employe_id,
                        name: doc.data().name,
                        dept: doc.data().dept,
                        position: doc.data().position,
                    }
                    this.employees.push(data)
                })
            })
    },
    methods:{
        log(item){
            console.log(item)
        }
    }
}
//test
</script>
