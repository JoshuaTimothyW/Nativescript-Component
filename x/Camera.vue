<template>
    <Page class="page bg-purple">
        <ActionBar class="action-bar">
            <Label class="action-bar-title" text="Home"></Label>
        </ActionBar>
             <ScrollView>
                <StackLayout class="p-4">
                        <Image id="img-result" marginTop="25" width="300" height="300"></Image>
                        <label id="path1" class="white"></label>
                        <label id="path2" class="white"></label>
                        <label id="path3" class="white"></label>
                        <button class="btn btn-primary btn-sky btn-active" @tap="openCamera">Open Camera</button>            
                        <button class="btn btn-primary btn-sky btn-active" @tap="printPath">Print Path</button>
                        <button class="btn btn-primary btn-sky btn-active" @tap="createFile">Create File</button>

                </StackLayout>
            </ScrollView>       
    </Page>
</template>

<script>

    const child = require('child-process-promise');
    const camera = require('nativescript-camera');
    // const setting = require('application-settings');
    const fs = require("tns-core-modules/file-system");
    const view = require("tns-core-modules/ui/core/view");

    export default {
        data(){
            return{

            }
        },
        methods:{
            openCamera(args){
                let btn = args.object;
                let page = btn.page;
                let img = view.getViewById(page,"img-result");

                camera.requestPermissions().then(
                    function(success){
                        let options = { width: 300, height: 300, keepAspectRatio: false, saveToGallery: false };
                        camera.takePicture(options)
                        .then(
                            function(imageAsset){
                                img.src = imageAsset;
                            }
                        )
                        .catch(
                            function(){
                                img.src = "";
                            }
                        );
                    }
                );
            },
            openGallery(args){
                let btn = args.object;
                let page = btn.page;
                let img = view.getViewById(page,"img-result");

                camera.requestPhotosPermissions()
                .then(
                    function(imageAsset){
                        img.src = imageAsset;
                    }
                )
                .catch(
                    function(){
                        img.src = "";
                    }
                );
            },
            printPath(args){
                let btn = args.object;
                let page = btn.page;
                let lbl1 = view.getViewById(page,"path1");
                let lbl2 = view.getViewById(page,"path2");
                let lbl3 = view.getViewById(page,"path3");

                lbl1.text = fs.knownFolders.documents().path;
                lbl2.text = fs.knownFolders.currentApp().path;
                lbl3.text = fs.knownFolders.temp().path;
            },
            createFile(){
                let path = "/storage/emulated/0/DCIM/";
                // let path = "/storage/emulated/0/Android/data/com.logfill.app/files/";
                let file = fs.File.fromPath(path+"wow.json");
                // console.log(file.path);

                let obj = {
                    data:[
                        {
                            "activ":"1",
                            "desc":"1",
                        },
                        {
                            "activ":"2",
                            "desc":"2",
                        },
                        {
                            "activ":"3",
                            "desc":"3",
                        },
                        {
                            "activ":"Code with vue",
                            "desc":"Code vue today and learning about vue program",
                        },
                    ],
                };


                file.writeText(JSON.stringify(obj))
                    .then(
                        function(result){
                            file.readText().then(
                                function(res){
                                    console.log(res);
                                }
                            );
                        }
                    )
                    .catch(
                        function(error){
                            console.log(error);
                        }
                );

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

    .bg-purple{
        background-color: darkviolet;
    }

    .white{
        color: white;
    }

    .btn-float{
        background-color: #30bcff;
        text-align: center;
        vertical-align: middle;
        height: 56;
        width: 56;
        border-radius: 30;
    }

    .btn-float-container{
        margin-top: 80%;
        margin-left: 80%;
    }

</style>
