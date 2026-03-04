<template>
    <v-container class="container" fluid align="center" justify="center">
        <v-row class="bg-surface">
            <v-col class="col col2" align="center">
                <v-sheet>
                    <v-card class="text-secondary" title="Combination" color="surface" subtitle="Enter your four-digit passcode" variant="tonal" flat>
                    </v-card>
                </v-sheet>
                <v-otp-input length="4" style="color:blueviolet" v-model="passcode">
                </v-otp-input>
            </v-col>
        </v-row>
        <v-row>
            <v-col class="col col2" align="center">
                <v-btn class="rounded-t-lg" align="center" variant="tonal" style="color:maroon" @click="readPasscode" >
                    Submit
                </v-btn>
            </v-col>
        </v-row>
    </v-container>
</template>

<script setup>
/** JAVASCRIPT HERE */

// IMPORTS
import { ref,reactive,watch ,onMounted,onBeforeUnmount,computed } from "vue";  
import { useRoute ,useRouter } from "vue-router";
import { useMqttStore } from "@/store/mqttStore";
import { useAppStore } from "@/store/appStore";
import { storeToRefs } from "pinia";

// VARIABLES
const Mqtt = useMqttStore();
const AppStore = useAppStore();
const { payload, payloadTopic } = storeToRefs(Mqtt);
 
// VARIABLES
const router      = useRouter();
const route       = useRoute();  
const passcode = ref("");


function readPasscode() {
    console.log(passcode.value);
    AppStore.setPasscode(passcode.value);
    
}

onMounted(()=>{
    // THIS FUNCTION IS CALLED AFTER THIS COMPONENT HAS BEEN MOUNTED
    Mqtt.connect();
    setTimeout( ()=>{ 
        // Subscribe to each topic 
        Mqtt.subscribe("620172690"); 
        Mqtt.subscribe("620172690_sub"); 
    },3000);
});

onBeforeUnmount(()=>{
    // THIS FUNCTION IS CALLED RIGHT BEFORE THIS COMPONENT IS UNMOUNTED
    Mqtt.unsubcribeAll();
});
</script>


<style scoped>
/** CSS STYLE HERE */
.container {
  background-color: #f5f5f5;
  width: 1200px;
}


</style>
  