<template>
    <div>
        <el-col :span="2" style="background-color:#F2F6FC;height:800px;text-align: center;">
            <div style="text-align:center;margin-bottom: 20px;font-size: 20px;">Elements</div>
            <div draggable="true" id="button" @dragstart="dragElementType=1" @dragend="dragElementType=0">
            </div>
            <p style="margin-bottom:20px">Button</p>
            <div style="color:blue;margin:10px;font-size: 35px;">
                <i class="el-icon-s-opportunity" circle draggable="true" id="LED" @dragstart="dragElementType=2" @dragend="dragElementType=0"></i>
            </div>
            <p style="margin-bottom:20px">LED</p>
            <div draggable="true" id="segment" @dragstart="dragElementType=3" @dragend="dragElementType=0">
                <img src="../assets/1.png" style="width:50px;height:100px">
                Segment
            </div>
        </el-col>
        
        <el-col :span="22" style="overflow-x:auto" v-loading="loading">
            <div :style="radioClass">
                <el-radio v-model="boardType" label="1" @change="loadBoard()">PS</el-radio>
                <el-radio v-model="boardType" label="2" @change="loadBoard()">OS</el-radio>
                <el-button @click="loadGroup();">Edit Mapping Info</el-button>   
                <el-button @click="initSegmentManagement();">Edit Segment Info</el-button>
            </div>
            <div id="MainArea" :style="workspaceDivClass">
                
                <div :style="iconDivClass">
                    <span>
                        <i class="el-icon-zoom-in" @click="zoomIn"></i>
                    </span>
                    <el-divider direction="vertical"></el-divider>
                    <span>
                        <i class="el-icon-zoom-out" @click="zoomOut"></i>
                    </span>
                </div>
                
                <!--生成所有网格-->
                <div v-for="index in rowGridNum*rowGridNum"
                    :key="index" :style="gridStyle[index-1]" :id="index-1"
                    @dragover="ondragover($event)" @drop="ondrop($event, index-1)"
                    @click="ifGridDialogFormVisible(index-1)"
                ></div>

                <br />
                <el-button type="primary" @click="ifDialogSaveVisible"> Save </el-button>
                <el-button type="primary" @click="dialogExportVisible=true; exportRelativePath=''"> Export </el-button>

                <!-- 更改某一网格所有组件属性的表单 -->
                <el-dialog title="Set Attribute" :visible.sync="dialogFormVisible">
                    <!-- 这里需要接受参数gridDialogIndex(点击的网格的index) -->
                    <el-form :inline="true">

                        <!-- 用键值对的方式遍历gridElementOverlap中的一个对象 -->
                        <!-- 根据gridElementOverlap的结构，第一个v-for遍历了一个网格中所有组件的类型 -->
                        <!-- elementList为某一类型的组件列表，elementName为组件名 -->
                        <div v-for="(elementList, elementName, index) in gridElementOverlap[this.gridDialogIndex]" :key="index">

                            <!-- v-if根据每个组件的类型（elementName的值）生成对应表单项。 -->
                            <div v-if="elementName==='button'">
                                <el-divider v-if="elementList.length>0"><h4>BUTTON</h4></el-divider>
                                <!-- 第二个v-for遍历了某一类型组件的组件列表（elementList） -->
                                <div v-for="(buttonId, index) in elementList" :key="index">
                                    <div v-if="targetGridElement.button[buttonId].toDelete==0">
                                        <el-row>
                                            <el-col :span="18">
                                                <p>Id:{{buttonId}}</p>
                                            </el-col>
                                            <el-col :span="6">
                                                <el-button type="warning" icon="el-icon-delete" @click="selectButtonId=buttonId; 
                                                     dialogDeleteButtonVisible=true;" size="medium">Delete</el-button>
                                            </el-col>
                                        </el-row>
                                        <el-col :span="18">
                                                <p v-if="targetGridElement.button[buttonId].parentId!=0">
                                                    Mapping: {{group[targetGridElement.button[buttonId].parentId].name}}
                                                </p>
                                                <p v-else>Mapping: none</p>
                                            </el-col>
                                            <el-col :span="6">
                                                <el-button type="info" icon="el-icon-folder-opened" @click="selectButtonId=buttonId;
                                                dialogSelectButtonGroupVisible=true" size="medium">Change</el-button>
                                            </el-col>
                                        <el-form-item label="name">
                                            <el-input v-model="targetGridElement.button[buttonId].name"></el-input>
                                        </el-form-item>
                                        <el-form-item label="hwId">
                                            <el-input v-model="targetGridElement.button[buttonId].hwId"></el-input>
                                        </el-form-item>
                                        
                                    </div> 
                                </div>
                            </div>

                            <!-- 同Button -->
                            <div v-if="elementName==='led'">
                                <el-divider v-if="elementList.length>0"><h4>LED</h4></el-divider>
                                <div v-for="(ledId, index) in elementList" :key="index">
                                    <div v-if="targetGridElement.led[ledId].toDelete==0">
                                        <el-row>
                                            <el-col :span="8">
                                                <p>Id:{{ledId}}</p>
                                            </el-col>
                                            <el-col :span="6" :offset="10">
                                                <el-button type="warning" icon="el-icon-delete" @click="selectLedId=ledId; 
                                                dialogDeleteLedVisible=true;" size="medium">Delete</el-button>
                                            </el-col>
                                        </el-row>
                                        <el-row>
                                            <el-col :span="18">
                                                <p v-if="targetGridElement.led[ledId].parentId!=0">
                                                    Mapping: {{group[targetGridElement.led[ledId].parentId].name}}
                                                </p>
                                                <p v-else>Mapping: none</p>
                                            </el-col>
                                            <el-col :span="6">
                                                <el-button type="info" icon="el-icon-folder-opened" @click="selectLedId=ledId;
                                                dialogSelectLedGroupVisible=true" size="medium">Change</el-button>
                                            </el-col>
                                        </el-row>
                                        <el-row>
                                            <el-col :span="18">
                                                <p v-if="segment[targetGridElement.led[ledId].segmentId]">Segment: {{segment[targetGridElement.led[ledId].segmentId].name}}</p>
                                                <p v-else>Segment: none</p>
                                            </el-col>
                                            <el-col :span="6">
                                                <el-button type="primary" icon="el-icon-collection" @click="selectLedId=ledId; 
                                                    dialogSelectSegmentVisible=true;" size="medium">Change</el-button>
                                            </el-col>
                                        </el-row>
                                        <el-form-item label="name">
                                            <el-input v-model="targetGridElement.led[ledId].name"></el-input> 
                                        </el-form-item>
                                        <el-form-item label="hwId">
                                            <el-input v-model="targetGridElement.led[ledId].hwId"></el-input>
                                        </el-form-item>
                                        
                                    </div>
                                </div>
                            </div>
                        </div>
                        
                    <el-button type="primary" @click="submitGridDialogForm">Confirm</el-button>
                    <el-button @click="dialogFormVisible=false">Cancel</el-button>
                        
                    </el-form>
                </el-dialog>

                <!-- button删除提示 -->
                <el-dialog
                    title="Tips"
                    :visible.sync="dialogDeleteButtonVisible">
                    <span>Are you sure?</span>
                    <span slot="footer" class="dialog-footer">
                        <el-button type="primary" @click="targetGridElement.button[selectButtonId].toDelete=1; dialogDeleteButtonVisible=false;">Confirm</el-button>
                        <el-button @click="dialogDeleteButtonVisible=false">Cancel</el-button>
                    </span>
                </el-dialog>

                <!-- led删除提示 -->
                <el-dialog
                    title="Tips"
                    :visible.sync="dialogDeleteLedVisible">
                    <span>Are you sure?</span>
                    <span slot="footer" class="dialog-footer">
                        <el-button type="primary" @click="targetGridElement.led[selectLedId].toDelete=1; dialogDeleteLedVisible=false;">Confirm</el-button>
                        <el-button @click="dialogDeleteLedVisible=false">Cancel</el-button>
                    </span>
                </el-dialog>

                <!-- 下拉菜单选择led所在segment分组 -->
                <el-dialog title="Select Segment" :visible.sync="dialogSelectSegmentVisible">
                    <el-select v-model="selectSegmentId" placeholder="Please Select">
                        <el-option value=0 label="none">none</el-option>
                        <el-option
                        v-for="(seg, segId) in segment"
                        :key="segId"
                        :label="seg.name"
                        :value="segId">
                        </el-option>
                    </el-select>
                    <el-button type="primary" @click="targetGridElement.led[selectLedId].segmentId=selectSegmentId;
                        targetGridElement.led[selectLedId].toAlterSegment=1; dialogSelectSegmentVisible=false;">Confirm</el-button>
                    <el-button @click="dialogSelectSegmentVisible=false">Cancel</el-button>
                </el-dialog>






                <!-- 下拉菜单选择button所在group分组 -->
                <el-dialog title="Select Mapping" :visible.sync="dialogSelectLedGroupVisible">
                    <el-select v-model="selectGroupId" placeholder="Please Select">
                        <el-option value=0 label="none">none</el-option>
                        <el-option
                        v-for="(group, groupId) in group"
                        :key="groupId"
                        :label="group.name"
                        :value="groupId">
                        </el-option>
                    </el-select>
                    <el-button type="primary" @click="targetGridElement.led[selectLedId].parentId=selectGroupId;
                        targetGridElement.led[selectLedId].toAlterGroup=1; dialogSelectLedGroupVisible=false;">Confirm</el-button>
                    <el-button @click="dialogSelectLedGroupVisible=false">Cancel</el-button>
                </el-dialog>

                <!-- 下拉菜单选择led所在group分组 -->
                <el-dialog title="Select Mapping" :visible.sync="dialogSelectButtonGroupVisible">
                    <el-select v-model="selectGroupId" placeholder="Please Select">
                        <el-option value=0 label="none">none</el-option>
                        <el-option
                        v-for="(group, groupId) in group"
                        :key="groupId"
                        :label="group.name"
                        :value="groupId">
                        </el-option>
                    </el-select>
                    <el-button type="primary" @click="targetGridElement.button[selectButtonId].parentId=selectGroupId;
                        targetGridElement.button[selectButtonId].toAlterGroup=1; dialogSelectButtonGroupVisible=false;">Confirm</el-button>
                    <el-button @click="dialogSelectButtonGroupVisible=false">Cancel</el-button>
                </el-dialog>
                
                <!-- 保存按钮弹出窗口 -->
                <el-dialog title="Please input the design name" :visible.sync="dialogSaveVisible">
                    <el-form :inline="true">
                        <el-form-item label="name">
                            <el-input v-model="templateName"></el-input>
                        </el-form-item>
                        <el-button type="primary" @click="submitTotalForm">Confirm</el-button>
                        <el-button @click="dialogSaveVisible=false">Cancel</el-button>
                    </el-form>
                </el-dialog>

                <el-dialog title="Please input export path" :visible.sync="dialogExportVisible">
                    <el-form>
                        <el-form-item label="System path">
                            <el-input v-model="exportRelativePath" :disabled="fixPath!=0"></el-input>
                            <el-radio-group v-model="fixPath">
                                <el-radio :label="3">Documents</el-radio>
                                <el-radio :label="2">Downloads</el-radio>
                                <el-radio :label="1">Desktop</el-radio>
                                <el-radio :label="0">Other</el-radio>
                            </el-radio-group>
                        </el-form-item>
                        <el-form-item label="File Name">
                            <el-input v-model="exportFileName"></el-input>
                        </el-form-item>
                        <el-button type="primary" @click="dialogExportVisible=false; testAxios();">Confirm</el-button>
                        <el-button @click="dialogExportVisible=false">Cancel</el-button>
                    </el-form>
                </el-dialog>

                <!-- Segment管理弹窗 -->
                <el-dialog title="Edit Segment Info" :visible.sync="dialogSegmentManagementVisible">
                    <el-table
                    :data="segmentList"
                    stripe
                    style="width: 100%">
                        <el-table-column
                            prop="id"
                            label="id">
                        </el-table-column>
                        <el-table-column
                            prop="name"
                            label="name">
                        </el-table-column>
                        <el-table-column label="Operations">
                            <template slot-scope="scope">
                                <el-button size="mini" @click="manageSegmentId=scope.row.id;editSegmentChild();">Edit</el-button>
                                <el-button size="mini" type="danger" @click="dialogDeleteSegmentVisible=true; manageSegmentId=scope.row.id;">Delete</el-button>
                            </template>
                        </el-table-column>
                    </el-table>
                   
                        <el-button type="primary" icon="el-icon-circle-plus-outline" @click="newSegmentName=''; dialogSegmentNameVisible=true">Add Segment</el-button>
                    <el-button type="text" @click="dialogSegmentManagementVisible=false">Cancel</el-button>
                </el-dialog>

                <!-- 为新建Segment取名 -->
                <el-dialog title="Input Name" :visible.sync="dialogSegmentNameVisible">
                    <el-input v-model="newSegmentName" placeholder="Please input segment name"></el-input>
                    <div>
                        <el-button type="primary" size="mini" @click="putSegment(segmentCount, newSegmentName, 1, defaultSegmentId, 0);
                            dialogSegmentNameVisible=false; segmentList.push({id: segmentCount, name: newSegmentName}); segmentCount++;">Confirm</el-button>
                        <el-button size="mini" type="text" @click="dialogSegmentNameVisible=false;">Cancel</el-button>
                    </div>
                </el-dialog>

                <!-- Segment删除提示 -->
                <el-dialog
                    title="Tips"
                    :visible.sync="dialogDeleteSegmentVisible">
                    <span>Do you confirm to delete this segment(id={{manageSegmentId}})?</span><br/>
                    <span>This operation is not reversible</span>
                    <span slot="footer" class="dialog-footer">
                        <el-button type="primary" @click="deleteSegment(manageSegmentId)">Confirm</el-button>
                        <el-button @click="dialogDeleteSegmentVisible=false">Cancel</el-button>
                    </span>
                </el-dialog>

                <!-- 管理Segment子组件的穿梭框 -->
                <el-dialog title="Edit Segment Info" :visible.sync="dialogSegmentChildVisible">
                    <el-transfer :data="availableSegmentChild" v-model="segmentChildMember"></el-transfer>
                    <el-button type="primary" @click="alterSegmentChild(); dialogSegmentChildVisible=false">Confirm</el-button>
                    <el-button type="text" @click="dialogSegmentChildVisible=false">Cancel</el-button>
                </el-dialog>

                <!-- 管理group信息 -->
                <el-dialog title="Edit Mapping Info" :visible.sync="dialogGroupVisible" width="500">
                    <el-table :data="groupList" height="300">
                        <el-table-column property="id" label="id" width="150"></el-table-column>
                        <el-table-column property="name" label="name" width="300"></el-table-column>
                        <el-table-column label="Operation" fixed="right">
                            <template slot-scope="scope">
                                <el-button size="medium" @click="groupId=scope.row.id; groupEdit();">Edit</el-button>
                                <el-button size="medium" type="danger" @click="groupId = scope.row.id; dialogDeleteGroupVisible = true;">Delete</el-button>
                            </template>
                        </el-table-column>
                    </el-table>
                    <el-button size="mini" type="primary" icon="el-icon-circle-plus-outline" @click="dialogAddGroupVisible = true">Add Mapping</el-button>
                    <el-button size="mini" type="text" @click="dialogGroupVisible=false">Cancel</el-button>
                </el-dialog>

                <!-- group删除提示 -->
                <el-dialog
                    title="Tips"
                    :visible.sync="dialogDeleteGroupVisible">
                    <span>Are you sure?</span>
                    <span slot="footer" class="dialog-footer">
                        <el-button type="primary" @click="groupDeleteConfirm">Confirm</el-button>
                        <el-button @click="dialogDeleteGroupVisible=false">Cancel</el-button>
                    </span>
                </el-dialog>
                <!-- 新增group信息 -->
                <el-dialog title="Please input mapping name" :visible.sync="dialogAddGroupVisible">
                    <el-form>
                        <el-form-item label="name">
                            <el-input v-model="newGroupName"></el-input>
                        </el-form-item>
                        <el-button type="primary" @click="putGroup(groupCount, newGroupName, 1); dialogAddGroupVisible=false;
                            groupList.push({id:groupCount, name:newGroupName}); groupCount++">Confirm</el-button>
                        <el-button @click="dialogAddGroupVisible=false">Cancel</el-button>
                    </el-form>
                </el-dialog>
                <!-- group双向穿梭框 -->
                <el-dialog title="Edit Mapping Info" :visible.sync="dialogEditGroupVisible">
                    <el-form>
                        <el-transfer v-model="groupContent" :data="availableElements" :titles="['Available', 'Mapping']"></el-transfer>
                        <el-button type="primary" @click="submitGroup()">Confirm</el-button>
                        <el-button @click="dialogEditGroupVisible=false">Cancel</el-button>
                    </el-form>
                </el-dialog>
            </div>
        </el-col>
    </div>
</template>

<script>
    import axios from 'axios';

    var db = openDatabase('BSHdb', '1.0', 'Test DB', 2 * 1024 * 1024);
    
    
    export default {
        name: 'templateDemo2',
        data() {
            return {
                buttonCount: 1,
                ledCount: 1,
                segmentCount: 1,
                groupCount: 1,
                loading: false,
                fixPath: 1,
                exportRelativePath:'',
                exportFileName: '',
                templateName:'',
                boardType: "1",
                //templateId：默认为0，保存后根据保存的id改变
                templateId:this.$route.params.templateId,
                //未被设置Segment分组的led的默认segmentId
                defaultSegmentId: 0,
                dragElementType: 1,
                dialogExportVisible:false,
                //保存弹窗默认关闭
                dialogSaveVisible:false,
                // 确认删除弹窗
                dialogDeleteButtonVisible: false,
                dialogDeleteLedVisible: false,
                dialogDeleteSegmentVisible: false,
                // 表单弹窗默认关闭
                dialogFormVisible: false,
                dialogEditGroupVisible:false,
                dialogAddGroupVisible:false,
                dialogDeleteGroupVisible:false,
                //编辑group弹窗，默认关闭
                dialogGroupVisible:false,
                // 选择segment组别弹窗
                dialogSelectSegmentVisible: false,
                dialogSelectButtonGroupVisible: false,
                dialogSelectLedGroupVisible: false,
                // 管理segment弹窗
                dialogSegmentManagementVisible: false,
                dialogSegmentNameVisible: false,
                dialogSegmentChildVisible: false,
                totalWidth: 800,
                // 每行网格个数
                rowGridNum: 35,
                // 每个网格的边长，mounted时将被重新计算
                gridWidth: 30,
                // 网格绑定的样式，在mounted中初始化
                gridStyle: [],
                /*
                    描述： gridIndex指网格的index; button列表存放所有button组件的id值; led列表同理。
                    构造：只要一个组件的ui图标填充了一个网格，那么就将该组件的id添加至该网格index所对应的组件列表中。
                    举例：组件Button的id为1，ui图标大小为2*2，覆盖了index分别为1, 2, 36, 37的网格，
                        那么程序便会在gridElementOverlap中这四个index下的button列表中分别添加Button的id值1
                    用法：用户点击一个网格，直接对gridElementOverlap索引网格index找到该网格上overlap的所有组件，修改对应参数值
                    注：gridIndex=0代表第1个格子
                    gridElementOverlap {
                        gridIndex1: {
                            button: [
                                buttonId1, buttonId2, ...
                            ],
                            led: [
                                ledId1, ledId2, ...
                            ]
                        }
                        ...
                    }
                */
                gridElementOverlap: {},

                radioClass:{
                    // display:'inline',
                    textAlign: 'left',
                    marginRight: '2%',
                    marginBottom: '1%',
                    marginTop: '1%',
                },

                // 用户点击的网格所对应的index值。生成表单时需要通过这个值找到某一网格下的所有组件
                gridDialogIndex: 0,

                /*
                    经过尝试，发现把button以及led声明为对象形式，比列表形式更方便（主要是在表单读取网格组件信息时方便在此处索引组件id获取数据）
                    buttonId: {
                        name: 
                        hwId: 
                        parentId:
                    }
                    ledId: {
                        name: 
                        hwId: 
                        segmentId: 
                        parentId: 
                    }
                    segment {
                        segmentId1: {
                            name: 
                            parentId: 
                            groupId: 
                            ledMember: [ledId1, ledId2, ...]
                            segmentMember: [segmentId1, segmentId2]
                        }
                    }
                    group {
                        groupId1: {
                            name:
                            buttonMember: []
                            segmentMember: []
                        }
                    }
                */
                button: {},
                led: {},
                segment: {},
                group:{},
                //存储group信息的数组
                groupList: [],
                groupContent: [],
                availableElements: [],
                newGroupName: '',
                groupId: 0,
                // 用于绑定Segment管理中的表格数据
                segmentList: [],
                
                // 在表单中对某一Button进行单独操作时更新该值
                selectButtonId: '',

                // 在表单中对某一led进行单独操作时更新该值
                selectLedId: '',

                // 和segment的下拉菜单选择项做双向绑定
                selectSegmentId: '',
                selectGroupId: '',

                // 绑定输入的新增segment的名称
                newSegmentName: '',

                // 在SegmentManagement弹窗中操作的待删除Segment的id值
                manageSegmentId: '',
                // 所有可选的子组件选项
                availableSegmentChild: [],
                segmentChildMember: [],

                /*
                    描述：与gridElementOverlap不同，这里存了特定一个网格中的所有组件的信息
                    用法：与表单数据做双向绑定
                    targetGridElement {
                        button: {
                            buttonId1: {
                                name:
                                hwId:
                                parentId:
                            },
                            ...
                        }
                        led: {
                            ledId1: {
                                name: 
                                hwId: 
                                segmentId:
                                parentId:
                            }
                            ...
                        }
                    }
                */
                targetGridElement: {
                    button: {},
                    led: {}
                },

                // 分别定义：背景颜色，单button颜色，单led颜色，组件叠加颜色
                backgroundColor: '#FFFFFF',
                buttonColor: "#8CFE90",
                ledColor: "#00EFEF",
                mixColor: "#000080",

                // 微调放大/缩小图标的布局
                iconDivClass: {
                    textAlign: 'right',
                    marginRight: '2%',
                    marginBottom: '1%',
                    marginTop: '1%'
                },
                // 微调网格的布局
                // offsetWidth会随此处的width改变
                workspaceDivClass: {
                    width: '96%',
                    height: '730px',
                }

            }
        }, 
        mounted: function() {
            this.loadBoard()
        },
        methods: {
            testAxios() {
                // 向后端传递组件参数
                let psButtonList = []
                let osButtonList = []
                let psLedList = []
                let osLedList = []
                let psSegmentList = []
                let osSegmentList = []
                
                this.loading = true
                if (this.fixPath == 3) {
                    this.exportRelativePath = 'document'
                } else if (this.fixPath == 2) {
                    this.exportRelativePath = 'download'
                } else if(this.fixPath == 1) {
                    this.exportRelativePath = 'desktop'
                } else {
                    1 + 1
                }
                
                
                let _this = this
                db.transaction(function (context) {        
                    context.executeSql('SELECT name, hwId, boardType FROM Button WHERE templateId = ?', 
                        [_this.templateId], function (context, results) {
                            for (let i = 0 ; i < results.rows.length ; ++i) {
                                if (results.rows[i].boardType == 1) {
                                    psButtonList.push({
                                        name: results.rows[i].name,
                                        hwId: results.rows[i].hwId
                                    })
                                } else {
                                    osButtonList.push({
                                        name: results.rows[i].name,
                                        hwId: results.rows[i].hwId
                                    })
                                }
                            }
                            axios ({
                                method: 'post',
                                url: '/save/touchButton',
                                data: {
                                    psButton: psButtonList,
                                    osButton: osButtonList
                                }
                                }).then(function (response) {
                                    console.log(response.data)
                            });
                            axios ({
                                method: 'get',
                                url: '/save/exportFile',
                                params: {
                                    relativePath: _this.exportRelativePath,
                                    fileName: _this.exportFileName
                                }
                                }).then(function (response) {
                                    if (response.data == "SAVE SUCCESSFULLY") {
                                        _this.loading = false
                                        _this.$message({
                                            message: 'Success',
                                            type: 'success',
                                            duration: 1500
                                        });
                                    }
                            });

                    });

                    context.executeSql('SELECT name, hwId, boardType FROM LED WHERE templateId = ?', 
                        [_this.templateId], function (context, results) {
                            for (let i = 0 ; i < results.rows.length ; ++i) {
                                if (results.rows[i].boardType == 1) {
                                    psLedList.push({
                                        name: results.rows[i].name,
                                        hwId: results.rows[i].hwId
                                    })
                                } else {
                                    osLedList.push({
                                        name: results.rows[i].name,
                                        hwId: results.rows[i].hwId
                                    })
                                }
                            }
                            axios ({
                                method: 'post',
                                url: '/save/physicalLed',
                                data: {
                                    psLed: psLedList,
                                    osLed: osLedList
                                }
                                }).then(function (response) {
                                    console.log(response.data)
                            });
                            axios ({
                                method: 'get',
                                url: '/save/exportFile',
                                params: {
                                    relativePath: _this.exportRelativePath,
                                    fileName: _this.exportFileName
                                }
                                }).then(function (response) {
                                    if (response.data == "SAVE SUCCESSFULLY") {
                                        _this.loading = false
                                        _this.$message({
                                            message: 'Success',
                                            type: 'success',
                                            duration: 1500
                                        });
                                    }
                            });
                    });

                    // 提取PS以及OS板的segment所包含的所有子组件，且按segmentId排序
                    context.executeSql('SELECT * FROM (SELECT LED.name AS memberName, Segment.id AS segParentId, Segment.name as segParentName, Segment.parentId AS upperId, \
                    LED.boardType AS boardType, \'led\' AS elType FROM LED INNER JOIN Segment ON LED.segmentId = Segment.id WHERE LED.templateId = ? UNION SELECT SegChild.name AS memberName,\
                     SegParent.id AS segParentId, SegParent.name AS segParentName, SegParent.parentId AS upperId, SegChild.boardType AS boardType, \'segment\' AS elType \
                     FROM Segment AS SegChild INNER JOIN Segment AS SegParent ON SegChild.parentId = SegParent.id WHERE SegChild.templateId = ?)\
                      ORDER BY segParentId ASC', [_this.templateId, _this.templateId], function(context, results) {

                            let lastSegmentId = ''
                            for (let i = 0 ; i < results.rows.length ; ++i) {
                                // 当一条数据的segmentId改变时，新增一个psSegmentList项
                                // 由于results.rows所获取的数据已按segmentId排序，所以可以保证新增psSegmentList项时不重复
                                if (results.rows[i].boardType == 1) {
                                    if (results.rows[i].segParentId != lastSegmentId) {
                                        psSegmentList.push({
                                            name: results.rows[i].segParentName,
                                            member: [{
                                                elementName: results.rows[i].memberName,
                                                elementType: results.rows[i].elType,
                                            }],
                                            upperId: results.rows[i].upperId
                                        })
                                        lastSegmentId = results.rows[i].segParentId
                                    } else {
                                        psSegmentList[psSegmentList.length - 1].member.push({
                                                elementName: results.rows[i].memberName,
                                                elementType: results.rows[i].elType,
                                            })
                                    }
                                } else {
                                    if (results.rows[i].segParentId != lastSegmentId) {
                                        osSegmentList.push({
                                            name: results.rows[i].segParentName,
                                            member: [{
                                                elementName: results.rows[i].memberName,
                                                elementType: results.rows[i].elType,
                                            }],
                                            upperId: results.rows[i].upperId
                                        })
                                        lastSegmentId = results.rows[i].segParentId
                                    } else {
                                        osSegmentList[osSegmentList.length - 1].member.push({
                                                elementName: results.rows[i].memberName,
                                                elementType: results.rows[i].elType
                                        })
                                    }
                                }
                            }
                            axios ({
                                method: 'post',
                                url: '/save/virtualSegment',
                                data: {
                                    psSegment: psSegmentList,
                                    osSegment: osSegmentList
                                }
                                }).then(function (response) {
                                    console.log(response.data)
                            });
                            axios ({
                                method: 'get',
                                url: '/save/exportFile',
                                params: {
                                    relativePath: _this.exportRelativePath,
                                    fileName: _this.exportFileName
                                }
                                }).then(function (response) {
                                    if (response.data == "SAVE SUCCESSFULLY") {
                                        _this.loading = false
                                        _this.$message({
                                            message: 'Success',
                                            type: 'success',
                                            duration: 1500
                                        });
                                    }
                            });
                    })
                })
               
            },
            pageInit(){
                this.button = {};
                this.led = {};
                this.segment={}
                this.group = {}
                this.gridElementOverlap = {};
                // 获取中间工作区的页面宽度
                this.totalWidth = document.getElementById("MainArea").offsetWidth;
                // 计算一个网格的边长, 减去0.01防止四舍五入后总宽度不够，在浏览器允许的缩放范围内这个误差够了
                // 1+this.rowGridNum用于抵消滚动条和一些玄学误差
                this.gridWidth = this.totalWidth / (1 + this.rowGridNum) - 0.01
                this.gridStyle = [];
                // 初始化所有网格的样式
                for (let i = 0 ; i < this.rowGridNum * this.rowGridNum ; ++i) {
                    document.getElementById(i).innerHTML = "";
                    this.gridStyle.push(
                        {
                            width: this.gridWidth + 'px',
                            height: this.gridWidth + 'px',
                            display: 'inline-block',
                            border: '0.2px dashed gray',
                            'box-sizing': 'border-box',
                            'font-size': '1px',
                            float: 'left',
                            background: this.backgroundColor,
                        }
                    )
                }
                // 监听改变页面大小的事件
                let _this = this
                window.onresize = function() {
                    _this.alterGridWidth()
                }
                // 利用elemet-resize-detector, 监听放大/缩小事件
                var elementResizeDetectorMaker = require("element-resize-detector");
                let erd = elementResizeDetectorMaker()
                erd.listenTo(document.getElementById("MainArea"), function() {
                    _this.alterGridWidth()
                });
            },
            loadBoard(){
                this.pageInit()
                let that = this;
                //使用that来接受this的内容，此时可以通过that访问data中的数据
                db.transaction(function (context) { 
                        // 读取group数据
                        context.executeSql('SELECT * FROM Groups WHERE templateId = ? and boardType = ?',[that.templateId,that.boardType],function(context,results){
                            let len = results.rows.length;
                            if(len>0)
                            {
                                for(let i = 0;i<len;i++){
                                    let groupId = results.rows.item(i).id
                                    let name = results.rows.item(i).name;
                                    that.putGroup(groupId, name, 0);
                                }
                            }
                        });
                        // 读取Button数据
                        context.executeSql('SELECT * FROM Button WHERE templateId = ? and boardType = ?',[that.templateId,that.boardType],function(context,results){
                            let len = results.rows.length;
                            for(let i = 0;i<len;i++){
                                let index = results.rows.item(i).areaId
                                let elementId = results.rows.item(i).id
                                let name = results.rows.item(i).name
                                let hwId = results.rows.item(i).hwId;
                                let parentId = results.rows.item(i).parentId;
                                that.putButton(index, elementId, name, hwId, 0,parentId);
                            }
                        });
                        // 读取Segment数据
                        context.executeSql('SELECT * FROM Segment WHERE templateId = ? and boardType = ?',[that.templateId,that.boardType],function(context,results){
                            let len = results.rows.length;
                            if(len>0)
                            {
                                for(let i = 0;i<len;i++){
                                    let segmentId = results.rows.item(i).id
                                    let name = results.rows.item(i).name;
                                    let parentId = results.rows.item(i).parentId;
                                    let groupId = results.rows.item(i).groupId
                                    // // 存储default分组的id值
                                    // if (name == 'default') {
                                    //     that.defaultSegmentId = segmentId;
                                    // }
                                    that.putSegment(segmentId, name, 0,parentId, groupId);
                                }
                            }
                        });
                        // 读取led数据
                        context.executeSql('SELECT * FROM LED WHERE templateId = ? and boardType = ?',[that.templateId,that.boardType],function(context,results){
                            let len = results.rows.length;
                            for(let i = 0;i<len;i++){
                                let index = results.rows.item(i).areaId;
                                let elementId = results.rows.item(i).id;
                                let name = results.rows.item(i).name
                                let hwId = results.rows.item(i).hwId
                                let segmentId = results.rows.item(i).segmentId
                                let parentId = results.rows.item(i).parentId;
                                that.putLed(index, elementId, name, hwId, segmentId, 0,parentId);
                            }
                        });

                        context.executeSql('SELECT MAX(bt.id) AS max_result FROM Button as bt', [], function (context, results) {
                            if (results.rows.length > 0) {
                                that.buttonCount = results.rows.item(0).max_result+1;
                            }
                        });
                        context.executeSql('SELECT MAX(led.id) AS max_result FROM LED as led', [], function (context, results) {
                            if (results.rows.length > 0) {
                                that.ledCount = results.rows.item(0).max_result+1;
                            }
                        });
                        context.executeSql('SELECT MAX(seg.id) AS max_result FROM Segment as seg', [], function (context, results) {
                            if (results.rows.length > 0) {
                                that.segmentCount = results.rows.item(0).max_result+1;
                            }
                        });
                        context.executeSql('SELECT MAX(grp.id) AS max_result FROM Groups as grp', [], function (context, results) {
                            if (results.rows.length > 0) {
                                that.groupCount = results.rows.item(0).max_result+1;
                            }
                        });
                });
            },
            ondragover(event) {
                event.preventDefault()
            },
            ondrop(event, index)  {
                event.preventDefault()
                let rangeIndex
                let elementName
                switch (this.dragElementType) {
                    // 新建一个button
                    case 1:
                        elementName = (this.boardType == 1 ? 'PS_Btn_' : 'OS_Btn_') + this.buttonCount
                        this.putButton(index, this.buttonCount, elementName, '', 1,0)
                        this.buttonCount++
                        break
                    // 新建一个led
                    case 2:
                        elementName = (this.boardType == 1 ? 'PS_LED_' : 'OS_LED_') + this.ledCount
                        this.putLed(index, this.ledCount, elementName, '', this.defaultSegmentId, 1,0)
                        this.ledCount++
                        break
                    // 新建一个segment并生成7个相应的led
                    case 3:
                        this.putSegment(this.segmentCount, "SegmentGroup" + this.segmentCount, 1,0,0);
                        rangeIndex = [index, index-1+this.rowGridNum, index+1+this.rowGridNum, index+2*this.rowGridNum,
                            index-1+3*this.rowGridNum, index+1+3*this.rowGridNum, index+4*this.rowGridNum]
                        for (let i = 0 ; i < rangeIndex.length ; ++i) {
                            elementName = (this.boardType == 1 ? 'PS_LED_' : 'OS_LED_') + this.ledCount
                            this.putLed(rangeIndex[i], this.ledCount, elementName, '', this.segmentCount, 1,0)
                            this.ledCount++
                        }
                        this.segmentCount++
                        break
                    default:
                        console.log('other situations')
                        break
                }
                this.dragElementType = 0
            },
            putButton(index, elementId, _name, _hwId, isCreate,_parentId) {
                let templateId = this.templateId
                let boardType = this.boardType
                // button覆盖的index
                let rangeIndex = [index, index+1, index+this.rowGridNum, index+this.rowGridNum+1]
                for(let i = 0 ; i < rangeIndex.length ; ++i) {
                    // 如果覆盖位置上已存在其它组件，就将背景颜色设为mixColor，否则设为buttonColor
                    let curBackground = this.gridStyle[rangeIndex[i]].background
                    if (curBackground == this.backgroundColor) {
                        this.gridStyle[rangeIndex[i]].background = this.buttonColor
                    } else {
                        this.gridStyle[rangeIndex[i]].background = this.mixColor
                    }

                    //已有元素在上面的格子变成可点击的手型 
                    this.gridStyle[rangeIndex[i]].cursor="pointer"

                    if (this.gridElementOverlap[rangeIndex[i]] == null) {
                        this.gridElementOverlap[rangeIndex[i]] = {
                            button: [],
                            led: []
                        }
                    }
                    this.gridElementOverlap[rangeIndex[i]]['button'].push(
                        elementId
                    )
                }

                this.button[elementId] = {
                    name: _name,
                    hwId: _hwId,
                    parentId:_parentId
                }

                if (_parentId != 0) {
                    this.group[_parentId].buttonMember.push(elementId)
                }

                if (isCreate == 1) {
                    db.transaction(function (context) {  
                        context.executeSql('INSERT INTO Button (id,name,hwId,areaId,templateId,boardType,parentId) VALUES (?,?,?,?,?,?,?)',[elementId,_name,_hwId,index,templateId,boardType,_parentId]);
                    })
                }
            },
            putLed(index, elementId, _name, _hwId, _segmentId, isCreate,_parentId) {
                let templateId = this.templateId
                let boardType = this.boardType
               
                this.gridStyle[index].cursor = "pointer";
                document.getElementById(index).innerHTML = "<div style = 'background-color:#409EFF;width:50%;height:50%;margin:auto'></div>";
                //拖动按钮松开后蓝色代表LED
                if (this.gridElementOverlap[index] == null) {
                    this.gridElementOverlap[index] = {
                        button: [],
                        led: []
                    }
                }
                this.gridElementOverlap[index]['led'].push(
                    elementId
                )

                this.led[elementId] = {
                    name: _name,
                    hwId: _hwId,
                    segmentId: _segmentId,
                    parentId:_parentId
                }
                // 把新建led添加至给定的segment组别中
                if(_segmentId!=0){
                    this.segment[_segmentId].ledMember.push(elementId);
                }
                if (_parentId != 0) {
                    this.group[_parentId].ledMember.push(elementId)
                }
                //将LED插入数据库
                if (isCreate == 1) {
                    db.transaction(function (context) {  
                        context.executeSql('INSERT INTO LED (id,name,hwId,areaId,segmentId,templateId,boardType,parentId) VALUES (?,?,?,?,?,?,?,?)',[elementId,_name,_hwId,index,_segmentId,templateId,boardType,_parentId]);
                    });
                }
            },
            putSegment(segmentId, _name, isCreate,_parentId, _groupId) {
                let templateId = this.templateId
                let boardType = this.boardType
                // 新建一个segment组别
                this.segment[segmentId] = {
                    name: _name,
                    ledMember: [],
                    segmentMember: [],
                    parentId:_parentId,
                    groupId: _groupId
                }
                if (_groupId != 0) {
                    this.group[_groupId].segmentMember.push(segmentId)
                }
                
                if (isCreate == 1) {
                    db.transaction(function (context) {  
                        context.executeSql('INSERT INTO Segment (id,name,templateId,boardType,parentId, groupId) VALUES (?,?,?,?,?,?)',[segmentId,_name,templateId,boardType,_parentId,_groupId]);
                    });
                }
                
            },
            putGroup(groupId, _name, isCreate){
                let templateId = this.templateId
                let boardType = this.boardType

                this.group[groupId] = {
                    name: _name,
                    buttonMember: [],
                    ledMember: [],
                    segmentMember: [],
                }

                if (isCreate == 1) {
                    db.transaction(function (context) { 
                        context.executeSql('INSERT INTO Groups (id,name,templateId,boardType) VALUES (?,?,?,?)',[groupId,_name,templateId,boardType]);
                    });
                }
                this.dialogAddGroupVisible = false;
            },
            //读取group列表, 存入groupList
            loadGroup(){
                this.groupList = []
                for (let groupId in this.group) {
                    this.groupList.push({
                        id: groupId,
                        name: this.group[groupId].name
                    })
                }
                this.dialogGroupVisible = true
            },
            //点击编辑group
            groupEdit() {
                let groupIdTmp = this.groupId;
                this.availableElements =[];
                this.groupContent = [];
                for (let buttonId in this.button) {
                    let name = this.button[buttonId].name
                    let parentId = this.button[buttonId].parentId
                    if (parentId == 0 || parentId == groupIdTmp) {
                        this.availableElements.push({
                            key: 'Button:'+buttonId,
                            label: 'Button:'+name,
                        })
                    }
                    if (parentId == groupIdTmp) {
                        this.groupContent.push("Button:"+buttonId)
                    }
                }

                // 同一个led只能被分为group或是segment
                for (let ledId in this.led) {
                    let name = this.led[ledId].name
                    let parentId = this.led[ledId].parentId
                    let segmentId = this.led[ledId].segmentId
                    if (segmentId == 0 && (parentId == 0 || parentId == groupIdTmp)) {
                        this.availableElements.push({
                            key: "LED:"+ledId,
                            label: "LED:"+name,
                        })
                    }
                    if (parentId == groupIdTmp) {
                        this.groupContent.push("LED:"+ledId)
                    }
                }

                // 只允许对游离的segment进行group分组
                for (let segmentId in this.segment) {
                    let name = this.segment[segmentId].name
                    let groupId = this.segment[segmentId].groupId
                    let parentId = this.segment[segmentId].parentId
                    if ((groupId == 0 || groupId == groupIdTmp) && parentId == 0) {
                        this.availableElements.push({
                            key: 'Segment:'+segmentId,
                            label: 'Segment:'+name,
                        })
                    }
                    if (groupId == groupIdTmp) {
                        this.groupContent.push("Segment:"+segmentId)
                    }
                }
                
                this.dialogEditGroupVisible = true;
            },
            submitGroup(){
                let groupIdTmp = this.groupId;
                let targetGroup = this.group[groupIdTmp]

                // 重置所有原始子组件的parentId
                for (let i in this.group[this.groupId].buttonMember) {
                    this.button[targetGroup.buttonMember[i]].parentId = 0
                }

                for (let i in this.group[this.groupId].ledMember) {
                    this.led[targetGroup.ledMember[i]].parentId = 0
                }

                for (let i in this.group[this.groupId].segmentMember) {
                    this.segment[targetGroup.segmentMember[i]].groupId = 0
                }

                targetGroup.buttonMember = []
                targetGroup.ledMember = []
                targetGroup.segmentMember = []

                // 将编辑后的所有子组件的parentId置为当前groupId
                for (let i in this.groupContent) {
                    let element = this.groupContent[i]
                    let elementName = element.split(':')[0]
                    let elementId = element.split(':')[1]
                    if (elementName == 'Button') {
                        this.button[elementId].parentId = groupIdTmp
                        targetGroup.buttonMember.push(elementId)
                    } else if (elementName == 'LED') {
                        this.led[elementId].parentId = groupIdTmp
                        targetGroup.ledMember.push(elementId)
                    } else {
                        this.segment[elementId].groupId = groupIdTmp
                        targetGroup.segmentMember.push(elementId)
                    }
                }
                let buttonChild = targetGroup.buttonMember
                let ledChild = targetGroup.ledMember
                let segmentChild = targetGroup.segmentMember

                db.transaction(function (context) {
                    context.executeSql('UPDATE Button SET parentId = 0 WHERE parentId = ?', [groupIdTmp])
                    context.executeSql('UPDATE LED SET parentId = 0 WHERE parentId = ?', [groupIdTmp])
                    context.executeSql('UPDATE Segment SET groupId = 0 WHERE groupId = ?', [groupIdTmp])
                    for (let i in buttonChild) {
                        context.executeSql('UPDATE Button SET parentId=? WHERE id =?',[groupIdTmp,buttonChild[i]]);
                    }
                    for (let i in ledChild) {
                        context.executeSql('UPDATE LED SET parentId=? WHERE id =?',[groupIdTmp,ledChild[i]]);
                    }      
                    for (let i in segmentChild) {
                        context.executeSql('UPDATE Segment SET groupId=? WHERE id=?',[groupIdTmp,segmentChild[i]])
                    }
                })
                
                this.$message({
                    message: 'Success!',
                    type: 'success',
                    duration: 1500
                });

                this.dialogEditGroupVisible = false;
            },
            //点击删除group后确认
            groupDeleteConfirm(){
                let groupIdTmp = this.groupId;
                let buttonMember = this.group[groupIdTmp].buttonMember
                let ledMember = this.group[groupIdTmp].ledMember
                let segmentMember = this.group[groupIdTmp].segmentMember

                // 重置所有子组件的parentId
                for (let i in buttonMember) {
                    this.button[buttonMember[i]].parentId = 0
                }
                for (let i in ledMember) {
                    this.led[ledMember[i]].parentId = 0
                }
                for (let i in segmentMember) {
                    this.segment[segmentMember[i]].groupId = 0
                }

                // 删除group和groupList中的对应项
                for (let i = 0 ; i < this.groupList.length ; ++i) {
                    if (this.groupList[i].id == groupIdTmp) {
                        this.groupList.splice(i, 1)
                        break
                    }
                }
                delete this.group[groupIdTmp]

                db.transaction(function (context) { 
                    context.executeSql('DELETE FROM Groups WHERE id = ?',[groupIdTmp]);
                    context.executeSql('UPDATE Button SET parentId=0 WHERE parentId =?',[groupIdTmp]);
                    context.executeSql('UPDATE LED SET parentId=0 WHERE parentId =?',[groupIdTmp]);
                    context.executeSql('UPDATE Segment SET groupId=0 WHERE groupId=?', [groupIdTmp])
                });
                this.dialogDeleteGroupVisible = false;
                this.$message({
                    message: 'Success!',
                    type: 'success',
                    duration: 1500
                });

            },

            // 打开管理Segment弹窗前进行的初始化工作
            initSegmentManagement() {
                this.segmentList = []
                for (let segmentId in this.segment) {
                    this.segmentList.push({
                        id: segmentId,
                        name: this.segment[segmentId].name
                    })
                }
                this.dialogSegmentManagementVisible = true
            },
            // 打开编辑Segment子组件弹窗并进行初始化工作
            editSegmentChild() {
                this.segmentChildMember = []
                this.availableSegmentChild = []

                // 判断两个segment之间是否存在父子关系
                let _this = this
                function isOfSegmentChild(childId, parentId) {
                    let childSegment = _this.segment[childId]
                    while (childSegment.parentId != 0) {
                        if (childSegment.parentId == parentId) {
                            return true;
                        }
                        childSegment = _this.segment[childSegment.parentId]
                    }
                    return false;
                }

                // 游离的led或是parent为选中segment的所有led
                // 同时除去所有已选择group分组的led
                for (let i in this.led) {
                    if ((this.led[i].segmentId == 0 || this.led[i].segmentId==this.manageSegmentId) && this.led[i].parentId == 0) {
                        this.availableSegmentChild.push(
                            {
                                key: 'led:'+i,
                                label: this.led[i].name,
                            }
                        )
                    }
                    // 初始化已成为子组件的led
                    if (this.led[i].segmentId == this.manageSegmentId) {
                        this.segmentChildMember.push('led:'+i)
                    }
                }

                // 游离的segment或是parent为选中segment的所有segment, 并且要保证添加的segment不是选中的segment的parent以及选中的segment其本身
                // 同时，不考虑所有已被group分组的segment
                for (let i in this.segment) {
                    if ((this.segment[i].parentId == 0 || this.segment[i].parentId == this.manageSegmentId) 
                        && isOfSegmentChild(this.manageSegmentId, i) == false && i != this.manageSegmentId && this.segment[i].groupId == 0) {
                        this.availableSegmentChild.push(
                            {
                                key: 'segment:'+i,
                                label: this.segment[i].name,
                            }
                        )
                    }
                    if (this.segment[i].parentId == this.manageSegmentId) {
                        this.segmentChildMember.push('segment:'+i)
                    }
                }
                this.dialogSegmentChildVisible = true
            },
            // 根据manageSegmentId和segmentChildMember, 更改segment相关组件之间的父子关系
            alterSegmentChild() {
                let targetSegment = this.segment[this.manageSegmentId]

                // 重置相关led和segment的父id
                for (let i = 0 ; i < targetSegment.ledMember.length ; ++i) {
                    this.led[targetSegment.ledMember[i]].segmentId = 0
                }
                for (let i = 0 ; i < targetSegment.segmentMember.length ; ++i) {
                    this.segment[targetSegment.segmentMember[i]].parentId = 0
                }
                
                targetSegment.ledMember = []
                targetSegment.segmentMember = []

                // 遍历segmentChildMember, 即待处理的所有子组件
                for (let i = 0 ; i < this.segmentChildMember.length ; ++i) {
                    let element = this.segmentChildMember[i].split(':')     // element: [elementName, elementId]
                    if (element[0] == 'led') {
                        this.led[element[1]].segmentId = this.manageSegmentId
                        targetSegment.ledMember.push(element[1])
                    } else {
                        this.segment[element[1]].parentId = this.manageSegmentId
                        targetSegment.segmentMember.push(element[1])
                    }
                }

                let childLed = targetSegment.ledMember
                let childSegment = targetSegment.segmentMember
                let _this = this
                db.transaction(function (context) { 
                    context.executeSql('UPDATE LED SET segmentId = 0 WHERE segmentId = ?',[_this.manageSegmentId]);
                    context.executeSql('UPDATE Segment SET parentId = 0 WHERE parentId = ?', [_this.manageSegmentId])
                    for (let i = 0 ; i < childLed.length; ++i) {
                        context.executeSql('UPDATE LED SET segmentId = ? WHERE id = ?', [_this.manageSegmentId, childLed[i]])
                    }
                    for (let i = 0 ; i < childSegment.length; ++i) {
                        context.executeSql('UPDATE Segment SET parentId = ? WHERE id = ?', [_this.manageSegmentId, childSegment[i]])
                    }
                })

                this.$message({
                    message: 'Success!',
                    type: 'success',
                    duration: 1500
                });
            },
            // 在segment管理弹窗中的删除功能
            deleteSegment(id) {
                // 将孩子元素(led以及segment)的segmentParentId都置为默认值
                for (let i = 0 ; i < this.segment[id].ledMember.length ; ++i) {
                    let childLedId = this.segment[id].ledMember[i]
                    this.led[childLedId].segmentId = this.defaultSegmentId
                }
                
                for (let i = 0 ; i < this.segment[id].segmentMember.length ; ++i) {
                    let childSegmentId = this.segment[id].segmentMember[i]
                    this.segment[childSegmentId].parentId = this.defaultSegmentId
                }
                // 删除segment和segmentList中的对应项
                delete this.segment[id]
                for (let i = 0 ; i < this.segmentList.length ; ++i) {
                    if (this.segmentList[i].id == id) {
                        this.segmentList.splice(i, 1)
                        break
                    }
                }
                let _this = this
                // 同步数据库数据
                db.transaction(function (context) {
                    context.executeSql('UPDATE LED SET segmentId = ? WHERE segmentId = ?', [_this.defaultSegmentId, _this.manageSegmentId])
                    context.executeSql('UPDATE Segment SET parentId = ? WHERE parentId = ?', [_this.defaultSegmentId, _this.manageSegmentId])
                    context.executeSql('DELETE FROM Segment Where id = ?', [id]);
                });
                
                this.dialogDeleteSegmentVisible=false
                this.$message({
                    message: 'Success!',
                    type: 'success',
                    duration: 1500
                });
            },
            // 判断是否显示表单，并初始化表单绑定的数据
            ifGridDialogFormVisible(index) {
                // targetGrid读取gridElementOverlap对象，获得了选中网格中的所有组件。
                let targetGrid = this.gridElementOverlap[index]
                if (targetGrid == null) {
                    this.dialogFormVisible = false
                } else {
                    this.gridDialogIndex = index
                    // 初始化
                    this.targetGridElement = {
                        button: {},
                        led: {}
                    }
                    for (let i = 0 ; i < targetGrid['button'].length ; ++i) {
                        let targetButtonId = targetGrid['button'][i]
                        // 新增的对象默认是非响应式的，要用 this.$set() 其声明为响应式。
                        // 详见 https://cn.vuejs.org/v2/guide/reactivity.html
                        this.$set(this.targetGridElement['button'], targetButtonId, {})
                        let targetButton = this.targetGridElement['button'][targetButtonId]
                        this.$set(targetButton, 'name', this.button[targetButtonId].name)
                        this.$set(targetButton, 'hwId', this.button[targetButtonId].hwId)
                        this.$set(targetButton, 'parentId', this.button[targetButtonId].parentId)
                        this.$set(targetButton, 'toDelete', 0)
                        this.$set(targetButton, 'toAlterGroup', 0)
                    }
                    for (let i = 0 ; i < targetGrid['led'].length ; ++i) {
                        let targetLedId = targetGrid['led'][i]
                        // 与 button 同理
                        this.$set(this.targetGridElement['led'], targetLedId, {})
                        let targetLed = this.targetGridElement['led'][targetLedId]
                        this.$set(targetLed, 'name', this.led[targetLedId].name)
                        this.$set(targetLed, 'hwId', this.led[targetLedId].hwId)
                        this.$set(targetLed, 'segmentId', this.led[targetLedId].segmentId)
                        this.$set(targetLed, 'parentId', this.led[targetLedId].parentId)
                        this.$set(targetLed, 'toDelete', 0)
                        this.$set(targetLed, 'toAlterSegment', 0)
                        this.$set(targetLed, 'toAlterGroup', 0)
                    }
                    this.dialogFormVisible = true
                }
            }, 
            ifDialogSaveVisible() {
                if (this.templateId == 0) {
                    this.dialogSaveVisible = true
                } else {
                    this.$message({
                        message: 'Success!',
                        type: 'success',
                        duration: 1500
                    });
                }
            },
            // 提交表单触发事件
            submitGridDialogForm() {
                // 处理表单数据
                let targetButton = this.targetGridElement['button']
                db.transaction(function (context) { 
                    for (let buttonId in targetButton) {
                        if (targetButton[buttonId].toDelete==1) {
                            // 删除指定的button
                            context.executeSql('DELETE FROM Button WHERE id =?',[buttonId]);
                            
                        } else {
                            if (targetButton[buttonId].toAlterGroup == 1) {
                                context.executeSql('UPDATE Button SET parentId=? WHERE id=?',[targetButton[buttonId].parentId, buttonId]);
                            }
                        }
                        context.executeSql('UPDATE Button SET name=?,hwId=? WHERE id =?',[targetButton[buttonId].name,targetButton[buttonId].hwId,buttonId]);
                    }
                })

                let targetLed = this.targetGridElement['led']
                db.transaction(function (context) {
                    for (let ledId in targetLed) {
                        if (targetLed[ledId].toDelete==1) {
                            // 删除指定的led
                            context.executeSql('DELETE FROM LED WHERE id =?',[ledId]);
                        } else {
                            // 修改led信息
                            
                            if (targetLed[ledId].toAlterSegment == 1) {
                                // 更改led分组
                                context.executeSql('UPDATE LED SET segmentId=? WHERE id=?',[targetLed[ledId].segmentId, ledId]);
                            }

                            if (targetLed[ledId].toAlterGroup == 1) {
                                context.executeSql('UPDATE LED SET parentId=? WHERE id=?',[targetLed[ledId].parentId, ledId]);
                            }

                            context.executeSql('UPDATE LED SET name=?,hwId=?,segmentId=? WHERE id =?',[targetLed[ledId].name,targetLed[ledId].hwId,targetLed[ledId].segmentId,ledId]);
                        }
                    }
                })
                this.$message({
                    message: 'Success!',
                    type: 'success',
                    duration: 1500
                });
                this.loadBoard()
                this.dialogFormVisible = false
            },
            //点保存取名后确定
            submitTotalForm(){
                let name = this.templateName;
                let templateId = this.templateId;
                db.transaction(function (context) { 
                    //若是新设计，插入TemplateList后将templateId暂时等于0的元素均设置成当前插入templateId
                    context.executeSql('INSERT INTO TemplateList (name) VALUES (?)',[name]);
                    context.executeSql('SELECT MAX(tp.id) AS max_result FROM TemplateList as tp', [], function (context, results) {
                        if (results.rows.length > 0) {
                            templateId = results.rows.item(0).max_result;
                            context.executeSql('UPDATE Button SET templateId=? WHERE templateId =0',[templateId]);
                            context.executeSql('UPDATE LED SET templateId=? WHERE templateId =0',[templateId]);
                            context.executeSql('UPDATE Segment SET templateId=? WHERE templateId =0',[templateId]);
                            context.executeSql('UPDATE Groups SET templateId=? WHERE templateId =0',[templateId]);

                            location.reload();
                        }
                    });
                })     
                this.dialogSaveVisible = false;
            },
            // 当页面状态改变时调用该函数重新计算网格边长
            alterGridWidth() {
                this.totalWidth = document.getElementById("MainArea").offsetWidth;
                // 重新计算一个网格的边长
                this.gridWidth = this.totalWidth / (1+this.rowGridNum) - 0.01
                
                // 改变网格宽度
                for (let i = 0 ; i < this.rowGridNum * this.rowGridNum ; ++i) {
                    this.gridStyle[i].width = this.gridWidth + 'px'
                    this.gridStyle[i].height = this.gridWidth + 'px'
                }
            },
            // 放大页面
            zoomIn() {
                // 解析百分制的width参数
                let width = parseInt(this.workspaceDivClass.width.replace('%', ''))
                if (width >= 300) {
                    this.$message({
                        type: 'error',
                        duration: 1500,
                        message: 'Error!'
                    });
                } else {
                    width += 10
                    this.workspaceDivClass.width = width + '%'
                    // 在这里读取offsetWidth仍会得到旧值
                    // 所以选择在mount里定义监听事件而非显式调用alterGridWidth()函数
                }
            },
            // 缩小页面
            zoomOut() {
                // 解析百分制的width参数
                let width = parseInt(this.workspaceDivClass.width.replace('%', ''))
                if (width <= 30) {
                    this.$message({
                        type: 'error',
                        duration: 1500,
                        message: 'Error!'
                    });
                } else {
                    this.workspaceDivClass.width = width - 10 + '%'
                }
            },
        }
    }
</script>

<style scoped>
    #MainArea {
        height:730px;
        border:1px solid #aaaaaa;
        overflow-y:auto
    }
    .el-icon-zoom-in {
        font-size: 30px;
        cursor: pointer
    }
    .el-icon-zoom-out {
        font-size: 30px;
        cursor: pointer
    }
    #button {
        width: 88px;
        height:88px;
        /*border:1px solid #aaaaaa;*/
        background-color: #8CFE90;
        margin: 10px;
    }

    #LED {
        width: 38px;
        height:38px;
        /*border:1px solid #aaaaaa;*/
    }

    #segment {
        margin: 10px;
        width: 88px;
        height:150px;
        /*border:1px solid #aaaaaa;*/
    }

</style>
