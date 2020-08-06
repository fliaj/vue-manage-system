<template>
    <div>
        <el-row :gutter="20">

                <div ref="chart" style="width:100%;height:600px"></div>
         
        </el-row>
        <!-- <el-row :gutter="20">
            <el-col :span="12">
                <el-card shadow="hover">
                    <schart ref="bar" class="schart" canvasId="bar" :options="options"></schart>
                </el-card>
            </el-col>
            <el-col :span="12">
                <el-card shadow="hover">
                    <schart ref="line" class="schart" canvasId="line" :options="options2"></schart>
                </el-card>
            </el-col>
        </el-row> -->
    </div>
</template>

<script>
import Schart from 'vue-schart';
import bus from '../common/bus';
export default {
    name: 'dashboard',
    // data() {
    //     return {
    //         name: localStorage.getItem('ms_username'),
    //         options: {
    //             type: 'bar',
    //             title: {
    //                 text: '最近一周各品类销售图'
    //             },
    //             xRorate: 25,
    //             labels: ['周一', '周二', '周三', '周四', '周五'],
    //             datasets: [
    //                 {
    //                     label: '家电',
    //                     data: [234, 278, 270, 190, 230]
    //                 },
    //                 {
    //                     label: '百货',
    //                     data: [164, 178, 190, 135, 160]
    //                 },
    //                 {
    //                     label: '食品',
    //                     data: [144, 198, 150, 235, 120]
    //                 }
    //             ]
    //         },
    //         options2: {
    //             type: 'line',
    //             title: {
    //                 text: '最近几个月各品类销售趋势图'
    //             },
    //             labels: ['6月', '7月', '8月', '9月', '10月'],
    //             datasets: [
    //                 {
    //                     label: '家电',
    //                     data: [234, 278, 270, 190, 230]
    //                 },
    //                 {
    //                     label: '百货',
    //                     data: [164, 178, 150, 135, 160]
    //                 },
    //                 {
    //                     label: '食品',
    //                     data: [74, 118, 200, 235, 90]
    //                 }
    //             ]
    //         }
    //     };
    // },
    // components: {
    //     Schart
    // },
    computed: {
        role() {
            return this.name === 'admin' ? '超级管理员' : '普通用户';
        }
    },
    mounted(){
        this.getEchartData()  
    },
    methods: {
        getEchartData() {
            const chart = this.$refs.chart
            if (chart) {
            const myChart = this.$echarts.init(chart)
            var baseName = "Store Warehouse";
            var chartData = {
                'Remove From Warehouse': ['Fee1', 'Fee2', 'Fee3'],
                'Outbound To Customer': ['1', '2', '3'],
                'Return to Warehouse': ['F1', 'F2', 'F3']
            };
            var datas = [{
                name: baseName || '',
                draggable: true
            }];
            var lines = [];
            var categoryIdx = 0;
            var keyIndex = 0;
            var dataIndex = 0;
            for(var key in chartData) {
                keyIndex = dataIndex;
                datas.push({
                    name: key,
                    category: categoryIdx,
                    draggable: true
                });
                keyIndex++;
                dataIndex++;
                lines.push({
                    source: 0,
                    target: keyIndex,
                    value: ''
                });
                for(var idx in chartData[key]) {
                    datas.push({
                        name: chartData[key][idx],
                        category: categoryIdx,
                        draggable: true
                    });
                    dataIndex++;
                    lines.push({
                        source: keyIndex,
                        target: dataIndex,
                        value: ''
                    });
                }
                categoryIdx++;
            };
            const option = {
                title: {
                    text: ''
                },
                tooltip: {},
                animationDurationUpdate: 1500,
                label: {
                    normal: {
                        show: true,
                        textStyle: {
                            fontSize: 12
                        },
                    }
                },
                series: [{
                    type: 'graph',
                    layout: 'force', //采用力引导布局
                    symbolSize: 45,
                    legendHoverLink: true, //启用图例 hover 时的联动高亮。
                    focusNodeAdjacency: true, //在鼠标移到节点上的时候突出显示节点以及节点的边和邻接节点。
                    roam: true,
                    label: {
                        normal: {
                            show: true,
                            position: 'inside',
                            textStyle: {
                                fontSize: 12
                            },
                        }
                    },
                    force: {
                        repulsion: 1000
                    },
                    edgeSymbolSize: [4, 50],
                    edgeLabel: {
                        normal: {
                            show: true,
                            textStyle: {
                                fontSize: 10
                            },
                            formatter: "{c}"
                        }
                    },
                    categories: [{
                        itemStyle: {
                            normal: {
                                color: "#BB8FCE",
                            }
                        }
                    }, {
                        itemStyle: {
                            normal: {
                                color: "#0099FF",
                            }
                        }
                    }, {
                        itemStyle: {
                            normal: {
                                color: "#5DADE2",
                            }
                        }
                    }],
                    data: datas,
                    links: lines,
                    lineStyle: {
                        normal: {
                            opacity: 0.9,
                            width: 1,
                            curveness: 0
                        }
                    }
                }]                
            }
            myChart.setOption(option)
            window.addEventListener("resize", function() {
                myChart.resize()
            })
        }
        this.$on('hook:destroyed',()=>{
                window.removeEventListener("resize", function() {
                    myChart.resize();
                });
            })
        }
    }
};
</script>


<style scoped>
.el-row {
    margin-bottom: 20px;
}

.grid-content {
    display: flex;
    align-items: center;
    height: 100px;
}

.grid-cont-right {
    flex: 1;
    text-align: center;
    font-size: 14px;
    color: #999;
}

.grid-num {
    font-size: 30px;
    font-weight: bold;
}

.grid-con-icon {
    font-size: 50px;
    width: 100px;
    height: 100px;
    text-align: center;
    line-height: 100px;
    color: #fff;
}

.grid-con-1 .grid-con-icon {
    background: rgb(45, 140, 240);
}

.grid-con-1 .grid-num {
    color: rgb(45, 140, 240);
}

.grid-con-2 .grid-con-icon {
    background: rgb(100, 213, 114);
}

.grid-con-2 .grid-num {
    color: rgb(45, 140, 240);
}

.grid-con-3 .grid-con-icon {
    background: rgb(242, 94, 67);
}

.grid-con-3 .grid-num {
    color: rgb(242, 94, 67);
}

.user-info {
    display: flex;
    align-items: center;
    padding-bottom: 20px;
    border-bottom: 2px solid #ccc;
    margin-bottom: 20px;
}

.user-avator {
    width: 120px;
    height: 120px;
    border-radius: 50%;
}

.user-info-cont {
    padding-left: 50px;
    flex: 1;
    font-size: 14px;
    color: #999;
}

.user-info-cont div:first-child {
    font-size: 30px;
    color: #222;
}

.user-info-list {
    font-size: 14px;
    color: #999;
    line-height: 25px;
}

.user-info-list span {
    margin-left: 70px;
}

.mgb20 {
    margin-bottom: 20px;
}

.todo-item {
    font-size: 14px;
}

.todo-item-del {
    text-decoration: line-through;
    color: #999;
}

.schart {
    width: 100%;
    height: 300px;
}

.center {
    margin: 0 auto;
    width: 500px;
}

</style>
