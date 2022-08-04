<template>
  <el-row>
    
    
    <el-col :span="22" style="overflow-x:auto">
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
  //元素唯一id
  var db = openDatabase('BSHdb', '1.0', 'Test DB', 2 * 1024 * 1024);
  db.transaction(function (context) {  

    //  tx.executeSql('CREATE TABLE IF NOT EXISTS Temp ("id" INTEGER NOT NULL,"type" INTEGER,"areaId" INTEGER,"elementId" INTEGER,PRIMARY KEY ("id"))');
   context.executeSql('CREATE TABLE IF NOT EXISTS TemplateList ("id" INTEGER NOT NULL,"name" TEXT,PRIMARY KEY ("id"))');
   context.executeSql('CREATE TABLE IF NOT EXISTS Button ("id" INTEGER NOT NULL,"name" TEXT,"hwId" TEXT,"areaId" TEXT,"templateId" INTEGER,"boardType" INTEGER,PRIMARY KEY ("id"))');
   context.executeSql('CREATE TABLE IF NOT EXISTS LED ("id" INTEGER NOT NULL,"name" TEXT,"hwId" TEXT,"areaId" TEXT,"segmentId" INTEGER,"templateId" INTEGER,"boardType" INTEGER,PRIMARY KEY ("id"))');
   context.executeSql('CREATE TABLE IF NOT EXISTS ButtonMapping ("id" INTEGER NOT NULL,"name" TEXT,"unitname" TEXT,"buttonMode" TEXT,"templateId" INTEGER,"buttonId" INTEGER,PRIMARY KEY ("id"))');
   context.executeSql('CREATE TABLE IF NOT EXISTS Segment ("id" INTEGER NOT NULL,"name" TEXT,"templateId" INTEGER,"boardType" INTEGER,PRIMARY KEY ("id"))');
     //创建表
    //  context.executeSql('INSERT INTO Button (id, name) VALUES (1, "aaa")');
    //  context.executeSql('INSERT INTO Button (id, name) VALUES (2, "aaa")');
    //  context.executeSql('INSERT INTO Button (id, name) VALUES (3, "aaa")');
    //  context.executeSql('INSERT INTO LED (id, name) VALUES (1, "aaa")');
    //  context.executeSql('INSERT INTO LED (id, name) VALUES (2, "aaa")');
    
  //取所有表中的id最大值+1作为elementCount值
});
  export default {
    data() {
      return {

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
      
    }
  }
</script>




