<template>
    <Page class="page" id="page 1">
        <ActionBar class="action-bar">
            <Label class="action-bar-title" text="Home"></Label>
        </ActionBar>
        <TabView id="tabView1">
            <TabViewItem title="Tab 1">
                <ScrollView>
                    <StackLayout>
                        <Button class="btn btn-primary btn-active text-capitalize" @tap="openDialog" title="Alert Dialog" message="This is dialog using UI Dialog">
                            <Span text="Dialog" />
                        </Button>
                        <StackLayout class="input-field">
                            <TextField hint="firstname" id="firstname" class="input input-border" returnKeyType="go" v-model="name"/>
                        </StackLayout>

                        <Button class="btn btn-primary btn-active" @tap="makeFile" text="test"></Button>
                        
                        <Label class="h2 text-center bg-info" :text="this.name"/>
                        <Label id="lbl1" class="h2 text-center bg-info"/>
                        
                        <GridLayout rows="*,2*" columns="*,2*" class="text-center">
                            <TextField row="0" col="0" hint="username" />
                            <TextField row="1" col="0" hint="password" secure="true" />
                        </GridLayout>
                        
                        <Button class="btn btn-primary btn-ruby btn-active btn-rounded-lg text-lowercase" text="count click" @tap="objName"/>
                    </StackLayout>
                </ScrollView>
            </TabViewItem>

            <TabViewItem title="Tab 2">
                <ScrollView>
                    <StackLayout>
                        <Label class="h1 text-center" id="select" text="This is Label in Tab 2" />
                        <Image class="thumb img-circle img-thumbnail" id="profile-picture" width="150" height="150" src="" @tap="imgTap"></Image>
                        <Button class="btn btn-outline text-uppercase" text="Fetch Data" @tap="fetchData"/>
                        <ScrollView>
                                <ListView class="list-group" for="hero in heroes"
                                    @itemTap="onItemTap" style="height:1250px">
                                    <v-template>
                                        <FlexboxLayout flexDirection="row" class="list-group-item">
                                            <Image :src="hero.imageSrc" class="thumb img-circle" />
                                            <Label :text="hero.name" class="list-group-item-heading"
                                                style="width: 60%" />
                                        </FlexboxLayout>
                                    </v-template>
                                </ListView>
                        </ScrollView>
                    </StackLayout>
                </ScrollView>
            </TabViewItem>
        </TabView>
    </Page>
</template>

<script>
    const view = require("tns-core-modules/ui/core/view");
    const fileSystemModule = require("tns-core-modules/file-system");
    const dialogs = require("tns-core-modules/ui/dialogs");
    
    export default{
        data(){
            return{
                name:"Input Your Name Firstname",
                nilai:0,
                heroes: [
                    {
                        name: "Ryan Reynolds",
                        imageSrc: "data:image/jpeg;base64,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",
                    },
                    {
                        name: "Pickachu",
                        imageSrc: "data:image/jpeg;base64,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",
                    },
                    {
                        name: "Deadpool",
                        imageSrc: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSKbepC0GAFNUMSJ_o5Rbfv6_LRW4S151hLcotaCaFtj9a6hLDSTQ",
                    },
                ]
            };
        },
        methods:{
            openDialog(args) {
                dialogs.alert({
                    title: args.object.title,
                    message: args.object.message,
                    okButtonText: "Ok"
                }).then(function() {
                    this.name = "This dialog is closed";
                });
            },
            submitName(){
                this.name = "tester";
            },
            objName(args){
                let button = args.object;
                let parent = button.page;
                if (parent) {
                    let lbl = view.getViewById(parent,"lbl1");
                    if (lbl) {
                        if(this.nilai < 10){
                            this.nilai += 1;
                            lbl.text = "You tapped " + this.nilai + " times!";
                        }else{
                            lbl.text = "Enough Already!";
                        }
                    }
                }
            },
            makeFile(){
                let folder = fileSystemModule.knownFolders.currentApp();
                let path = fileSystemModule.path.join(folder.path, "images/logo.png");
                this.name = path;
            },
            imgTap(args){
                let img = args.object;
                img.borderRadius = "30"; 
                img.borderColor= "black"; 
                img.borderWidth= "1";
            },
            onItemTap(args){
                let item = args.object;
                let index = args.index;
                let page = item.page;
                if(page){
                    let lbl = view.getViewById(page,"select");
                    let img = view.getViewById(page,"profile-picture"); 
                    if(lbl){
                        lbl.text = this.heroes[index].name;
                        if(img){
                            img.src = this.heroes[index].imageSrc;
                        }
                    }
                }                
            },
            fetchData(args){
                let url = "https://jsonplaceholder.typicode.com/posts/1";
                let page = args.object.page;
                let lbl = view.getViewById(page,"select");

                fetch(url)
                .then((response) => response.json())
                .then((r) => {
                    lbl.text = r["title"];
                }).catch((e) => {
                });
            },
            onNavigationItemTap(component) {
                this.$navigateTo(component, {
                    clearHistory: true
                });
            },
        },
        computed: {
            message(args) {
                return "Blank {N}-Vue app";
            }
        }
    }
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
