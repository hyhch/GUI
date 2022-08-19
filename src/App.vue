

<template>
  <el-row>
    <el-col :span="22" style="overflow-x:auto">
      <router-view :key="$route.fullPath" />
    </el-col>
    <el-col :span="2" style="background-color:#F2F6FC;">
    <h3 style="text-align:center">DesignList</h3>
      <el-menu default-active="2" class="el-menu-vertical-demo" background-color="#F2F6FC">
      <el-submenu v-for="(t_item, index) in templates" :key="index" :index = "index+''">
        <template slot="title">
          <span>{{t_item.name}}</span>
        </template>
        <el-menu-item-group>
          <el-menu-item index="${index}-1"><router-link :to="t_item.path">Edit</router-link></el-menu-item>
          <el-menu-item  index="${index}-2" @click="DeleteTemplateId = t_item.id;dialogDeleteTemplateVisible = true">Delete</el-menu-item>
        </el-menu-item-group>
      </el-submenu>
    </el-menu>
    <!-- /////////////////////////////// -->
      <!-- <div v-for="(t_item, index) in templates" :key="index">
        <router-link :to="t_item.path">{{t_item.name}}</router-link>
      </div> -->
      <el-dialog title="Tips" :visible.sync="dialogDeleteTemplateVisible">
            <span>Are you sure?</span>
            <span slot="footer" class="dialog-footer">
              <el-button type="primary" @click="deleteDesign()">Confirm</el-button>
              <el-button @click="dialogDeleteTemplateVisible=false">Cancel</el-button>
            </span>
          </el-dialog>
    </el-col>

  </el-row>
  
</template>

<script>
var db = openDatabase('BSHdb', '1.0', 'Test DB', 2 * 1024 * 1024);
  export default {
    data() {
      return {
        templates: [
          {
            id:0,
            name: "Default",
            path: "/templateDemo2/0"
          }
        ],
        dialogDeleteTemplateVisible:false,
        DeleteTemplateId:0,
      }
    },
    mounted:function(){
      this.initPage();
    },
    methods: {
      initPage(){
      //元素唯一id
        let templates = this.templates;
        db.transaction(function (context) {  
          //  tx.executeSql('CREATE TABLE IF NOT EXISTS Temp ("id" INTEGER NOT NULL,"type" INTEGER,"areaId" INTEGER,"elementId" INTEGER,PRIMARY KEY ("id"))');
          context.executeSql('CREATE TABLE IF NOT EXISTS TemplateList ("id" INTEGER NOT NULL,"name" TEXT,PRIMARY KEY ("id"))');
          context.executeSql('CREATE TABLE IF NOT EXISTS Button ("id" INTEGER NOT NULL,"name" TEXT,"hwId" TEXT,"mode" TEXT,"areaId" INTEGER,"templateId" INTEGER,"boardType" INTEGER,"parentId" INTEGER,PRIMARY KEY ("id"))');
          context.executeSql('CREATE TABLE IF NOT EXISTS LED ("id" INTEGER NOT NULL,"name" TEXT,"hwId" TEXT,"mappingName" TEXT,"mode" TEXT,"areaId" INTEGER,"segmentId" INTEGER,"templateId" INTEGER,"boardType" INTEGER,"parentId" INTEGER,PRIMARY KEY ("id"))');
          context.executeSql('CREATE TABLE IF NOT EXISTS ButtonMapping ("id" INTEGER NOT NULL,"name" TEXT,"unitname" TEXT,"buttonMode" TEXT,"templateId" INTEGER,"buttonId" INTEGER,PRIMARY KEY ("id"))');
          context.executeSql('CREATE TABLE IF NOT EXISTS Segment ("id" INTEGER NOT NULL,"name" TEXT,"mappingName" TEXT,"mode" TEXT,"templateId" INTEGER,"boardType" INTEGER,"parentId" INTEGER, "groupId" INTEGER, PRIMARY KEY ("id"))');
          context.executeSql('CREATE TABLE IF NOT EXISTS Groups ("id" INTEGER NOT NULL,"name" TEXT,"templateId" INTEGER,"boardType" INTEGER,PRIMARY KEY ("id"))');
          context.executeSql('SELECT * FROM TemplateList',[],function(context,results){
            let len = results.rows.length;
            for(let i = 0;i<len;i++){
              console.log(results);
              templates.push({name:results.rows.item(i).name,path:"/templateDemo2/"+results.rows.item(i).id,id:results.rows.item(i).id});
            }
          });
});
      },
      deleteDesign(){
        let id = this.DeleteTemplateId;
        console.log(id);
        db.transaction(function(context){
          context.executeSql('DELETE FROM TemplateList WHERE id = ?',[id]);
          context.executeSql('DELETE FROM Segment WHERE templateId = ?',[id]);
          context.executeSql('DELETE FROM LED WHERE templateId = ?',[id]);
          context.executeSql('DELETE FROM Groups WHERE templateId = ?',[id]);
          context.executeSql('DELETE FROM Button WHERE templateId = ?',[id]);
        })
        this.dialogDeleteTemplateVisible = false;
        location.reload();
      }
    }
  }
</script>

<style>
.el-menu {
	border-right:0!important;
}
</style>


