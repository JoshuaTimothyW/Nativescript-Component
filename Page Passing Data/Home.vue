<template>
    <Page class="page">
        <ActionBar class="action-bar">
            <Label class="action-bar-title" text="Home"></Label>
        </ActionBar>

        <StackLayout class="p-5 bg-gray">
            <StackLayout class="m-b-5" orientation="horizontal">
                <Image class="" src="~/img/icon.jpg" stretch="none" />
                <Label class="h2">Hi, My Name is Homer</Label>
            </StackLayout>
            <SearchBar class="bg-white" hint="Cari Contact" />
           <button class="btn btn-red btn-active btn-rounded-lg m-t-5" @tap="page2">To Page 2</button>
           <button class="btn btn-red btn-active btn-rounded-lg m-t-5" @tap="chat">To Chat Page</button>
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
    import Chat from "./Chat";

    export default {
        methods:{
            page2(){
                let obj = {
                    data:[
                        {        
                            id: 1,
                            msg:"First Data",
                        },
                        {        
                            id: 2,
                            msg:"Second Data",
                        },
                        {        
                            id: 3,
                            msg:"Third Data",
                        },
                    ]
                }
                
                this.$navigateTo(Second,{props:obj});
            },
            chat(){
                let obj = {
                    name: "Homer"
                }

                this.$navigateTo(Chat,{props:obj})
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

    .bg-gray{
        background-color:#e7dddd;
    }

    .bg-purple{
		background-color:#8A2BE2;
	}

    .bg-white{
		background-color:#ffffff;
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
