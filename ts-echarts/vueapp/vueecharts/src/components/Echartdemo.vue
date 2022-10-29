<!--
 * @Author: OCEAN.GZY
 * @Date: 2022-10-28 16:00:05
 * @LastEditors: OCEAN.GZY
 * @LastEditTime: 2022-10-28 19:58:18
 * @FilePath: /echarts/ts-echarts/vueapp/vueecharts/src/components/Echartdemo.vue
 * @Description: 注释信息
-->
<script setup lang="ts">
import * as echarts from 'echarts';
import { onMounted } from 'vue';
import data from "../assets/nutrients.json"
import data201604 from "../assets/ec-option-doc-statistics-201604.json"
import $ from 'jquery'
import axios from 'axios'
onMounted(() => {
    init_demo01()
    init_demo02()
})
var init_demo01 = () => {
    var ROOT_PATH = 'https://echarts.apache.org/examples';

    var chartDom = document.getElementById('demo01') as HTMLElement;

    var myChart = echarts.init(chartDom);
    var option;

    const indices = {
        name: 0,
        group: 1,
        id: 16
    };
    const schema = [
        { name: 'name', index: 0 },
        { name: 'group', index: 1 },
        { name: 'protein', index: 2 },
        { name: 'calcium', index: 3 },
        { name: 'sodium', index: 4 },
        { name: 'fiber', index: 5 },
        { name: 'vitaminc', index: 6 },
        { name: 'potassium', index: 7 },
        { name: 'carbohydrate', index: 8 },
        { name: 'sugars', index: 9 },
        { name: 'fat', index: 10 },
        { name: 'water', index: 11 },
        { name: 'calories', index: 12 },
        { name: 'saturated', index: 13 },
        { name: 'monounsat', index: 14 },
        { name: 'polyunsat', index: 15 },
        { name: 'id', index: 16 }
    ];
    const groupCategories: string[] = [];
    const groupColors: string[] = [];
    // $.get(ROOT_PATH + '/data/asset/data/nutrients.json', function (data: any) {
    //     normalizeData(data);
    //     myChart.setOption((option = getOption(data)));
    // });
    console.log(data)
    normalizeData(data);
    myChart.setOption((option = getOption(data)));
    function normalizeData(originData: any[]) {
        const groupMap: {} = {};
        originData.forEach(function (row: any[]) {
            const groupName = row[indices.group];
            if (!groupMap.hasOwnProperty(groupName)) {
                groupMap[groupName] = 1;
            }
        });
        originData.forEach(function (row: any[]) {
            row.forEach(function (item: string, index: number) {
                if (
                    index !== indices.name &&
                    index !== indices.group &&
                    index !== indices.id
                ) {
                    // Convert null to zero, as all of them under unit "g".
                    row[index] = parseFloat(item) || 0;
                }
            });
        });
        for (const groupName in groupMap) {
            if (groupMap.hasOwnProperty(groupName)) {
                groupCategories.push(groupName);
            }
        }
        const hStep = Math.round(300 / (groupCategories.length - 1));
        for (var i = 0; i < groupCategories.length; i++) {
            groupColors.push(echarts.color.modifyHSL('#5A94DF', hStep * i));
        }
    }
    function getOption(data: any) {
        const lineStyle = {
            width: 0.5,
            opacity: 0.05
        };
        return {
            backgroundColor: '#333',
            tooltip: {
                padding: 10,
                backgroundColor: '#222',
                borderColor: '#777',
                borderWidth: 1
            },
            title: [
                {
                    text: 'Groups',
                    top: 0,
                    left: 0,
                    textStyle: {
                        color: '#fff'
                    }
                }
            ],
            visualMap: {
                show: true,
                type: 'piecewise',
                categories: groupCategories,
                dimension: indices.group,
                inRange: {
                    color: groupColors //['#d94e5d','#eac736','#50a3ba']
                },
                outOfRange: {
                    color: ['#ccc'] //['#d94e5d','#eac736','#50a3ba']
                },
                top: 20,
                textStyle: {
                    color: '#fff'
                },
                realtime: false
            },
            parallelAxis: [
                { dim: 16, name: schema[16].name, scale: true, nameLocation: 'end' },
                { dim: 2, name: schema[2].name, nameLocation: 'end' },
                { dim: 4, name: schema[4].name, nameLocation: 'end' },
                { dim: 3, name: schema[3].name, nameLocation: 'end' },
                { dim: 5, name: schema[5].name, nameLocation: 'end' },
                { dim: 6, name: schema[6].name, nameLocation: 'end' },
                { dim: 7, name: schema[7].name, nameLocation: 'end' },
                { dim: 8, name: schema[8].name, nameLocation: 'end' },
                { dim: 9, name: schema[9].name, nameLocation: 'end' },
                { dim: 10, name: schema[10].name, nameLocation: 'end' },
                { dim: 11, name: schema[11].name, nameLocation: 'end' },
                { dim: 12, name: schema[12].name, nameLocation: 'end' },
                { dim: 13, name: schema[13].name, nameLocation: 'end' },
                { dim: 14, name: schema[14].name, nameLocation: 'end' },
                { dim: 15, name: schema[15].name, nameLocation: 'end' }
            ],
            parallel: {
                left: 280,
                top: 20,
                // top: 150,
                // height: 300,
                width: 400,
                layout: 'vertical',
                parallelAxisDefault: {
                    type: 'value',
                    name: 'nutrients',
                    nameLocation: 'end',
                    nameGap: 20,
                    nameTextStyle: {
                        color: '#fff',
                        fontSize: 14
                    },
                    axisLine: {
                        lineStyle: {
                            color: '#aaa'
                        }
                    },
                    axisTick: {
                        lineStyle: {
                            color: '#777'
                        }
                    },
                    splitLine: {
                        show: false
                    },
                    axisLabel: {
                        color: '#fff'
                    },
                    realtime: false
                }
            },
            animation: false,
            series: [
                {
                    name: 'nutrients',
                    type: 'parallel',
                    lineStyle: lineStyle,
                    inactiveOpacity: 0,
                    activeOpacity: 0.01,
                    progressive: 500,
                    smooth: true,
                    data: data
                }
            ]
        };
    }

    option && myChart.setOption(option);
    console.log("初始化了")
}

var init_demo02 = () => {
    var ROOT_PATH = 'https://echarts.apache.org/examples';
    var chartDom = document.getElementById('demo02')!;
    var myChart = echarts.init(chartDom);
    var option;

    type RawNode = {
        [key: string]: RawNode;
    } & {
        $count: number;
    };

    interface TreeNode {
        name: string;
        value: number;
        children?: TreeNode[];
    }

    const uploadedDataURL =
        ROOT_PATH + '/data/asset/data/ec-option-doc-statistics-201604.json';

    myChart.showLoading();

    // $.getJSON(uploadedDataURL, function (rawData) {
        myChart.hideLoading();

        function convert(source: any, target: any, basePath: string) {
            for (let key in source) {
                let path = basePath ? basePath + '.' + key : key;
                if (!key.match(/^\$/)) {
                    target.children = target.children || [];
                    const child = {
                        name: path
                    } as TreeNode;
                    target.children.push(child);
                    convert(source[key], child, path);
                }
            }

            if (!target.children) {
                target.value = source.$count || 1;
            } else {
                target.children.push({
                    name: basePath,
                    value: source.$count
                });
            }
        }

        const data = {
            children: [] as TreeNode[]
        } as TreeNode;

        convert(data201604, data, '');

        myChart.setOption(
            (option = {
                title: {
                    text: 'ECharts Options',
                    subtext: '2016/04',
                    left: 'leafDepth'
                },
                tooltip: {},
                series: [
                    {
                        name: 'option',
                        type: 'treemap',
                        visibleMin: 300,
                        data: data.children,
                        leafDepth: 2,
                        levels: [
                            {
                                itemStyle: {
                                    borderColor: '#555',
                                    borderWidth: 4,
                                    gapWidth: 4
                                }
                            },
                            {
                                colorSaturation: [0.3, 0.6],
                                itemStyle: {
                                    borderColorSaturation: 0.7,
                                    gapWidth: 2,
                                    borderWidth: 2
                                }
                            },
                            {
                                colorSaturation: [0.3, 0.5],
                                itemStyle: {
                                    borderColorSaturation: 0.6,
                                    gapWidth: 1
                                }
                            },
                            {
                                colorSaturation: [0.3, 0.5]
                            }
                        ]
                    }
                ]
            })
        );
    // })
    ;

    option && myChart.setOption(option);
}

</script>
<template>
    <div style="height:1400px;width:1024px;background-color:red">
        <div class="custom-test" id="demo01" style="width: 100%;height:100%"></div>
    </div>
    <div style="height:1400px;width:1024px;background-color:#f4f4f4">
        <div class="custom-test" id="demo02" style="width: 100%;height:100%"></div>
    </div>
</template>