<template>
    <Page class="page" id="page 1">
        <ActionBar class="c-bg-sky">
            <Label id="title-page" class="action-bar-title h3 text-primary" text="Mcoc"></Label>
            <!-- <NavigationButton text="Go back" android.systemIcon="ic_menu_back" @tap="$navigateBack" /> -->
        </ActionBar>
        <TabView id="tabView1">
            <TabViewItem title="Synergy">
                <StackLayout>
                    <TextView id="txt-search" hint="Search champion name..." @textChange="autoComplete"/>
                    
                    <ListView id="autocomplete" for="champ in this.auto_list" @itemTap="selectAuto">
                        <v-template>
                            <FlexboxLayout>
                                <Label :text="champ" class="h2"></Label>
                            </FlexboxLayout>
                        </v-template>
                    </ListView>
                    <StackLayout class="hr-light m-3"></StackLayout>

                    <Button id="btn-search" class="btn btn-primary btn-sky btn-active btn-rounded-lg" @tap="search" text="search" />

                    <Label id="champ-name" class="h2 text-center" v-model="name"></Label>
                    <!-- <Switch @checkedChange="check" id="my-switch"/> -->
                    <StackLayout class="hr-light m-10"></StackLayout>
                    <ListView id="list_champ" for="synergy in synergy_list" @itemTap="showDialogSynergy">
                        <v-template>
                            <StackLayout flexDirection="row" class="list-group-item">
                                <Label class="list-group-item-heading h2" :text="synergy.with" />
                                <Label class="list-group-item-text" :text="synergy.desc"></Label>
                            </StackLayout>
                        </v-template>
                    </ListView>
                </StackLayout>
            </TabViewItem>
            <TabViewItem title="Log-Fill">
                <StackLayout class="p-10">
                    <Label class="h2 text-center" text="Auto Log-Fill" />
                    <Button id="btn-auto-fill" class="btn btn-primary btn-sky btn-active btn-rounded-lg" @tap="auto_fill" text="Insert Logbook" />
                    <Label id="status-text" class="h2 text-center" />
                    <StackLayout class="hr-light m-10"></StackLayout>
                    <StackLayout>
                        <TextField id="clock-in" hint="Clock-in" text="" />
                        <TextField id="clock-out" hint="Clock-out" text="" />
                        <TextField id="clock-activity" hint="Activity" text="" />
                        <TextView id="clock-desc" hint="Description" text="" />
                    </StackLayout>
                </StackLayout>
            </TabViewItem>
        </TabView>
    </Page>
</template>

<script>
    
    const JSSoup = require('jssoup').default;
    const httpModule = require("tns-core-modules/http");
    const view = require("tns-core-modules/ui/core/view");
    const dialogs = require("tns-core-modules/ui/dialogs");
    const appSettings = require("application-settings");
    
    export default{
        data(){
            return{
                auto_list: [],
                synergy_list: [],
                selected : "",
                user: "",
                pass: "",
            }
        },
        mounted(){
        //   this.user = appSettings.getString("username",""); 
        //   this.pass = appSettings.getString("pass","");
        },
        methods:{
            auto_fill(args){
                let btn = args.object;
                let page = btn.page;
                let status = view.getViewById(page,'status-text');

                if(this.user == "" || this.pass == ""){
                    // User name and password arguments are optional.
                    dialogs.login("You havent login yet", "User name label text", "Password label text").then(function (r) {
                        // console.log("Dialog result: " + r.result + ", user: " + r.userName + ", pwd: " + r.password);
                        if(r.result){
                            appSettings.setString("username",r.userName);
                            appSettings.setString("pass",r.password);
                            this.user = r.userName;
                            this.pass = r.password;
                        }
                    });
                }else{
                    status.text = this.getRandomInt(3);
                }
            },
            logout(){
                appSettings.clear();
                this.user = appSettings.getString("username","");
                this.pass = appSettings.getString("password","");

                dialogs.alert({
                    title: "Logout",
                    message: "Logout Success",
                    okButtonText: "Ok"
                }).then(function() {
                });
            },
            getRandomInt(max){
                return Math.floor(Math.random() * Math.floor(max));
            },
            showModal() {
                this.$showModal(ModalView, {
                    fullscreen: false
                });
            },
            showDialogSynergy(args){
                let list = args.object;

                dialogs.alert({
                    title: args.item.star+" - "+args.item.name,
                    message: ""+args.item.note,
                    okButtonText: "Close"
                }).then(function() {
                });
            },
            autoComplete(args){
                let txt_input = args.object;
                let page = txt_input.page;
                let list = view.getViewById(page,"autocomplete");
                let lbl = view.getViewById(page,"champ-name");
                
                // empty the auto list
                let val = txt_input.text;
                this.auto_list = [];
                
                // copy value from champion list
                let champ_list = ['Abomination', 'Ant-Man', 'Captain America', 'Captain America (Infinity War)', 'Captain America WWII', 'Electro', 'Hulk', 'Hulk (Ragnarok)', 'Human Torch', 'Invisible Woman', 'Joe Fixit', 'Luke Cage', 'M.O.D.O.K.', 'Quake', 'Red Hulk', 'Rhino', 'Sentry', 'She-Hulk', 'Spider-Gwen', 'Spider-Man (Classic)', 'Spider-Man (Miles Morales)', 'Thing', 'Void', 'Wasp', 'Yellowjacket', 'Diablo', 'Doctor Strange', 'Doctor Voodoo', 'Dormammu', 'Ebony Maw', 'Ghost Rider', 'Guillotine', 'Iron Fist', 'Iron Fist (Immortal)', 'Juggernaut', 'Loki', 'Magik', 'Mephisto', 'Mordo', 'Morningstar', 'Scarlet Witch', 'Symbiote Supreme', 'The Hood', 'Thor(Jane Foster)', 'Unstoppable Colossus', 'Angela', 'Annihilus', 'Black Bolt', 'Captain Marvel', 'Captain Marvel (Classic)', 'Carnage', 'Corvus Glaive', 'Cull Obsidian', 'Drax', 'Gamora', 'Groot', 'Heimdall', 'Hela', 'Hyperion', 'King Groot', 'Medusa', 'Ms. Marvel', 'Ms. Marvel (Kamala Khan)', 'Phoenix', 'Proxima Midnight', 'Ronan', 'Spider-Man (Symbiote)', 'Superior Iron Man', 'Thanos', 'The Champion', 'Thor', 'Venom', 'Venom the Duck', 'Venompool', 'Civil Warrior', 'Darkhawk', 'Doctor Octopus', 'Ghost', 'Green Goblin', 'Howard the Duck', 'Hulkbuster', 'Iron Man', 'Iron Man (Infinity War)', 'Iron Patriot', 'Kang', 'Nebula', 'Punisher 2099', 'Red Skull', 'Rocket Raccoon', 'Sentinel', 'Spider-Man (Stark Enhanced)', 'Star-Lord', 'Ultron', 'Ultron (Classic)', 'Vision', 'Vision (Age of Ultron)', 'Vulture', 'War Machine', 'Yondu','Archangel', 'Beast', 'Bishop', 'Cable', 'Colossus', 'Cyclops (Blue Team)', 'Cyclops (New Xavier School)', 'Deadpool', 'Deadpool (X-Force)', 'Domino', 'Emma Frost', 'Gambit', 'Goldpool', 'Havok', 'Iceman', 'Magneto', 'Magneto (Marvel Now)', 'Mister Sinister', 'Namor', 'Nightcrawler', 'Old Man Logan', 'Omega Red', 'Psylocke', 'Rogue', 'Sabretooth', 'Storm', 'Wolverine', 'Wolverine X-23', 'Aegon', 'Agent Venom', 'Black Panther', 'Black Panther (Civil War)', 'Black Widow', 'Blade', 'Crossbones', 'Daredevil', 'Daredevil (Classic)', 'Elektra', 'Falcon', 'Gwenpool', 'Hawkeye', 'Karnak', 'Killmonger', 'Kingpin', 'Korg', 'Masacre', 'Moon Knight', 'Nick Fury', 'Night Thrasher', 'Punisher', 'Ronin', 'Taskmaster', 'Thor (Ragnarok)', 'Winter Soldier'];
                
                if(val.length != 0){
                    let count = 0;
                    for(let i=0 ; i<champ_list.length ; i++){
                        if(count<5 && champ_list[i].substr(0, val.length).toUpperCase() == val.toUpperCase()){
                            this.auto_list.push(champ_list[i]);
                            count++;
                        }
                    }
                }                

                list.refresh();
            },
            selectAuto(args){
                let item_list = args.object;
                let page = item_list.page;
                let txt_input = view.getViewById(page,"txt-search");
                let btn = view.getViewById(page,"btn-search");
                
                //return index of champion list
                let champ_list = ['Abomination', 'Ant-Man', 'Captain America', 'Captain America (Infinity War)', 'Captain America WWII', 'Electro', 'Hulk', 'Hulk (Ragnarok)', 'Human Torch', 'Invisible Woman', 'Joe Fixit', 'Luke Cage', 'M.O.D.O.K.', 'Quake', 'Red Hulk', 'Rhino', 'Sentry', 'She-Hulk', 'Spider-Gwen', 'Spider-Man (Classic)', 'Spider-Man (Miles Morales)', 'Thing', 'Void', 'Wasp', 'Yellowjacket', 'Diablo', 'Doctor Strange', 'Doctor Voodoo', 'Dormammu', 'Ebony Maw', 'Ghost Rider', 'Guillotine', 'Iron Fist', 'Iron Fist (Immortal)', 'Juggernaut', 'Loki', 'Magik', 'Mephisto', 'Mordo', 'Morningstar', 'Scarlet Witch', 'Symbiote Supreme', 'The Hood', 'Thor(Jane Foster)', 'Unstoppable Colossus', 'Angela', 'Annihilus', 'Black Bolt', 'Captain Marvel', 'Captain Marvel (Classic)', 'Carnage', 'Corvus Glaive', 'Cull Obsidian', 'Drax', 'Gamora', 'Groot', 'Heimdall', 'Hela', 'Hyperion', 'King Groot', 'Medusa', 'Ms. Marvel', 'Ms. Marvel (Kamala Khan)', 'Phoenix', 'Proxima Midnight', 'Ronan', 'Spider-Man (Symbiote)', 'Superior Iron Man', 'Thanos', 'The Champion', 'Thor', 'Venom', 'Venom the Duck', 'Venompool', 'Civil Warrior', 'Darkhawk', 'Doctor Octopus', 'Ghost', 'Green Goblin', 'Howard the Duck', 'Hulkbuster', 'Iron Man', 'Iron Man (Infinity War)', 'Iron Patriot', 'Kang', 'Nebula', 'Punisher 2099', 'Red Skull', 'Rocket Raccoon', 'Sentinel', 'Spider-Man (Stark Enhanced)', 'Star-Lord', 'Ultron', 'Ultron (Classic)', 'Vision', 'Vision (Age of Ultron)', 'Vulture', 'War Machine', 'Yondu','Archangel', 'Beast', 'Bishop', 'Cable', 'Colossus', 'Cyclops (Blue Team)', 'Cyclops (New Xavier School)', 'Deadpool', 'Deadpool (X-Force)', 'Domino', 'Emma Frost', 'Gambit', 'Goldpool', 'Havok', 'Iceman', 'Magneto', 'Magneto (Marvel Now)', 'Mister Sinister', 'Namor', 'Nightcrawler', 'Old Man Logan', 'Omega Red', 'Psylocke', 'Rogue', 'Sabretooth', 'Storm', 'Wolverine', 'Wolverine X-23', 'Aegon', 'Agent Venom', 'Black Panther', 'Black Panther (Civil War)', 'Black Widow', 'Blade', 'Crossbones', 'Daredevil', 'Daredevil (Classic)', 'Elektra', 'Falcon', 'Gwenpool', 'Hawkeye', 'Karnak', 'Killmonger', 'Kingpin', 'Korg', 'Masacre', 'Moon Knight', 'Nick Fury', 'Night Thrasher', 'Punisher', 'Ronin', 'Taskmaster', 'Thor (Ragnarok)', 'Winter Soldier'];
                let url_list = ['/champions/abomination', '/champions/ant-man', '/champions/captain-america', '/champions/captain-america-infinity-war', '/champions/captain-america-wwii', '/champions/electro', '/champions/hulk', '/champions/hulk-ragnarok', '/champions/human-torch', '/champions/invisible-woman', '/champions/joe-fixit', '/champions/luke-cage', '/champions/modok', '/champions/quake', '/champions/red-hulk', '/champions/rhino', '/champions/sentry', '/champions/she-hulk', '/champions/spider-gwen', '/champions/spider-man_classic', '/champions/spider-man-miles-morales', '/champions/thing', '/champions/void', '/champions/wasp', '/champions/yellowjacket', '/champions/diablo', '/champions/doctor-strange', '/champions/doctor-voodoo', '/champions/dormammu', '/champions/ebony-maw', '/champions/ghost-rider', '/champions/guillotine', '/champions/iron-fist', '/champions/immortal-iron-fist', '/champions/juggernaut', '/champions/loki', '/champions/magik', '/champions/mephisto', '/champions/mordo', '/champions/morningstar', '/champions/scarlet-witch', '/champions/symbiote-supreme', '/champions/the-hood', '/champions/thor-jane-foster', '/champions/unstoppable-colossus', '/champions/angela', '/champions/annihilus', '/champions/black-bolt', '/champions/captain-marvel-movie', '/champions/captain-marvel', '/champions/carnage', '/champions/corvus-glaive', '/champions/cull-obsidian', '/champions/drax', '/champions/gamora', '/champions/groot', '/champions/heimdall', '/champions/hela', '/champions/hyperion', '/champions/king-groot', '/champions/medusa', '/champions/ms-marvel', '/champions/ms-marvel-kamala-khan', '/champions/phoenix', '/champions/proxima-midnight', '/champions/ronan', '/champions/spider-man_symbiote', '/champions/superior-iron-man', '/champions/thanos', '/champions/the-champion', '/champions/thor', '/champions/venom', '/champions/venom-the-duck', '/champions/venompool', '/champions/civil_warrior', '/champions/darkhawk', '/champions/doctor-octopus', '/champions/ghost', '/champions/green-goblin', '/champions/howard_the_duck', '/champions/hulkbuster', '/champions/iron-man', '/champions/iron-man-infinity-war', '/champions/iron-patriot', '/champions/kang', '/champions/nebula', '/champions/punisher-2099', '/champions/red-skull', '/champions/rocket-raccoon', '/champions/sentinel', '/champions/spider-man-stark-enhanced', '/champions/star-lord', '/champions/ultron', '/champions/ultron_classic', '/champions/vision', '/champions/vision_age_of_ultron', '/champions/vulture', '/champions/war-machine', '/champions/yondu', '/champions/archangel', '/champions/beast', '/champions/bishop', '/champions/cable', '/champions/colossus', '/champions/cyclops_blue_team', '/champions/cyclops', '/champions/deadpool', '/champions/deadpool-x-force', '/champions/domino', '/champions/emma-frost', '/champions/gambit', '/champions/goldpool', '/champions/havok', '/champions/iceman', '/champions/magneto', '/champions/magneto-marvel-now', '/champions/mister-sinister', '/champions/namor', '/champions/nightcrawler', '/champions/old-man-logan', '/champions/omega-red', '/champions/psylocke', '/champions/rogue', '/champions/sabretooth', '/champions/storm', '/champions/wolverine', '/champions/wolverine-x-23', '/champions/aegon', '/champions/agent-venom', '/champions/black-panther', '/champions/black_panther_civil_war', '/champions/black-widow', '/champions/blade', '/champions/crossbones', '/champions/daredevil-netflix', '/champions/daredevil', '/champions/elektra', '/champions/falcon', '/champions/gwenpool', '/champions/hawkeye', '/champions/karnak', '/champions/killmonger', '/champions/kingpin', '/champions/korg', '/champions/masacre', '/champions/moon-knight', '/champions/nick-fury', '/champions/night-thrasher', '/champions/punisher', '/champions/ronin', '/champions/taskmaster', '/champions/thor_ragnarok', '/champions/winter-soldier'];
                
                this.selected = url_list[champ_list.indexOf(args.item)];
                txt_input.text = args.item;
            },
            search: function(args){  
                let btn = args.object;
                let page = btn.page;
                let txt_input = view.getViewById(page,"txt-search");                
                let lbl = view.getViewById(page,"champ-name");
                let list = view.getViewById(page,"list_champ");

                // If text empty
                if(txt_input.text.length < 1){
                    lbl.text = "Page Not Found";
                    this.synergy_list = [];
                    list.refresh();
                    return;
                }

                // Put in url
                let url = "https://www.marvelsynergy.com"+this.selected;
               
                // Disable the button after clicked
                btn.isEnabled = false;
                btn.text = "Working on it...";

                // Clear Text
                txt_input.text = "";
                
                // request and get body response as html
                httpModule.getString(url).then((r) => {
                    // Use JSSoup
                    let content = new JSSoup(r);
                    
                    // Default Error Message
                    let errorMsg = "404: Page not found | MarvelSynergy.com";
                    
                    if(content.find('title').string == errorMsg){
                        lbl.text = "Page Not Found";

                        // Back to normal button state
                        btn.isEnabled = true;
                        btn.text = "Search";
                    }else{
                        // Selected Name
                        this.name = content.find("option",{"selected":"selected"}).text;

                        // Synergy
                        let champ_synergy = content.findAll("section",{"itemprop":"gameTip"});

                        let arr_synergy = [];

                        // Looping Synergy
                        this.champ_list = champ_synergy.forEach(function(synergy){
                            // Name Synergy
                            let name = synergy.find("h2",{"itemprop":"name"}).text;

                            // Synergy With : td[0] , Min Star : td[1] , Desc : td[2]
                            let table_list = synergy.find('table','f11');
                            let row_list = table_list.findAll('tr');
                            let td = row_list[1].findAll('td');

                            // Notes
                            let desc = synergy.find("p",{"itemprop":"description"}).text;

                            // New Object
                            let obj = {
                                "with":name,
                                "star":td[0].text,
                                "name":td[1].text,
                                "desc":td[2].text,
                                "note":desc,
                            }

                            arr_synergy.push(obj);
                        });
                            
                        this.synergy_list = arr_synergy;
                        list.refresh();

                        // Back to normal button state
                        btn.isEnabled = true;
                        btn.text = "Search";
                    }  
                },(e) => {
                    // Failed to connect
                    lbl.text = "Offline";

                    // Back to normal button state
                    btn.isEnabled = true;
                    btn.text = "Search";
                });
            },
            testSoup(args){

                const data = `
                <html><head><title>The Dormouse's story</title></head>
                <body>
                <p class="title"><b>The Dormouse's story</b></p>
                <p class="story">Once upon a time there were three little sisters; and their names were
                <a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
                <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
                <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
                and they lived at the bottom of a well.</p>
                <p class="story">...</p>
                <span class="one">One</span>
                <span class="two">Two</span>
                <span class="three">Three</span>
                <span class="one two three">One Two Three</span>
                <div class=" whitespace">Whitespace Left</div>
                <div class="whitespace ">Whitespace Right</div>
                <div class=" whitespace ">Whitespace Left and Right</div>
                <div class="    so    much    whitespace    ">Whitespace</div>
                </body>
                </html>
                `;
                
                const data2 = `
                    <p>
                        <span contenteditable="false">blabla bla bla</span>
                        <strong style="color: rgb(203, 65, 64);"> </strong> here a double space bla bla
                    </p>
                `

                let btn = args.object;
                let page = btn.page;
                let txt_input = view.getViewById(page,"txt-search");

                let soup = new JSSoup(data2);
                let p = soup.find('p');
                txt_input.text = p.text;
            },
        },
    };
</script>