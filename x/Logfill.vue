<template>
    <Page id="myPage" class="page bg-purple p-5" @loaded="onLoaded" actionBarHidden=true>
        <ScrollView>
            <StackLayout class="p-10">
                <StackLayout class="m-4 p-5" marginBottom="25" backgroundColor="white" borderRadius="10" borderColor="white" borderWidth="0.5">
                    <label id="user-lbl" class="h2 text-center"/>
                    <Button id="login-btn" visibility="collapsed" class="btn btn-primary btn-sky btn-active text-capitalize" borderRadius="10" text="Login First" horizontalAlignment="center" @tap="showLogin" />
                    <Button id="logout-btn" visibility="collapsed" class="btn btn-primary btn-ruby btn-active text-capitalize" borderRadius="10" text="Logout" horizontalAlignment="center" @tap="logout" />
                    <Button id="list-btn" visibility="collapsed" class="btn btn-primary btn-orange btn-active text-capitalize m-y-4" borderRadius="10" text="List" horizontalAlignment="center" @tap="showList" />
                </StackLayout>
                <Label class="h2 text-center light" marginBottom="25">Test</Label>
                <StackLayout id="result" class="" visibility="collapsed" marginBottom="25" borderRadius="10" borderWidth="0.5">
                    <label id="result-lbl" class="" text="" />
                </StackLayout>
                <FlexboxLayout horizontalAlignment="center">
                    <Button class="btn btn-primary btn-sky btn-active " marginRight="0" text="Automated Now" mode="0" @tap="successResult" />
                    <Button class="btn btn-primary btn-sky btn-active " marginLeft="0" text="Choose" @tap="errorResult" />
                </FlexboxLayout>
                <ListView visibility="collapsed" for="i in this.option">
                    <v-template>
                        <FlexboxLayout>
                            <Label :text="i" class="h2"></Label>
                        </FlexboxLayout>
                    </v-template>
                </ListView>
                <StackLayout class="hr-light m-10"></StackLayout>
                <StackLayout class="m-4 p-5" backgroundColor="white" borderRadius="10" borderColor="white" borderWidth="0.5">
                    <Label text="Clock-In" class="label font-weight-bold"/>
                    <TextField id="new-clockin" class="input" text="09.30" />
                    <Label text="Clock-Out" class="label font-weight-bold"/>
                    <TextField id="new-clockout" class="input" text="18.30" />
                </StackLayout>
                <StackLayout class="hr-light m-10"></StackLayout>
                <StackLayout class="m-4 p-4" backgroundColor="#F8F8F8" marginBottom="15" borderRadius="10" borderColor="white" borderWidth="0.5">
                    <label class="h2 text-center">
                        <FormattedString>
                            <Span text="Insert New Activity" class="text-primary" fontWeight="bold" />
                        </FormattedString>
                    </label>
                    <TextField id="new-activ" hint="Activity, max: 25 chars" maxLength="25" />
                    <TextView id="new-desc" hint="Description" marginBottom="25"></TextView>
                    <Button class="btn btn-primary btn-active text-capitalize" borderRadius="10" text="Insert" @tap="insertItem" />
                </StackLayout>
                <StackLayout class="m-4">
                    <ListView id="list-activ" class="list-group" for="(index,item) in arr.data" >
                        <v-template>
                            <StackLayout class="list-group-item active" backgroundColor="white" borderRadius="10" borderColor="white" borderWidth="0.5">
                                <TextField :text="item.activ" editable=false />
                                <TextView :text="item.desc" editable=false></TextView>					
                                <Button class= "btn btn-primary btn-ruby btn-active btn-sm text-capitalize" borderRadius="10" text="Delete" style="width:25%" />
                            </StackLayout>
                        </v-template>
                    </ListView>
                </StackLayout>
            </StackLayout>
        </ScrollView>
    </Page>
</template>

<script>
    import ListActivity from "./ListActivity";
    import * as fs from "tns-core-modules/file-system";
    const setting = require('application-settings');
    const view = require("tns-core-modules/ui/core/view");
    const dialogs = require("tns-core-modules/ui/dialogs");
    
    export default {

        data(){
            return{
                user:"",
                pass:"",
                option: ["Automate Now","Nah, Im Off"],
                arr:{
                    data:[
                        {
                            activ:"1",
                            desc:"Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum",
                        },
                        {
                            activ:"2",
                            desc:"Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum",
                        },
                        {
                            activ:"3",
                            desc:"Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum",
                        },
                    ],
                },
            }
        },
        methods:{
            onLoaded(args){
                let page = args.object;
                let loginBtn = view.getViewById(page,'login-btn');
                let listBtn = view.getViewById(page,'list-btn');
                let userLbl = view.getViewById(page,'user-lbl');

                console.log("page loaded");

                setting.clear();
                this.user = setting.getString("user");
                this.pass = setting.getString("pass");
                
                // Hide button
                if(this.user == undefined || this.pass == undefined){
                    userLbl.text = "Login First";
                    loginBtn.visibility = "";
                    listBtn.visibility = "collapsed";
                }else{
                    userLbl.text = "Welcome, ",this.user;
                    listBtn.visibility = "";
                    loginBtn.visibility = "collapsed";
                }
            },
            logout(){
                setting.clear();
                this.$navigateTo(this);
            },
            showLogin(args){
                let btn = args.object;
                let page = btn.page;
                let listBtn = view.getViewById(page,'list-btn');
                let logoutBtn = view.getViewById(page,'logout-btn');

                // Show login dialog
                dialogs.login({
                    title: "Login",
                    message: "Please enter your NIM and Password, All data will be saved in local storage",
                    okButtonText: "Submit",
                    cancelButtonText: "Cancel",
                    userName: "",
                    password: "",
                }).then(function (r) {
                    console.log(r.result);
                    if(r.result == true){
                        console.log(r.userName," ",r.password);
                        // setting.setString("user",r.userName);
                        // setting.setString("pass",r.password);
                        btn.visibility = "collapsed";
                        listBtn.visibility = "";
                        logoutBtn.visibility = "";
                    }                        
                });
            },
            showList(){
                this.$navigateTo(ListActivity);                
            },
            successResult(args){
                let btn = args.object;
                let page = btn.page;
                let result = view.getViewById(page,"result-lbl");
                let parent = result.parent;
                
                parent.visibility = "collapsed";

                result.text = "Success";
                result.className = "text-center h2 m-4";
                parent.className = "p-4";
                parent.backgroundColor = "#d4edda";
                parent.visibility = "";

            },
            errorResult(args){
                let btn = args.object;
                let page = btn.page;
                let result = view.getViewById(page,"result-lbl");
                let parent = result.parent;

                parent.visibility = "collapsed";

                result.text = "Error";
                result.className = "text-center h2 m-4";
                parent.className = "p-4";
                parent.backgroundColor = "#f8d7da";
                parent.visibility = "";
            },
            insertItem: function(args){
				let btn = args.object;
				let page = btn.page;
				let list = view.getViewById(page,"list-activ");
				let activ = view.getViewById(page,'new-activ');
				let desc = view.getViewById(page,'new-desc');

				let currentAppFolder = fs.knownFolders.currentApp();
                let path = fs.path.normalize(currentAppFolder.path+"/mydata.json");
                let file = fs.File.fromPath(path);
               
				let obj = {
					"activ":activ.text,
					"desc":desc.text,
				}

				activ.text = "";
				desc.text = "";
				this.arr["data"].unshift(obj);

				file.writeText(JSON.stringify(this.arr))
				.then(res => {
					console.log("file made");
				})
				.catch(err => {
					console.log(err);
				});					

				list.refresh();
			},
			removeItem(index){
				this.arr.data.splice(index,1);

				let currentAppFolder = fs.knownFolders.currentApp();
                let path = fs.path.normalize(currentAppFolder.path+"/mydata.json");
                let file = fs.File.fromPath(path);

				file.writeText(JSON.stringify(this.arr))
				.then(res => {
					console.log("file made");
				})
				.catch(err => {
					console.log(err);
				});	
			},
        },
        computed: {
            message() {
                return global.username;
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
        background-color:#8A2BE2;
    }

    .light{
        color:#F8F8F8;
    }
</style>
