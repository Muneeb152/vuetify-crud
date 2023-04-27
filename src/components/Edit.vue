<template>
    <v-card max-width="600" class="pa-8 mt-8 mb-8 rounded mx-auto" outlined>
        <v-card-text xs4>
            <v-layout row class=" mb-n6" wrap justify-space-between>
                <v-flex xs12>
                    <v-layout row wrap justify-space-between>
                        <v-flex xs12 lg12 class="mb-4">
                            <h2 class="font-weight-medium"> Edit Record</h2>
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
                        <!-- <v-flex xs12 lg12 class="mb-n1">
                            <v-textarea v-model="model.user.body" name="input-7-1" outlined label="Body" auto-grow
                                :rules="[() => !!model.user.body || 'This field is required']"></v-textarea>
                        </v-flex> -->

                        <v-btn @click="goBack" class="me-4">
                            Back
                        </v-btn>
                        <v-btn :disabled="!model.user.name || !model.user.course || !model.user.email || !model.user.phone" @click="editRecord" class="white--text"
                            color="primary">
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
    props: ['userObj'],
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
 
    mounted() {
        this.model.user = this.userObj;
    },
    methods:
    {
        editRecord() {
            axios.put(`http://127.0.0.1:8000/api/students/${this.userObj.id}/edit`, this.model.user).then(res => {
                alert("Record Edited Successfully");
                this.$emit('goBack', false);
            }).catch(function (error) {
                alert(error);

            })
        },

        goBack() {
            this.$emit('goBack', false);
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
  