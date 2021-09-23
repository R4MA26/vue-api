<template>
    <div class="container my-5">
        <div class="row justify-content-center">
            <div class="col-8">
                <router-link 
                :to="{ name:'transaction.index'}"
                class="btn btn-primary btn-sm rounded shadow mb-3">
                BACK</router-link>

                <div class="card rounded shadow">
                    <div class="card-header">
                        Created Transaction
                    </div>
                    <div class="card-body">
                        <form @submit.prevent="store">
                            <div class="mb-3">
                                <label for="title" class="form-label">Title</label>
                                <input type="text" class="form-control" v-model="transaction.title">
                                <div v-if="validation.title" class="text-danger">
                                    {{ validation.title[0] }}
                                </div>
                            </div>
                            <div class="mb-3">
                                <label for="amount" class="form-label">Amount</label>
                                <input type="number" class="form-control" v-model="transaction.amount">
                                <div v-if="validation.amount" class="text-danger">
                                    {{ validation.amount[0] }}
                                </div>
                            </div>
                            <div class="mb-3">
                                <label for="time" class="form-label">Time</label>
                                <input type="text" class="form-control" placeholder="yyy-mm-dd hh:mm:ss" v-model="transaction.time">
                                <div v-if="validation.time" class="text-danger">
                                    {{ validation.time[0] }}
                                </div>
                            </div>
                            <div class="mb-3">
                                <label for="type" class="form-label">Type</label>
                                <select id="" class="form-select" v-model="transaction.type">
                                    <option value="expense">Expense</option>
                                    <option value="revanue">Revanue</option>
                                </select>
                                <div v-if="validation.type" class="text-danger">
                                    {{ validation.type[0] }}
                                </div>
                            </div>
                            <button class="btn btn-outline-primary">Submit</button>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { reactive, ref } from 'vue'
import { useRouter } from 'vue-router'
import axios from 'axios'

export default {
    setup() {

        //state posts
        const transaction = reactive({
            title: '',
            amount: '',
            time: '',
            type: '',
        })


        //state validation
        const validation = ref([])

        //vue router
        const router = useRouter()

        function store() {

            let title   = transaction.title
            let amount = transaction.amount
            let time = transaction.time
            let type = transaction.type

            axios.post('http://127.0.0.1:8000/api/transaction', {
                title: title,
                amount: amount,
                time: time,
                type: type,
            }).then(() => {

                //redirect ke post index
                router.push({
                    name: 'transaction.index'
                })

            }).catch(error => {
                //assign state validation with error 
                validation.value = error.response.data
            })

        }

        return {
            transaction,
            validation,
            router,
            store
        }
    }
}
</script>