<template>
    <q-dialog v-model="store.state.newRecord" :persistent="uploading" @hide="() => store.commit('handleNewRecord', false)">
        <div class="bg-gray-100 flex w-[800px] p-4 text-black">
            <p class="w-full text-center text-lg font-bold text-[#28025A] pt-2 pb-5">Upload New Record</p>
            <div class="grid grid-cols-2 gap-3">
                <div class="col-span-1 h-full">
                    <q-uploader url="http://file.europa777.com/upload" label="Record Files" hide-upload-btn
                        ref="uploader" @finish="() => uploading = false" @added="addedFile = true"
                        class="text-black bg-transparent border-[#28025A] border-[1px] max-w-full h-full"
                        color="[#28025A]" />
                </div>
                <div class="col-span-1 flex flex-col gap-3">
                    <q-input v-model="agentName" label="Agent" outlined placeholder="Input Agent Name" color="[#28025A]"
                        input-class="text-black text-md" dark="true" />
                    <q-input v-model="date" outlined color="[#28025A]" placeholder="YYYY-MM-DD HH-MM"
                        input-class="text-black text-md" dark="true">
                        <template v-slot:prepend>
                            <q-icon name="event" class="cursor-pointer">
                                <q-popup-proxy cover transition-show="scale" transition-hide="scale">
                                    <q-date v-model="date" mask="YYYY-MM-DD HH:mm">
                                        <div class="row items-center justify-end">
                                            <q-btn v-close-popup label="Close" color="primary" flat />
                                        </div>
                                    </q-date>
                                </q-popup-proxy>
                            </q-icon>
                        </template>

                        <template v-slot:append>
                            <q-icon name="access_time" class="cursor-pointer">
                                <q-popup-proxy cover transition-show="scale" transition-hide="scale">
                                    <q-time v-model="date" mask="YYYY-MM-DD HH:mm" format24h>
                                        <div class="row items-center justify-end">
                                            <q-btn v-close-popup label="Close" color="primary" flat />
                                        </div>
                                    </q-time>
                                </q-popup-proxy>
                            </q-icon>
                        </template>
                    </q-input>
                    <q-btn label="Upload" no-caps color="[#28025A]" @click="() => { uploading = true; uploader.upload(); }" />
                </div>
            </div>
        </div>
    </q-dialog>
</template>
<script setup>
import { ref } from "vue";
import { useStore } from "vuex";
import { useRouter } from "vue-router";
import axios from 'axios';

const store = useStore();
const router = useRouter();

const agentName = ref("");
const date = ref(new Date())
const uploader = ref();
const uploading = ref(false);
const addedFile = ref(false);
let formData = {
    file: null,
}

// const onSubmit = (evt) => {
//     console.log("-", formData);
//     const mformData = new FormData();
//     mformData.append('file', formData.file);

//     // Send the form data to the server using an HTTP request (e.g., using Axios)
//     // Example using Axios:
//     axios.post('http://localhost:3033/upload', mformData, {
//     headers: {
//         "Content-Type": 'multipart/form-data',
//         "Access-Control-Allow-Origin": "*",
//         "Access-Control-Allow-Methods": "GET, POST, PATCH, PUT, DELETE, OPTIONS",
//         "Access-Control-Allow-Headers": "Origin, Content-Type, X-Auth-Token"  
//     }
//     })
//     .then(response => {
//     // Handle the response from the server
//     console.log('File uploaded successfully');
//     })
//     .catch(error => {
//     // Handle any errors
//     console.error('Error uploading file', error);
//     });
// }

const uploadAvailable = () => {
    if (agentName.value == "") {
        store.commit('handleNotification', { type: 'Error', message: "Enter agent name." });
        return false;
    }
    else if (!addedFile) {
        store.commit('handleNotification', { type: 'Error', message: "Select file to upload." });
        return false;
    }
    return true;
}

</script>