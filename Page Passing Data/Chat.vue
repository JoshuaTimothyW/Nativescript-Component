<template>
    <Page class="page">
        <ActionBar class="action-bar bg-purple">
            <NavigationButton text="Go back" android.systemIcon="ic_menu_back" @tap="$navigateBack" />
            <StackLayout orientation="horizontal">
                <Image src="~/img/icon.jpg" width="50" height="50" class="thumb img-circle"></Image>
                <Label class="action-bar-title" :text="this.name"></Label>
                <ActionItem  v-show="isEditing"
                ios.systemIcon="1"
                android.systemIcon="ic_menu_close_clear_cancel" />
            </StackLayout>
            <!-- <Label class="action-bar-title" :text="this.name"></Label> -->
        </ActionBar>
        <StackLayout>
            <StackLayout id="chatScreen" height="85%" class="p-5">
                <Label class="footnote m-b-10 text-center" text="Today, 3:23 PM"></Label>
                <Label class="bg-primary img-circle pull-left h3 text-white p-10 m-y-2" text="Hey, Names Homer"></Label>
                <Label class="bg-primary img-circle pull-left h3 text-white p-10 m-y-2" text="Need donut, I'm Starved here"></Label>
                <Label class="bg-green img-circle pull-right h3 text-white p-10 m-y-2" text="Nope"></Label>
            </StackLayout>
            <StackLayout id="chatBox" height="15%" class="bg-gray" orientation="horizontal">
                <GridLayout columns="*,auto" style="padding: 10">
                    <TextField #textfield class="bg-white" row="0" col="0" @tap=""></TextField>
                    <Button #btn class="btn btn-primary" row="0" col="1" text="Send"></Button>
               </GridLayout>
            </StackLayout>
        </StackLayout>
    </Page>
</template>

<script>

    // const view = require("tns-core-modules/ui/core/view");
    // import {ScrollView} from "tns-core-modules/ui/scroll-view";
    // import * as notif from "nativescript-local-notifications";

    export default {
        props: ['name'],
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

    .bg-gray{
        background-color:#e7dddd;
    }

    .bg-white{
        background-color: #ffffff; 
    }

    .border{
        border-radius: 10;
        border-color: black;
        border-width: 1px;
    }

    .text-white{
        color: #ffffff; 
    }

    .bg-green{
        background-color: #2cc96b;
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
