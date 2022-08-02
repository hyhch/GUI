<template>
    <div>
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
        </el-col>
        
        <el-col :span="22" style="overflow-x:auto">
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
                    :key="index" :style="gridStyle[index-1]"
                    :id="index-1"
                    @dragenter="ondragenter" @dragover="ondragover($event)" 
                    @dragleave="ondragleave" @drop="ondrop($event, index, boardType)"
                    @click="ifDialogFormVisible(index)"
                ></div>

                <el-dialog title="设置属性" :visible.sync="dialogFormVisible">
                    <!-- 这里需要接受参数gridDialogIndex(点击的网格的index) -->
                    <el-form :inline="true">

                        <!-- 用键值对的方式遍历gridElementOverlap中的一个对象 -->
                        <!-- 根据gridElementOverlap的结构，第一个v-for遍历了一个网格中所有组件的类型 -->
                        <!-- elementList为某一类型的组件列表，elementName为组件名 -->
                        <div v-for="(elementList, elementName, index) in gridElementOverlap[this.gridDialogIndex-1]" :key="index">

                            <!-- v-if根据每个组件的类型（elementName的值）生成对应表单项。 -->
                            <div v-if="elementName=='button'">
                                <el-divider><h4>BUTTON</h4></el-divider>

                                <!-- 第二个v-for遍历了某一类型组件的组件列表（elementList） -->
                                <div v-for="(buttonId, index) in elementList" :key="index">
                                    <p>当前组件编号：{{buttonId}}</p>
                                    <el-form-item label="name">
                                        <el-input v-model="targetGridElement.button[buttonId].name"></el-input>
                                    </el-form-item>
                                    <el-form-item label="hwId">
                                        <el-input v-model="targetGridElement.button[buttonId].hwId"></el-input>
                                    </el-form-item>
                                </div>
                            </div>

                            <!-- 同Button -->
                            <div v-if="elementName=='led'">
                                <el-divider><h4>LED</h4></el-divider>
                                <div v-for="(ledId, index) in elementList" :key="index">
                                    <p>当前组件编号：{{ledId}}</p>
                                    <el-form-item label="name">
                                        <el-input v-model="targetGridElement.led[ledId].name"></el-input> 
                                    </el-form-item>
                                    <el-form-item label="hwId">
                                        <el-input v-model="targetGridElement.led[ledId].hwId"></el-input>
                                    </el-form-item>
                                </div>
                            </div>
                        </div>
                        
                    <el-button type="primary" @click="submitGridDialogForm">确定</el-button>
                    <el-button type="primary" @click="dialogFormVisible=false">取消</el-button>
                        
                    </el-form>
                </el-dialog>

                <br />
                <el-button type="primary" @click="testAxios"> 保存 </el-button>
            </div>
        </el-col>
    </div>
</template>

<script>
    import axios from 'axios';
    var elementCount = 1;
    var db = openDatabase('BSHdb', '1.0', 'Test DB', 2 * 1024 * 1024);
    db.transaction(function (context) {
            context.executeSql('SELECT MAX(MAX(bt.id),MAX(led.id)) AS max_result FROM Button as bt,LED as led', [], function (context, results) {
            elementCount = results.rows.item(0).max_result+1;
            console.log(elementCount);
        });
    });
    export default {
        name: 'templateDemo2',
        data() {
            return {

                boardType: "1",

                //弹窗默认关闭
                dialogFormVisible: false,
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
                    id: {
                        name: 
                        hwId: 
                    }
                */
                button: {},
                led: {},

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
            // 获取中间工作区的页面宽度
            this.totalWidth = document.getElementById("MainArea").offsetWidth;
            // 计算一个网格的边长, 减去0.01防止四舍五入后总宽度不够，在浏览器允许的缩放范围内这个误差够了
            // 1+this.rowGridNum用于抵消滚动条和一些玄学误差
            this.gridWidth = this.totalWidth / (1 + this.rowGridNum) - 0.01

            // 初始化所有网格的样式
            for (let i = 0 ; i < this.rowGridNum * this.rowGridNum ; ++i) {
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
        methods: {
            buttonDragstart() {
                let Btn = new Object;
                Btn.id = elementCount;
                Btn.name = "";
                Btn.hwId = "";
                //type属性：1=按钮；2=LED灯
                Btn.type = 1;
                elementCount++;
                sessionStorage.setItem("element", JSON.stringify(Btn));
            },
            ledDragstart() {
                let LED = new Object;
                LED.id = elementCount;
                LED.name = "";
                LED.hwId = "";
                LED.type = 2;
                elementCount++;
                sessionStorage.setItem("element", JSON.stringify(LED));
            },
            testAxios() {
                // 向后端传递组件参数
                let buttonList = []
                let ledList = []
                // 生成组件列表
                for (let buttonId in this.button) {
                    buttonList.push(this.button[buttonId])
                }
                for (let ledId in this.led) {
                    ledList.push(this.led[ledId])
                }
                axios ({
                    method: 'post',
                    url: '/save',
                    data: {
                        button: buttonList,
                        led: ledList
                    }
                    }).then(function (response) {
                        console.log(response.data)
                });
            },
            ondragenter() {
                //this.style.borderColor='red';
            },
            ondragover(event) {
                // this.style.borderColor='red';
                event.preventDefault()
            },
            ondragleave() {
                //this.style.borderColor='#aaaaaa'
            },
             ondrop(event, index,boardType)  {
                //this.style.borderColor='#aaaaaa';
                event.preventDefault()
                let data = JSON.parse(sessionStorage.getItem("element"));
                //this.style.borderColor='#aaaaaa';
                console.log(data);
                switch(data.type){
                    // 放置一个Button
                    case 1:{
                        db.transaction(function (context) {  
                            context.executeSql('INSERT INTO Button (id,areaId,templateId,boardType) VALUES (?,?,?,?)',[data.id,index-1,0,boardType]);
                        })
                        // button覆盖的index
                        let rangeIndex = [index-1, index, index+this.rowGridNum-1, index+this.rowGridNum]
                        for(let i = 0 ; i < rangeIndex.length ; ++i) {
                            // 如果覆盖位置上已存在其它组件，就将背景颜色设为mixColor，否则设为buttonColor
                            let curBackground = this.gridStyle[rangeIndex[i]].background
                            if (curBackground == this.backgroundColor) {
                                this.gridStyle[rangeIndex[i]].background = this.buttonColor
                            } else {
                                this.gridStyle[rangeIndex[i]].background = this.mixColor
                            }
                            this.gridStyle[rangeIndex[i]].cursor="pointer"

                            if (this.gridElementOverlap[rangeIndex[i]] == null) {
                                this.gridElementOverlap[rangeIndex[i]] = {
                                    button: [],
                                    led: []
                                }
                            }
                            this.gridElementOverlap[rangeIndex[i]]['button'].push(
                                data.id
                            )
                        }

                        //已有元素在上面的格子变成可点击的手型 
                        this.button[data.id] = {
                            name: data.name,
                            hwId: data.hwId
                        }
                        
                        break;
                    }
                    // 放置一个led
                    case 2:{
                        //将LED插入数据库
                        db.transaction(function (context) {  
                            context.executeSql('INSERT INTO LED (id,areaId,templateId,boardType) VALUES (?,?,?,?)',[data.id,index-1,0,boardType]);
                        });
                        this.gridStyle[index-1].cursor = "pointer";
                        document.getElementById(index-1).innerHTML = "<div style = 'background-color:#409EFF;width:50%;height:50%;margin:auto'></div>";
                        //拖动按钮松开后蓝色代表LED
                        if (this.gridElementOverlap[index-1] == null) {
                            this.gridElementOverlap[index-1] = {
                                button: [],
                                led: []
                            }
                        }
                        this.gridElementOverlap[index-1]['led'].push(
                            data.id
                        )

                        this.led[data.id] = {
                            name: data.name,
                            hwId: data.hwId
                        }
                        break;
                    }
                }
            },
            // 判断是否显示表单，并初始化表单绑定的数据
            ifDialogFormVisible(index) {
                // targetGrid读取gridElementOverlap对象，获得了选中网格中的所有组件。
                let targetGrid = this.gridElementOverlap[index-1]
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
                        // this.targetGridElement['button'][targetButtonId] = {
                        //      name: this.button[targetButtonId].name,
                        //      hwId: this.button[targetButtonId].hwId
                        // }
                    }
                    for (let i = 0 ; i < targetGrid['led'].length ; ++i) {
                        let targetLedId = targetGrid['led'][i]
                        // 与 button 同理
                        this.$set(this.targetGridElement['led'], targetLedId, {})
                        let targetLed = this.targetGridElement['led'][targetLedId]
                        this.$set(targetLed, 'name', this.led[targetLedId].name)
                        this.$set(targetLed, 'hwId', this.led[targetLedId].hwId)
                        // this.targetGridElement['led'][targetLedId] = {
                        //     name: this.led[targetLedId].name,
                        //     hwId: this.led[targetLedId].hwId
                        // }
                    }
                    this.dialogFormVisible = true
                }
            },
            // 提交表单触发事件
            submitGridDialogForm() {
                
                // 存储表单数据
                let targetButton = this.targetGridElement['button']
                for (let buttonId in targetButton) {
                    this.button[buttonId].name = targetButton[buttonId].name
                    this.button[buttonId].hwId = targetButton[buttonId].hwId
                    db.transaction(function (context) { 
                        context.executeSql('UPDATE Button SET name=?,hwid=? WHERE id =?',[targetButton[buttonId].name,targetButton[buttonId].hwId,buttonId]);
                    });
                }

                let targetLed = this.targetGridElement['led']
                for (let ledId in targetLed) {
                    this.led[ledId].name = targetLed[ledId].name
                    this.led[ledId].hwId = targetLed[ledId].hwId
                    db.transaction(function (context) {
                        context.executeSql('UPDATE LED SET name=?,hwid=? WHERE id =?',[targetLed[ledId].name,targetLed[ledId].hwId,ledId]);
                    });
                }

                this.dialogFormVisible = false
            },
            loadBoard(){

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

</style>
