<template>
    <Page class="page">
        <ActionBar class="action-bar bg-purple">
              <NavigationButton text="Go back" android.systemIcon="ic_menu_back" @tap="$navigateBack" />
            <Label class="action-bar-title" text="Second Page"></Label>
        </ActionBar>

        <StackLayout class="p-10">
            <!-- <button class="btn btn-primary btn-active btn-rounded-lg m-t-5" @tap="$navigateBack">Back</button> -->
            <ListView for="item in this.data">
                <v-template class="m-y-10">
                   <StackLayout class="border m-y-20" >
                        <label class="text-center h1" :text="'Message ID: '+item.id"></label>
                        <StackLayout class="hr-light"></StackLayout>
                        <label class="text-center h2" :text="item.msg"></label>
                    </StackLayout>
                </v-template>
            </ListView>
        </StackLayout>
    </Page>
</template>

<script>

    // const view = require("tns-core-modules/ui/core/view");
    // import {ScrollView} from "tns-core-modules/ui/scroll-view";
    // import * as notif from "nativescript-local-notifications";

    export default {
        props: ['data'],
        methods:{
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

    .border{
        border-radius: 10;
        border-color: black;
        border-width: 1px;
    }

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
