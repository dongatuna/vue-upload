<template>
    <form enctype="multipart/form-data" v-on:submit.prevent="sendFile" >
        <div v-if="message" 
            :class="`message ${error? 'is-danger':'is-success'}`">
        </div>

        <div class="message-body">{{message}}</div>
        <div class="field">

            <div class="file is-boxed is-primary">
                <label class="file-label">
                    <input 
                        v-on:change="selectFile"
                        ref="file" 
                        class="file-input"
                        type="file"/>
                

                    <span class="file-cta">
                        <span class="file-icon">
                            <i class="fas fa-upload"></i>
                        </span>
                        <span class="file-label">
                            Choose a file...
                        </span>
                    </span>

                    <span v-if="file" class="file-name">{{file.name}}</span>

                </label> 
            </div>
        </div>
        

        <div class="field">
            <button class="button is-info">Send</button>
        </div>
    </form>
    
</template>
<script>

import axios from "axios"
export default {
    name: "SimpleUpload",
    data(){
        return{
            file:"",
            message: "",
            error:false
        }
    },

    methods:{
        selectFile(){
            this.file = this.$refs.file.files[0];
            this.error= false;
            this.message = "";
        },

        async sendFile(){
            const formData = new FormData();

            formData.append('file', this.file);

            try{
                
                await axios.post('/upload', formData);

                this.message = "File has been uploaded";
                this.message = "";
                this.error =false;

            }catch(error){
                this.message = "There has been an error!";
                this.error = true;
            }
            


        }
    }
}
</script>
