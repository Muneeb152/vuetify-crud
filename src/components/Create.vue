<template>
    <v-card max-width="600" class="pa-8 mt-8 mb-8 rounded mx-auto" outlined>
        <v-card-text xs4>
            <v-layout row class=" mb-n6" wrap justify-space-between>
                <v-flex xs12>
                    <v-layout row wrap justify-space-between>
                        <v-flex xs12 lg12 class="mb-4">
                            <h2 class="font-weight-medium"> Add New</h2>
                        </v-flex>
                        <v-flex xs12 lg12 class="mb-n1">
                            <v-text-field v-model="model.user.name" label="Name" type="text" outlined dense
                                :rules="[() => !!model.user.name || 'This field is required']"></v-text-field>
                        </v-flex>
                        <v-flex xs12 lg12 class="mb-n1">
                            <v-text-field v-model="model.user.course" label="Course" type="text" outlined dense
                                :rules="[() => !!model.user.course || 'This field is required']"></v-text-field>
                        </v-flex>
                        <v-flex xs12 lg12 class="mb-n1">
                            <v-text-field v-model="model.user.email" label="Email" type="text" outlined dense
                                :rules="[() => !!model.user.email || 'This field is required']"></v-text-field>
                        </v-flex>
                        <v-flex xs12 lg12 class="mb-n1">
                            <v-text-field v-model="model.user.phone" label="Phone" type="text" outlined dense
                                :rules="[() => !!model.user.phone || 'This field is required']"></v-text-field>
                        </v-flex>
                        <v-btn @click="goBack" class="me-4">
                            Back
                        </v-btn>
                        <v-btn @click="newRegister" :disabled="!model.user.name || !model.user.course || !model.user.email || !model.user.phone" class="white--text" color="primary">
                            Save
                        </v-btn>
                    </v-layout>
                </v-flex>
            </v-layout>
        </v-card-text>
    </v-card>
</template>
<script>
import { mapActions, mapGetters } from "vuex";
import axios from 'axios';
export default {

    data() {
        return {
            model: {
                user: {
                    name:"",
                    course:"",
                    email:"",
                    phone:"",

                }
            },
        };
    },
    computed:
    {
    },
    methods:
    {
        newRegister() {
            axios.post('http://127.0.0.1:8000/api/students/', this.model.user).then(res => {
                alert("Added Successfully");

                this.model.user = {
                    name: '',
                    course: '',
                    email: '',
                    phone: '',
                }
                this.errorList = '';
            }).catch(function (error) {
                alert(error);

            })
        },
        goBack() {
            this.$router.push("/")
        },
    },
};
</script>
<style scoped>

.addSpaceRight {
    margin-right: 250px;
}

.reducewidth {
    width: 198px;
}
</style>
  