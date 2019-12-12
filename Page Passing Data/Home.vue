<template>
    <Page class="page">
        <ActionBar class="action-bar bg-purple">
            <Label class="action-bar-title" text="Home"></Label>
        </ActionBar>

        <StackLayout class="p-5">
           <button class="btn btn-red btn-active btn-rounded-lg m-t-5" @tap="page2">Page 2</button>
            <!-- <StackLayout class="form border m-5 p-5">

                    <StackLayout class="input-field">
                        <TextField id="notif_id" class="input" hint="ID"/>
                        <StackLayout class="hr-light"></StackLayout>
                    </StackLayout>
                    
                    <StackLayout class="input-field">
                        <TextField id="notif_title" class="input" hint="Title"/>
                        <StackLayout class="hr-light"></StackLayout>
                    </StackLayout>

                    <StackLayout class="input-field">
                        <TextField id="notif_body" class="input" hint="Body"/>
                        <StackLayout class="hr-light"></StackLayout>
                    </StackLayout>

                    
            
            </StackLayout> -->

        </StackLayout>
    </Page>
</template>

<script>

    // const view = require("tns-core-modules/ui/core/view");
    // import {ScrollView} from "tns-core-modules/ui/scroll-view";
    // import * as notif from "nativescript-local-notifications";
    import Second from "./Second";

    export default {
        methods:{
            page2(){
                let obj = {
                    id: 1,
                    msg:"This data from first page",
                }
                
                this.$navigateTo(Second,{props:obj});
            }
        },
        computed: {
            message() {
                return "Blank {N}-Vue app";
            }
        }
    };
</script>

<style scoped lang="scss">
    // Start custom common variables
    @import '../app-variables';
    // End custom common variables

    // Custom styles
    .fa {
        color: $accent-dark;
    }

    .info {
        font-size: 20;
    }

    .border{
        border-radius: 10;
        border-color: black;
        border-width: 1px;
    }

    .btn-red{
        background-color:#ff1744;
        border-color:#ff1744;
        color:#fff
    }

    .btn-red.btn-active{
        background-color:#d50000;
        border-color:#d50000;
        color:#fff
    }
</style>
