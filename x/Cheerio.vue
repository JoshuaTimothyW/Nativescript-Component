<template>
    <Page class="page" @loaded="method1">
        <ActionBar class="action-bar">
            <Label class="action-bar-title" text="Home"></Label>
        </ActionBar>
        <StackLayout class="m-4">
            <label id="result" class="text-center h1" text=""></label>
            <button class="btn btn-primary btn-active btn-sky" @tap="method1">Test</button>
        </StackLayout>
    </Page>
</template>

<script>
    // require("nativescript-nodeisfy");
    // const setting = require('application-settings');
    // const fs = require("tns-core-modules/file-system");
    const http = require("tns-core-modules/http");
    const view = require("tns-core-modules/ui/core/view");
    const cheerio = require("cheerio-without-node-native");


    export default {
        data(){
            return{

            }
        },
        methods:{
            method1(args){
                
                let btn = args.object;
                let page = btn.page;
                let lbl = view.getViewById(page,"result");

                btn.isEnabled = false;

                // let html = `
                //     <ul id="fruits">
                //     <li class="green apple">Green Apple</li>
                //     <li class="apple">Apple</li>
                //     <li class="apple" type="juicy">Fruty Apple</li>
                //     <li class="orange">Orange</li>
                //     <li class="orange" type="juicy">Fruty Orange</li>
                //     <li class="pear">Pear</li>
                //     <li class="pear" type="juicy">Fruty Pear</li>
                //     </ul>
                    
                //     <ul id="veggie">
                //     <li class="green veggie">Green Veggie</li>
                //     <li class="brocoli">Brocoli</li>
                //     <li class="tomato" type="veggie">Tomato</li>
                //     </ul>
                // `

                let URL = "https://www.marvelsynergy.com/champions";

                http.getString(URL).then(function(html){
                    let $ = cheerio.load(html);
                    let list_url = $("div[class='label']").find("a[itemprop='url']");
                    let champions = [];

                    // extract each link and text, save in array
                    list_url.each(function(i,e){
                        champions[i] = {
                            "name":$(this).text(),
                            "url":$(this).attr("href"),
                        }
                    })

                    let choose = Math.floor(Math.random()*champions.length);

                    lbl.text = champions[choose]["name"];
                    btn.isEnabled = true;
                })

            },
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
</style>
