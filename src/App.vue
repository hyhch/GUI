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

var elementCount = 1;
//元素唯一id
var db = openDatabase('BSHdb', '1.0', 'Test DB', 2 * 1024 * 1024);
db.transaction(function (context) {  

  //  tx.executeSql('CREATE TABLE IF NOT EXISTS Temp ("id" INTEGER NOT NULL,"type" INTEGER,"areaId" INTEGER,"elementId" INTEGER,PRIMARY KEY ("id"))');
   context.executeSql('CREATE TABLE IF NOT EXISTS TemplateList ("id" INTEGER NOT NULL,"name" TEXT,PRIMARY KEY ("id"))');
   context.executeSql('CREATE TABLE IF NOT EXISTS Button ("id" INTEGER NOT NULL,"name" TEXT,"hwid" TEXT,"areaId" TEXT,"templateId" INTEGER,PRIMARY KEY ("id"))');
   context.executeSql('CREATE TABLE IF NOT EXISTS LED ("id" INTEGER NOT NULL,"name" TEXT,"hwid" TEXT,"areaId" TEXT,"templateId" INTEGER,PRIMARY KEY ("id"))');
   context.executeSql('CREATE TABLE IF NOT EXISTS ButtonMapping ("id" INTEGER NOT NULL,"name" TEXT,"unitname" TEXT,"buttonMode" TEXT,"templateId" INTEGER,"buttonId" INTEGER,PRIMARY KEY ("id"))');
   //创建表
  //  context.executeSql('INSERT INTO Button (id, name) VALUES (1, "aaa")');
  //  context.executeSql('INSERT INTO Button (id, name) VALUES (2, "aaa")');
  //  context.executeSql('INSERT INTO Button (id, name) VALUES (3, "aaa")');
  //  context.executeSql('INSERT INTO LED (id, name) VALUES (1, "aaa")');
  //  context.executeSql('INSERT INTO LED (id, name) VALUES (2, "aaa")');
  context.executeSql('SELECT MAX(MAX(bt.id),MAX(led.id)) AS max_result FROM Button as bt,LED as led', [], function (context, results) {
            elementCount = results.rows.item(0).max_result+1;
            console.log(elementCount);
          });
  //取所有表中的id最大值+1作为elementCount值
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
        Btn.id = elementCount;
        Btn.name = "";
        Btn.hwId = "";
        Btn.type = 1;
        elementCount++;
        //type属性：1=按钮；2=LED灯
        return Btn;
      },
      createLED(){
        var LED = new Object;
        LED.id = elementCount;
        LED.name = "";
        LED.hwId = "";
        LED.type = 2;
        elementCount++;
        return LED;
      },
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
