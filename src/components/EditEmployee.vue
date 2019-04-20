<template>
    <div id="edit-employee">
        <h3>Edit Employee</h3>
        <div class="row">
            <form @submit.prevent="updateEmployee" class="col s12">
                <div class="row">
                    <div class="input-field col s12">
                        <input disabled type="text" v-model="employee_id" required>
                        <label class="active">Employee ID</label>
                    </div>
                </div>
                <div class="row">
                    <div class="input-field col s12">
                        <input type="text" v-model="name" required>
                        <label class="active">Nam</label>
                    </div>
                </div>
                <div class="row">
                    <div class="input-field col s12">
                        <input type="text" v-model="dept" required>
                        <label class="active">Department</label>
                    </div>
                </div>
                <div class="row">
                    <div class="input-field col s12">
                        <input type="text" v-model="position" required>
                        <label class="active">Position</label>
                    </div>
                </div>
                <button type="submit" class="btn">Submit</button>
                <router-link to='/' class="btn grey">Cancel</router-link>
            </form>
        </div>
    </div>
</template>

<script>
import db from './firebaseInit'
export default {
    name: 'edit-employee',
    data(){
        return{
            employee_id: null,
            name: null,
            dept: null,
            position: null,
        }
    },
    beforeRouteEnter (to, from, next) {
        next()
    },
    watch: {
        '$route' (to, from){
            this.fetchData()
        }
    },

    created() {
        this.fetchData()
    },
    methods: {
        test(){
            console.log("OK MAN-----------")
            this.name = "joe king"
        },
        fetchData(){
            db.collection('employes').where('employe_id', '==', this.$route.params.employee_id).get()
                .then(querySnapshot => {
                    querySnapshot.forEach(doc => { 
                        this.employee_id = doc.data().employe_id
                        this.name = doc.data().name
                        this.dept = doc.data().dept
                        this.position = doc.data().position
                    })
                })
        },
        updateEmployee(){
            db.collection('employes').where('employe_id', '==', this.$route.params.employee_id).get()
                .then(querySnapshot => {
                    querySnapshot.forEach(doc => {
                        doc.ref.update({
                             employe_id:this.employee_id,
                             name:this.name,
                             dept:this.dept,
                             position:this.position,
                        })
                        .then(() => {
                            this.$router.push({name: 'view-employee', params:{employee_id: this.employee_id}})
                        }) 
                    })
                })
        }
    }
}
</script>
