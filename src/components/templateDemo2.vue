<template>
    <div>
        <el-col :span="2">
       
            <div draggable="true" id="button" @dragstart="dragElementType=1" @dragend="dragElementType=0">
                button
            </div>
            <div draggable="true" id="LED" @dragstart="dragElementType=2" @dragend="dragElementType=0">
                LED
            </div>
            <div draggable="true" id="segment" @dragstart="dragElementType=3" @dragend="dragElementType=0">
                segment
            </div>
        </el-col>
        
        <el-col :span="22" style="overflow-x:auto" v-loading="loading">
            <div :style="radioClass">
                <el-radio v-model="boardType" label="1" @change="loadBoard()">PS</el-radio>
                <el-radio v-model="boardType" label="2" @change="loadBoard()">OS</el-radio>
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

                <!-- 更改某一网格所有组件属性的表单 -->
                <el-dialog title="设置属性" :visible.sync="dialogFormVisible">
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
                                                <p>当前组件编号：{{buttonId}}</p>
                                            </el-col>
                                            <el-col :span="6">
                                                <el-button type="warning" icon="el-icon-delete" @click="selectButtonId=buttonId; 
                                                     dialogDeleteButtonVisible=true;" size="medium">删除</el-button>
                                            </el-col>
                                        </el-row>
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
                                                <p>当前组件编号：{{ledId}}</p>
                                            </el-col>
                                            <el-col :span="6" :offset="10">
                                                <el-button type="warning" icon="el-icon-delete" @click="selectLedId=ledId; 
                                                dialogDeleteLedVisible=true;" size="medium">删除</el-button>
                                            </el-col>
                                        </el-row>
                                        <el-row>
                                            <el-col :span="18">
                                                <p>SegmentGroup: {{segment[targetGridElement.led[ledId].segmentId].name}}</p>
                                            </el-col>
                                            <el-col :span="6">
                                                <el-button type="info" icon="el-icon-folder" @click="selectLedId=ledId; 
                                                    dialogSelectSegmentVisible=true;" size="medium">更改分组</el-button>
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
                        
                    <el-button type="primary" @click="submitGridDialogForm">确定</el-button>
                    <el-button @click="dialogFormVisible=false">取消</el-button>
                        
                    </el-form>
                </el-dialog>

                <!-- button删除提示 -->
                <el-dialog
                    title="提示"
                    :visible.sync="dialogDeleteButtonVisible">
                    <span>确认删除？</span>
                    <span slot="footer" class="dialog-footer">
                        <el-button type="primary" @click="targetGridElement.button[selectButtonId].toDelete=1; dialogDeleteButtonVisible=false;">确定</el-button>
                        <el-button @click="dialogDeleteButtonVisible=false">取消</el-button>
                    </span>
                </el-dialog>

                <!-- led删除提示 -->
                <el-dialog
                    title="提示"
                    :visible.sync="dialogDeleteLedVisible">
                    <span>确认删除？</span>
                    <span slot="footer" class="dialog-footer">
                        <el-button type="primary" @click="targetGridElement.led[selectLedId].toDelete=1; dialogDeleteLedVisible=false;">确定</el-button>
                        <el-button @click="dialogDeleteLedVisible=false">取消</el-button>
                    </span>
                </el-dialog>

                <!-- 下拉菜单选择led所在segment分组 -->
                <el-dialog title="选择segment分组" :visible.sync="dialogSelectSegmentVisible">
                    <el-select v-model="selectSegmentId" placeholder="请选择">
                        <el-option
                        v-for="(seg, segId) in segment"
                        :key="segId"
                        :label="seg.name"
                        :value="segId">
                        </el-option>
                    </el-select>
                    <el-button type="primary" @click="targetGridElement.led[selectLedId].segmentId=selectSegmentId;
                        targetGridElement.led[selectLedId].toAlterSegment=1; dialogSelectSegmentVisible=false;">确定</el-button>
                    <el-button @click="dialogSelectSegmentVisible=false">取消</el-button>
                </el-dialog>
                
                <!-- 保存按钮弹出窗口 -->
                <el-dialog title="请输入设计结果名称" :visible.sync="dialogSaveVisible">
                    <el-form :inline="true">
                        <el-form-item label="name">
                            <el-input v-model="templateName"></el-input>
                        </el-form-item>
                        <el-button type="primary" @click="submitTotalForm">确定</el-button>
                        <el-button @click="dialogSaveVisible=false">取消</el-button>
                    </el-form>
                </el-dialog>

                <el-dialog title="请输入导出路径" :visible.sync="dialogExportVisible">
                    <el-form :inline="true">
                        <el-form-item label="路径">
                            <el-input v-model="exportPath"></el-input>
                        </el-form-item>
                        <el-button type="primary" @click=" dialogExportVisible=false; testAxios();">确定</el-button>
                        <el-button @click="dialogExportVisible=false">取消</el-button>
                    </el-form>
                </el-dialog>
                <br />
                <el-button type="primary" @click="ifDialogSaveVisible"> 保存 </el-button>
                <el-button type="primary" @click="dialogExportVisible=true"> 导出 </el-button>
            </div>
        </el-col>
    </div>
</template>

<script>
    import axios from 'axios';
    var buttonCount = 1;
    var ledCount = 1;
    var segmentCount = 1;
    var db = openDatabase('BSHdb', '1.0', 'Test DB', 2 * 1024 * 1024);
    db.transaction(function (context) {
        context.executeSql('SELECT MAX(bt.id) AS max_result FROM Button as bt', [], function (context, results) {
            if (results.rows.length > 0) {
                buttonCount = results.rows.item(0).max_result+1;
            }
        });
        context.executeSql('SELECT MAX(led.id) AS max_result FROM LED as led', [], function (context, results) {
            if (results.rows.length > 0) {
                ledCount = results.rows.item(0).max_result+1;
            }
        });
        context.executeSql('SELECT MAX(seg.id) AS max_result FROM Segment as seg', [], function (context, results) {
            if (results.rows.length > 0) {
                segmentCount = results.rows.item(0).max_result+1;
            }
        });
    });
    
    export default {
        name: 'templateDemo2',
        data() {
            return {
                loading: false,
                exportPath:'',
                templateName:'',
                boardType: "1",
                //templateId：默认为0，保存后根据保存的id改变
                templateId:this.$route.params.templateId,
                defaultSegmentId: 0,
                dragElementType: 1,
                dialogExportVisible:false,
                //保存弹窗默认关闭
                dialogSaveVisible:false,
                // 确认删除弹窗
                dialogDeleteButtonVisible: false,
                dialogDeleteLedVisible: false,
                // 表单弹窗默认关闭
                dialogFormVisible: false,
                // 选择segment组别弹窗
                dialogSelectSegmentVisible: false,
                // 列举segment成员弹窗
                dialogSegmentMemberVisible: false,
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
                    }
                    ledId: {
                        name: 
                        hwId: 
                        segmentId: 
                    }
                    segment {
                        segmentId1: {
                            name: 
                            ledMember: [ledId1, ledId2, ...]
                        }
                    }
                */
                button: {},
                led: {},
                segment: {},

                
                // 在表单中对某一Button进行单独操作时更新该值
                selectButtonId: '',

                // 在表单中对某一led进行单独操作时更新该值
                selectLedId: '',

                // 和segment的下拉菜单选择项做双向绑定
                selectSegmentId: '',

                /* 
                    绑定选择led的segment group时下拉菜单的选项
                    {
                        segmentId: 
                        segmentName:
                    }
                */


                /*
                    描述：与gridElementOverlap不同，这里存了特定一个网格中的所有组件的信息
                    用法：与表单数据做双向绑定
                    targetGridElement {
                        button: {
                            buttonId1: {
                                name:
                                hwId:
                            },
                            ...
                        }
                        led: {
                            ledId1: {
                                name: 
                                hwId: 
                                segmentId:
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
                                    exportPath: _this.exportPath
                                }
                                }).then(function (response) {
                                    if (response.data == "SAVE SUCCESSFULLY") {
                                        _this.loading = false
                                        _this.$message({
                                            message: '导出成功',
                                            type: 'success'
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
                                    exportPath: _this.exportPath
                                }
                                }).then(function (response) {
                                    if (response.data == "SAVE SUCCESSFULLY") {
                                        _this.loading = false
                                        _this.$message({
                                            message: '导出成功',
                                            type: 'success'
                                        });
                                    }
                            });
                    });

                    // 提取PS以及OS板的led与segment的join数据，且按segmentId排序
                    context.executeSql('SELECT LED.name AS ledMemberName, Segment.name AS segmentName, Segment.id AS segmentId, LED.boardType AS boardType FROM LED \
                        INNER JOIN Segment ON LED.segmentId = Segment.id WHERE LED.templateId = ? ORDER BY segmentId ASC', [_this.templateId], function(context, results) {
                            let lastSegmentId = ''
                            for (let i = 0 ; i < results.rows.length ; ++i) {
                                if (results.rows[i].segmentName == 'default') {
                                    continue
                                } else {
                                    // 当一条数据的segmentId改变时，新增一个psSegmentList项
                                    // 由于results.rows所获取的数据已按segmentId排序，所以可以保证新增psSegmentList项时不重复
                                    if (results.rows[i].boardType == 1) {
                                        if (results.rows[i].segmentId != lastSegmentId) {
                                            psSegmentList.push({
                                                name: results.rows[i].segmentName,
                                                ledMemberName: [results.rows[i].ledMemberName]
                                            })
                                            lastSegmentId = results.rows[i].segmentId
                                        } else {
                                            psSegmentList[psSegmentList.length - 1].ledMemberName.push(results.rows[i].ledMemberName)
                                        }
                                    } else {
                                        if (results.rows[i].segmentId != lastSegmentId) {
                                            osSegmentList.push({
                                                name: results.rows[i].segmentName,
                                                ledMemberName: [results.rows[i].ledMemberName]
                                            })
                                            lastSegmentId = results.rows[i].segmentId
                                        } else {
                                            osSegmentList[osSegmentList.length - 1].ledMemberName.push(results.rows[i].ledMemberName)
                                        }
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
                                    exportPath: _this.exportPath
                                }
                                }).then(function (response) {
                                    if (response.data == "SAVE SUCCESSFULLY") {
                                        _this.loading = false
                                        _this.$message({
                                            message: '导出成功',
                                            type: 'success'
                                        });
                                    }
                            });
                    })

                    
                    
                });

               
            },
            pageInit(){
                this.button = {};
                this.led = {};
                this.segment={}
                this.gridElementOverlap = {};
                // this.targetGridElement = {
                //     button: {},
                //     led: {}
                // };
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
                            border: '1px solid',
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
                        // 读取Button数据
                        context.executeSql('SELECT * FROM Button WHERE templateId = ? and boardType = ?',[that.templateId,that.boardType],function(context,results){
                            let len = results.rows.length;
                            for(let i = 0;i<len;i++){
                                let index = results.rows.item(i).areaId
                                let elementId = results.rows.item(i).id
                                let name = results.rows.item(i).name
                                let hwId = results.rows.item(i).hwId
                                that.putButton(index, elementId, name, hwId, 0)
                            }
                        });
                        // 读取Segment数据
                        context.executeSql('SELECT * FROM Segment WHERE templateId = ? and boardType = ?',[that.templateId,that.boardType],function(context,results){
                            let len = results.rows.length;
                            // 若没有任何segment分组，则直接新建一个，名称固定为"default"
                            if (len == 0) {
                                that.defaultSegmentId = segmentCount
                                that.putSegment(that.defaultSegmentId, "default", 1)
                                segmentCount++
                            } else {
                                for(let i = 0;i<len;i++){
                                    let segmentId = results.rows.item(i).id
                                    let name = results.rows.item(i).name
                                    // 存储default分组的id值
                                    if (name == 'default') {
                                        that.defaultSegmentId = segmentId
                                    }
                                    that.putSegment(segmentId, name, 0)
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
                                that.putLed(index, elementId, name, hwId, segmentId, 0)
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
                switch (this.dragElementType) {
                    // 新建一个button
                    case 1:
                        this.putButton(index, buttonCount, '', '', 1)
                        buttonCount++
                        break
                    // 新建一个led
                    case 2:
                        this.putLed(index, ledCount, '', '', this.defaultSegmentId, 1)
                        ledCount++
                        break
                    // 新建一个segment并生成7个相应的led
                    case 3:
                        this.putSegment(segmentCount, "SegmentGroup" + segmentCount, 1)
                        rangeIndex = [index, index-1+this.rowGridNum, index+1+this.rowGridNum, index+2*this.rowGridNum,
                            index-1+3*this.rowGridNum, index+1+3*this.rowGridNum, index+4*this.rowGridNum]
                        for (let i = 0 ; i < rangeIndex.length ; ++i) {
                            this.putLed(rangeIndex[i], ledCount, '', '', segmentCount, 1)
                            ledCount++
                        }
                        segmentCount++
                        break
                    default:
                        console.log('other situations')
                        break
                }
            },
            putButton(index, elementId, _name, _hwId, isCreate) {
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
                    hwId: _hwId
                }

                if (isCreate == 1) {
                    db.transaction(function (context) {  
                        context.executeSql('INSERT INTO Button (id,areaId,templateId,boardType) VALUES (?,?,?,?)',[elementId,index,templateId,boardType]);
                    })
                }
            },
            putLed(index, elementId, _name, _hwId, _segmentId, isCreate) {
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
                }
                // 把新建led添加至给定的segment组别中
               
                this.segment[_segmentId].ledMember.push(elementId)

                //将LED插入数据库
                if (isCreate == 1) {
                    db.transaction(function (context) {  
                        context.executeSql('INSERT INTO LED (id,name,hwId,areaId,segmentId,templateId,boardType) VALUES (?,?,?,?,?,?,?)',[elementId,_name,_hwId,index,_segmentId,templateId,boardType]);
                    });
                }
            },
            putSegment(segmentId, _name, isCreate) {
                let templateId = this.templateId
                let boardType = this.boardType
                // 新建一个segment组别
                this.segment[segmentId] = {
                    name: _name,
                    ledMember: []
                }
                
                if (isCreate == 1) {
                    db.transaction(function (context) {  
                        context.executeSql('INSERT INTO Segment (id,name,templateId,boardType) VALUES (?,?,?,?)',[segmentId,_name,templateId,boardType]);
                    });
                }
                
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
                        this.$set(targetButton, 'toDelete', 0)
                    }
                    for (let i = 0 ; i < targetGrid['led'].length ; ++i) {
                        let targetLedId = targetGrid['led'][i]
                        // 与 button 同理
                        this.$set(this.targetGridElement['led'], targetLedId, {})
                        let targetLed = this.targetGridElement['led'][targetLedId]
                        this.$set(targetLed, 'name', this.led[targetLedId].name)
                        this.$set(targetLed, 'hwId', this.led[targetLedId].hwId)
                        this.$set(targetLed, 'segmentId', this.led[targetLedId].segmentId)
                        this.$set(targetLed, 'toDelete', 0)
                        this.$set(targetLed, 'toAlterSegment', 0)
                    }
                    this.dialogFormVisible = true
                }
            }, 
            ifDialogSaveVisible() {
                if (this.templateId == 0) {
                    this.dialogSaveVisible = true
                } else {
                    this.$message({
                        message: '已保存',
                        type: 'success',
                        duration: 1500
                    });
                }
            },
            // 提交表单触发事件
            submitGridDialogForm() {
                // 处理表单数据
                let targetButton = this.targetGridElement['button']
                for (let buttonId in targetButton) {
                    if (targetButton[buttonId].toDelete==1) {
                        // 删除指定的button
                        db.transaction(function (context) { 
                            context.executeSql('DELETE FROM Button WHERE id =?',[buttonId]);
                        });
                        
                    } else {
                        // 修改button信息
                        this.button[buttonId].name = targetButton[buttonId].name
                        this.button[buttonId].hwId = targetButton[buttonId].hwId
                        db.transaction(function (context) { 
                            context.executeSql('UPDATE Button SET name=?,hwId=? WHERE id =?',[targetButton[buttonId].name,targetButton[buttonId].hwId,buttonId]);
                        });
                    }
                }

                let targetLed = this.targetGridElement['led']
                for (let ledId in targetLed) {
                    if (targetLed[ledId].toDelete==1) {
                        // 删除指定的led
                        delete this.led[ledId]
                        db.transaction(function (context) { 
                            context.executeSql('DELETE FROM LED WHERE id =?',[ledId]);
                        });
                    } else {
                        // 修改led信息
                        this.led[ledId].name = targetLed[ledId].name
                        this.led[ledId].hwId = targetLed[ledId].hwId
                        
                        if (this.led[ledId].toAlterSegment == 1) {
                            // 更改led分组
                            let originSegmentId = this.led[ledId].segmentId
                            let segmentId = targetLed[ledId].segmentId
                            this.led[ledId].segmentId = segmentId
                            let originLedIdIndex = this.segment[originSegmentId].ledMember.indexOf(ledId)
                            this.segment[originSegmentId].ledMember.splice(originLedIdIndex, 1)
                            this.segment[segmentId].ledMember.push(ledId)
                            db.transaction(function (context) { 
                                context.executeSql('UPDATE LED SET segmentId=? WHERE id=?',[segmentId, ledId]);
                            })
                        }

                        db.transaction(function (context) {
                            context.executeSql('UPDATE LED SET name=?,hwId=?,segmentId=? WHERE id =?',[targetLed[ledId].name,targetLed[ledId].hwId,targetLed[ledId].segmentId,ledId]);
                        });
                    }
                }
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
                        message: '不可放大'
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
                        message: '不可缩小'
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
        border:1px solid #aaaaaa;
    }

    #LED {
        width: 38px;
        height:38px;
        border:1px solid #aaaaaa;
    }

    #segment {
        width: 88px;
        height:150px;
        border:1px solid #aaaaaa;
    }

</style>

