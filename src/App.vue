<template>
  <el-row>
    <el-col :span="2">
      <!-- <div v-for="(c_item, index) in components" :key="index" draggable="true" @dragstart="c_item.ondragstart($event)">
        {{c_item.name}}
      </div> -->
      <div draggable="true" id = "button" @dragstart="buttonDragstart">
        button
      </div>
      <div draggable="true" id = "LED" @dragstart="ledDragstart">
        LED
      </div>
      <div draggable="true" id = "LED" @dragstart="ledDragstart">
        TEXTAREA
      </div>
    </el-col>
    
    <el-col :span="20" style="overflow-x:auto">
      <router-view />
    </el-col>

    <el-col :span="2">
      <div v-for="(t_item, index) in templates" :key="index">
        <router-link :to="t_item.path" @click="createDB()">{{t_item.name}}</router-link>
      </div>
    </el-col>
  </el-row>
</template>

<script>
// import { create } from 'domain';
// import { Button } from 'element-ui';

var db = openDatabase('BSHdb', '1.0', 'Test DB', 2 * 1024 * 1024);

db.transaction(function (tx) {  

   tx.executeSql('CREATE TABLE IF NOT EXISTS Temp (id unique,type,areaId,elementId)');

});
  export default {
    data() {
      return {
        components: [
          {
            name: "PS_BUTTON",
            ondragstart: (dragstartevent) => { this.buttonDragstart(dragstartevent) }
          },
          {
            name: "LED",
            ondragstart: () => { this.ledDragstart() }
          },
          {
            name: "TEXT",
            ondragstart: () => { this.textDragstart() }
          }
        ],
        elementCount:1,
        templates: [
          {
            name: "DEMO1",
            path: "/templateDemo1"
          },
          {
            name: "DEMO2",
            path: "/templateDemo2"
          }

        ]
      }
    },

    methods: {
      buttonDragstart() {
        let button = this.createBtn();
       // console.log(button);
       // console.log(JSON.stringify(button));
       // dragstartevent.dataTransfer.setData("element",JSON.stringify(button));
       sessionStorage.setItem("element", JSON.stringify(button));
      },
      ledDragstart() {
        let LED = this.createLED();
       // console.log(LED);
       // console.log(JSON.stringify(LED));
        // dragstartevent.dataTransfer.setData("element",JSON.stringify(LED));
        sessionStorage.setItem("element", JSON.stringify(LED));
      },
      textDragstart() {
        
      },
      createBtn(){
        var Btn = new Object;
        // Btn.id = this.elementCount;
        Btn.name = "";
        Btn.hwId = "";
        Btn.type = 1;
        // this.elementCount++;
        //type属性：1=按钮；2=LED灯
        return Btn;
      },
      createLED(){
        var LED = new Object;
        // LED.id = this.elementCount;
        LED.name = "";
        LED.hwId = "";
        LED.type = 2;
        // this.elementCount++;
        return LED;
      },
      createDB(index){
        db.transaction(function (tx) {  

        tx.executeSql('CREATE TABLE IF NOT EXISTS'+index+'(id unique, log)');

        });
      }
    }
  }
</script>

<style scoped>
  #button {
    width: 88px;
    height:88px;
    border:1px solid #aaaaaa;
}

#LED {
    width: 38px;
    height:38px;
    border:1px solid #aaaaaa;
}
</style>
