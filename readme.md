# A simple side navigation drawer component for Vue.JS

Use the Sidenav tag in your HTML part and set the v-model attribute.

HTML:
```
<Sidenav v-model="sidenavStatus">
```

Javascript:
```
import Sidenav from './Sidenav.vue';
var vue = new Vue({
    el:"#app",
    components:{Sidenav},
    data:{
        sidenavStatus:false,
    }
})
```
Set the "sidenavStatus" to true/false to force open/close the drawer

Licence :
[MIT](https://en.wikipedia.org/wiki/MIT_License)

