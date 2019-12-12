<template>
    <Page class="page">
        <ActionBar class="action-bar bg-purple">
            <Label class="action-bar-title" text="Second"></Label>
        </ActionBar>

        <StackLayout class="p-5">
            <button class="btn btn-primary btn-active btn-rounded-lg m-t-5" @tap="$navigateBack">Back</button>
            <label>Second Page</label>
            <StackLayout>
                <label class="text-center h1" :text="'Message ID: '+this.id"></label>
                <label class="text-center h3" :text="this.msg"></label>
            </StackLayout>
        </StackLayout>
    </Page>
</template>

<script>

    // const view = require("tns-core-modules/ui/core/view");
    // import {ScrollView} from "tns-core-modules/ui/scroll-view";
    // import * as notif from "nativescript-local-notifications";

    export default {
        props: ['id','msg'],
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
