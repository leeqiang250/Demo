<template>
    <div class="comp-kline">
        <div class="period-bar">
            <span class="period-item" :class="{'period-item--active':period==='min5'}" @click="changePeriod('min5')">5 Min</span>
            <span class="period-item" :class="{'period-item--active':period==='min15'}" @click="changePeriod('min15')">15 Min</span>
            <span class="period-item" :class="{'period-item--active':period==='min30'}" @click="changePeriod('min30')">30 Min</span>
            <span class="period-item" :class="{'period-item--active':period==='h1'}" @click="changePeriod('h1')">1 Hr</span>
            <span class="period-item" :class="{'period-item--active':period==='d1'}" @click="changePeriod('d1')">1 D</span>
            <span class="period-item" :class="{'period-item--active':period==='w1'}" @click="changePeriod('w1')">1 W</span>
        </div>
        <div  id="linechart" style="width: 100%;height: 6rem"></div>
    </div>
</template>

<script type="text/babel">
    export default {
        data(){
            return {
                period :'min5'
            }
        },
        mounted(){
            let linechart = document.querySelector('#linechart');
            this.myChart = echarts.init(linechart);
            this.drawGrap();
        },
        created() {

        },
        components: {
        },

        methods: {


            changePeriod(val){
                this.period = val;
                this.drawGrap();
            },

            splitData(rawData) {
                var categoryData = [];
                var values = [];
                var volumes = [];
                for (var i = 0; i < rawData.length; i++) {
                    categoryData.push(rawData[i].splice(0, 1)[0]);
                    values.push(rawData[i]);
                    volumes.push([i, rawData[i][4], rawData[i][0] > rawData[i][1] ? 1 : -1]);
                }

                return {
                    categoryData: categoryData,
                    values: values,
                    volumes: volumes
                };

            },

            calculateMA(dayCount, data) {
                var result = [];
                for (var i = 0, len = data.values.length; i < len; i++) {
                    if (i < dayCount) {
                        result.push('-');
                        continue;
                    }
                    var sum = 0;
                    for (var j = 0; j < dayCount; j++) {
                        sum += data.values[i - j][1];
                    }
                    result.push(+(sum / dayCount).toFixed(3));
                }
                return result;
            },

            drawGrap(){

                var upColor = '#75aa3d';
                var downColor = '#f13e79';

                let temp = {
                    min5: [ ["2016-04-21", 18092.84, 17982.52, 17963.89, 18107.29, 102720000], ["2016-04-22", 17987.38, 17990.32, 17934.17, 18043.77, 92570000], ["2016-04-25", 17990.94, 17977.24, 17855.55, 17990.94, 83770000], ["2016-04-26", 17987.38, 17990.32, 17934.17, 18043.77, 92570000], ["2016-04-27", 17996.14, 18041.55, 17920.26, 18084.66, 109090000], ["2016-04-28", 18023.88, 17830.76, 17796.55, 18035.73, 100920000], ["2016-04-29", 17813.09, 17773.64, 17651.98, 17814.83, 136070000], ["2016-05-02", 17783.78, 17891.16, 17773.71, 17912.35, 80100000], ["2016-05-03", 17870.75, 17750.91, 17670.88, 17870.75, 97060000], ["2016-05-04", 17735.02, 17651.26, 17609.01, 17738.06, 95020000], ["2016-05-05", 17664.48, 17660.71, 17615.82, 17736.11, 81530000], ["2016-05-06", 17650.3, 17740.63, 17580.38, 17744.54, 80020000], ["2016-05-09", 17743.85, 17705.91, 17668.38, 17783.16, 85590000], ["2016-05-10", 17726.66, 17928.35, 17726.66, 17934.61, 75790000], ["2016-05-11", 17919.03, 17711.12, 17711.05, 17919.03, 87390000], ["2016-05-12", 17711.12, 17720.5, 17625.38, 17798.19, 88560000], ["2016-05-13", 17711.12, 17535.32, 17512.48, 17734.74, 86640000], ["2016-05-16", 17531.76, 17710.71, 17531.76, 17755.8, 88440000], ["2016-05-17", 17701.46, 17529.98, 17469.92, 17701.46, 103260000], ["2016-05-18", 17501.28, 17526.62, 17418.21, 17636.22, 79120000], ["2016-05-19", 17514.16, 17435.4, 17331.07, 17514.16, 95530000], ["2016-05-20", 17437.32, 17500.94, 17437.32, 17571.75, 111990000], ["2016-05-23", 17507.04, 17492.93, 17480.05, 17550.7, 87790000], ["2016-05-24", 17525.19, 17706.05, 17525.19, 17742.59, 86480000], ["2016-05-25", 17735.09, 17851.51, 17735.09, 17891.71, 79180000], ["2016-05-26", 17859.52, 17828.29, 17803.82, 17888.66, 68940000], ["2016-05-27", 17826.85, 17873.22, 17824.73, 17873.22, 73190000], ["2016-05-31", 17789.05, 17838.56, 17703.55, 17838.56, 75560000], ["2016-06-01", 17754.55, 17789.67, 17664.79, 17809.18, 78530000], ["2016-06-02", 17789.05, 17838.56, 17703.55, 17838.56, 75560000], ["2016-06-03", 17799.8, 17807.06, 17689.68, 17833.17, 82270000], ["2016-06-06", 17825.69, 17920.33, 17822.81, 17949.68, 71870000], ["2016-06-07", 17936.22, 17938.28, 17936.22, 18003.23, 78750000], ["2016-06-08",17938.82, 17865.34, 17812.34, 17938.82, 90540000], ["2016-06-09", 17969.98, 17985.19, 17915.88, 18005.22, 69690000], ["2016-06-10", 17938.82, 17865.34, 17812.34, 17938.82, 90540000], ["2016-06-13", 17830.5, 17732.48, 17731.35, 17893.28, 101690000], ["2016-06-14", 17710.77, 17674.82, 17595.79, 17733.92, 93740000], ["2016-06-15", 17703.65, 17640.17, 17629.01, 17762.96, 94130000], ["2016-06-16", 17602.23, 17733.1, 17471.29, 17754.91, 91950000], ["2016-06-17", 17733.44, 17675.16, 17602.78, 17733.44, 248680000], ["2016-06-20", 17736.87, 17804.87, 17736.87, 17946.36, 99380000], ["2016-06-21", 17827.33, 17829.73, 17799.8, 17877.84, 85130000], ["2016-06-22", 17832.67, 17780.83, 17770.36, 17920.16, 89440000]],

                    min15: [ ["2016-04-21", 18092.84, 17982.52, 17963.89, 18107.29, 102720000], ["2016-04-22", 17985.05, 18003.75, 17909.89, 18026.85, 134120000], ["2016-04-25", 17990.94, 17977.24, 17855.55, 17990.94, 80770000], ["2016-04-26", 17987.38, 17990.32, 17934.17, 18043.77, 92570000], ["2016-04-27", 17996.14, 18041.55, 17920.26, 18084.66, 109090000], ["2016-04-28", 18023.88, 17830.76, 17796.55, 18035.73, 100920000], ["2016-04-29", 18092.84, 17982.52, 17963.89, 18107.29, 102720000], ["2016-05-02", 17735.02, 17651.26, 17609.01, 17738.06, 90020000], ["2016-05-03", 17870.75, 17750.91, 17670.88, 17870.75, 97060000], ["2016-05-04", 17735.02, 17651.26, 17609.01, 17738.06, 90020000], ["2016-05-05", 17664.48, 17660.71, 17615.82, 17736.11, 81530000], ["2016-05-06", 17650.3, 17740.63, 17580.38, 17744.54, 80020000], ["2016-05-09", 17743.85, 17705.91, 17668.38, 17783.16, 85590000], ["2016-05-10", 17726.66, 17908.35, 17726.66, 17934.61, 75790000], ["2016-05-11", 17919.03, 17711.12, 17711.05, 17919.03, 87390000], ["2016-05-12", 17711.12, 17720.5, 17625.38, 17798.19, 88560000], ["2016-05-13", 18119.03, 17711.12, 17711.05, 17919.03, 87390000], ["2016-05-16", 17531.76, 17710.71, 17531.76, 17755.8, 88440000], ["2016-05-17", 17701.46, 17529.98, 17469.92, 17701.46, 103260000], ["2016-05-18", 17501.28, 17526.62, 17418.21, 17636.22, 79120000], ["2016-05-19", 17514.16, 17435.4, 17431.07, 17514.16, 95530000], ["2016-05-20", 17701.46, 17529.98, 17469.92, 17701.46, 103260000], ["2016-05-23", 17507.04, 17492.93, 17480.05, 17550.7, 87790000], ["2016-05-24",17826.85, 17873.22, 17824.73, 17873.22, 73190000], ["2016-05-25", 17735.09, 17851.51, 17735.09, 17891.71, 79180000], ["2016-05-26",  17525.19, 17706.05, 17525.19, 17742.59, 86480000], ["2016-05-27", 17826.85, 17873.22, 17824.73, 17873.22, 73190000], ["2016-05-31", 17891.5, 17787.2, 17724.03, 17899.24, 147390000], ["2016-06-01", 17754.55, 17789.67, 17664.79, 17809.18, 78530000], ["2016-06-02",17938.82, 17865.34, 17812.34, 17938.82, 90540000], ["2016-06-03", 17799.8, 17807.06, 17689.68, 17833.17, 82270000], ["2016-06-06", 17825.69, 17920.33, 17822.81, 17949.68, 71870000], ["2016-06-07", 17936.22, 17938.28, 17936.22, 18003.23, 78750000], ["2016-06-08", 17825.69, 17920.33, 17822.81, 17949.68, 71870000], ["2016-06-09", 17969.98, 17985.19, 17915.88, 18005.22, 69690000], ["2016-06-10", 17938.82, 17865.34, 17812.34, 17938.82, 90540000], ["2016-06-13", 17830.5, 17732.48, 17731.35, 17893.28, 101690000], ["2016-06-14", 17938.82, 17865.34, 17812.34, 17938.82, 90540000], ["2016-06-15", 17703.65, 17640.17, 17629.01, 17762.96, 94130000], ["2016-06-16", 17602.23, 17733.1, 17471.29, 17754.91, 91950000], ["2016-06-17", 17733.44, 17675.16, 17602.78, 17733.44, 248680000], ["2016-06-20", 17736.87, 17804.87, 17736.87, 17946.36, 99380000], ["2016-06-21", 17827.33, 17829.73, 17799.8, 17877.84, 85130000], ["2016-06-22", 17832.67, 17780.83, 17770.36, 17920.16, 89440000]],

                    min30: [ ["2016-04-21", 18002.84, 17982.52, 17963.89, 18107.29, 102720000], ["2016-04-22", 17985.05, 18003.75, 17909.89, 18026.85, 134120000], ["2016-04-25", 17990.94, 17977.24, 17855.55, 17990.94, 83770000], ["2016-04-26", 17987.38, 17990.32, 17934.17, 18043.77, 92570000], ["2016-04-27", 17996.14, 18041.55, 17920.26, 18084.66, 109090000], ["2016-04-28", 18023.88, 17830.76, 17796.55, 18035.73, 100920000], ["2016-04-29", 17813.09, 17773.64, 17651.98, 17814.83, 136670000], ["2016-05-02", 17783.78, 17891.16, 17773.71, 17912.35, 80100000], ["2016-05-03", 17870.75, 17750.91, 17670.88, 17870.75, 97060000], ["2016-05-04", 17735.02, 17651.26, 17609.01, 17738.06, 95020000], ["2016-05-05", 17743.85, 17705.91, 17668.38, 17783.16, 85590000], ["2016-05-06", 17783.78, 17891.16, 17773.71, 17912.35, 80100000], ["2016-05-09", 17743.85, 17705.91, 17668.38, 17783.16, 85590000], ["2016-05-10", 17726.66, 17928.35, 17726.66, 17934.61, 75790000], ["2016-05-11", 17919.03, 17711.12, 17711.05, 17919.03, 87390000], ["2016-05-12", 17711.12, 17720.5, 17625.38, 17798.19, 88560000], ["2016-05-13", 17711.12, 17535.32, 17512.48, 17734.74, 86640000], ["2016-05-16", 17531.76, 17710.71, 17531.76, 17755.8, 88440000], ["2016-05-17", 17701.46, 17529.98, 17469.92, 17701.46, 103260000], ["2016-05-18", 17501.28, 17826.62, 17418.21, 17636.22, 79120000], ["2016-05-19", 17514.16, 17435.4, 17331.07, 17514.16, 95500], ["2016-05-20", 17437.32, 17500.94, 17437.32, 17571.75, 111990000], ["2016-05-23", 17507.04, 17492.93, 17480.05, 17550.7, 87790000], ["2016-05-24", 17525.19, 17706.05, 17525.19, 17742.59, 86480000], ["2016-05-25", 17735.09, 17851.51, 17735.09, 17891.71, 79180000], ["2016-05-26", 17859.52, 17828.29, 17803.82, 17888.66, 68940000], ["2016-05-27", 17826.85, 17873.22, 17824.73, 17873.22, 73190000], ["2016-05-31", 17891.5, 17787.2, 17724.03, 17899.24, 147390000], ["2016-06-01", 17754.55, 17789.67, 17664.79, 17809.18, 78530000], ["2016-06-02", 17825.69, 17920.33, 17822.81, 17949.68, 71870000], ["2016-06-03", 17799.8, 17807.06, 17689.68, 17833.17, 82270000], ["2016-06-06", 17825.69, 17920.33, 17822.81, 17949.68, 71870000], ["2016-06-07", 17936.22, 17938.28, 17936.22, 18003.23, 78750000], ["2016-06-08", 17931.91, 18005.05, 17931.91, 18016, 71260000], ["2016-06-09", 17969.98, 17985.19, 17915.88, 18005.22, 69690000], ["2016-06-10", 17938.82, 17865.34, 17812.34, 17938.82, 90540000], ["2016-06-13", 17830.5, 17732.48, 17731.35, 17893.28, 101690000], ["2016-06-14", 17710.77, 17674.82, 17595.79, 17733.92, 93740000], ["2016-06-15", 17703.65, 17640.17, 17629.01, 17762.96, 94130000], ["2016-06-16", 17602.23, 17733.1, 17471.29, 17754.91, 91950000], ["2016-06-17", 17733.44, 17675.16, 17602.78, 17733.44, 248680000], ["2016-06-20", 17602.23, 17733.1, 17471.29, 17754.91, 91950000], ["2016-06-21", 17827.33, 17829.73, 17799.8, 17877.84, 85130000], ["2016-06-22", 17827.33, 17829.73, 17799.8, 17877.84, 85130000]],

                    d1: [ ["2016-04-21", 18092.84, 17982.52, 17963.89, 18107.29, 102720000], ["2016-04-22", 17985.05, 18003.75, 17909.89, 18026.85, 134120000], ["2016-04-25", 17990.94, 17977.24, 17855.55, 17990.94, 83770000], ["2016-04-26", 17987.38, 17990.32, 17934.17, 18043.77, 92570000], ["2016-04-27", 17996.14, 18041.55, 17920.26, 18084.66, 109090000], ["2016-04-28", 18023.88, 17830.76, 17796.55, 18035.73, 100920000], ["2016-04-29", 17813.09, 17773.64, 17651.98, 17814.83, 136070000], ["2016-05-02", 17783.78, 17891.16, 17773.71, 17912.35, 80100000], ["2016-05-03", 17870.75, 17750.91, 17610.88, 17870.75, 97060000], ["2016-05-04", 17735.02, 17651.26, 17609.01, 17738.06, 95020000], ["2016-05-05", 17664.48, 17660.71, 17615.82, 17736.11, 81530000], ["2016-05-06", 17650.3, 17740.63, 17580.38, 17744.54, 80021000], ["2016-05-09", 17743.85, 17705.91, 17668.38, 17783.16, 85590000], ["2016-05-10", 17726.66, 17928.35, 17726.66, 17534.61, 75790000], ["2016-05-11", 17919.03, 17711.12, 17711.05, 17600.03, 87390000], ["2016-05-12", 17711.12, 17920.5, 17625.38, 17798.19, 88520000], ["2016-05-13", 17711.12, 17535.32, 17512.48, 17734.74, 86640000], ["2016-05-16", 17531.76, 17810.71, 17531.76, 17755.8, 88400000], ["2016-05-17", 17701.46, 17529.98, 17469.92, 17701.46, 103267000], ["2016-05-18", 17501.28, 17526.62, 17418.21, 17636.22, 79120000], ["2016-05-19", 17514.16, 17435.4, 17331.07, 17514.16, 95530000], ["2016-05-20", 17437.32, 17500.94, 17437.32, 17571.75, 111990000], ["2016-05-23", 17507.04, 17492.93, 17480.05, 17550.7, 87790000], ["2016-05-24", 17525.19, 17706.05, 17525.19, 17742.59, 86480000], ["2016-05-25", 17826.85, 17873.22, 17824.73, 17873.22, 73190000], ["2016-05-26", 17859.52, 17828.29, 17803.82, 17888.66, 68940000], ["2016-05-27", 17826.85, 17873.22, 17824.73, 17873.22, 73190000], ["2016-05-31", 17891.5, 17787.2, 17724.03, 17899.24, 147390000], ["2016-06-01", 17754.55, 17789.67, 17664.79, 17809.18, 78530000], ["2016-06-02", 17789.05, 17838.56, 17703.55, 17838.56, 75560000], ["2016-06-03", 17799.8, 17807.06, 17689.68, 17833.17, 82270000], ["2016-06-06", 17825.69, 17920.33, 17822.81, 17949.68, 71870000], ["2016-06-07", 17936.22, 17938.28, 17936.22, 18003.23, 78750000], ["2016-06-08",17703.65, 17640.17, 17629.01, 17762.96, 94130000], ["2016-06-09", 17969.98, 17985.19, 17915.88, 18005.22, 69690000], ["2016-06-10", 17938.82, 17865.34, 17812.34, 17938.82, 90540000], ["2016-06-13", 17830.5, 17732.48, 17731.35, 17893.28, 101690000], ["2016-06-14", 17710.77, 17674.82, 17595.79, 17733.92, 93740000], ["2016-06-15", 17703.65, 17640.17, 17629.01, 17762.96, 94130000], ["2016-06-16", 17602.23, 17733.1, 17471.29, 17754.91, 91950000], ["2016-06-17", 17733.44, 17675.16, 17602.78, 17733.44, 248680000], ["2016-06-20", 17736.87, 17804.87, 17736.87, 17946.36, 99380000], ["2016-06-21", 17827.33, 17829.73, 17799.8, 17877.84, 85130000], ["2016-06-22", 17832.67, 17780.83, 17770.36, 17920.16, 89440000]],

                    h1:  [ ["2016-04-21", 18192.84, 17982.52, 17963.89, 18107.29, 102720000], ["2016-04-22", 17985.05, 18003.75, 17909.89, 18026.85, 134120000], ["2016-04-25", 17990.94, 17977.24, 17855.55, 17990.94, 80770000], ["2016-04-26", 17987.38, 17990.32, 17934.17, 18043.77, 92570000], ["2016-04-27", 17996.14, 18041.55, 17920.26, 18084.66, 109090000], ["2016-04-28", 18023.88, 17830.76, 17796.55, 18035.73, 100920000], ["2016-04-29", 18092.84, 17982.52, 17963.89, 18107.29, 102720000], ["2016-05-02", 17985.05, 18003.75, 17909.89, 18026.85, 134120000], ["2016-05-03", 17870.75, 17750.91, 17670.88, 17870.75, 97060000], ["2016-05-04", 17735.02, 17651.26, 17409.01, 17738.06, 90020000], ["2016-05-05", 17664.48, 17660.71, 17615.82, 17736.11, 81530000], ["2016-05-06", 17650.3, 17740.63, 17580.38, 17744.54, 80000000], ["2016-05-09", 17743.85, 17705.91, 17668.38, 17783.16, 85590000], ["2016-05-10", 17726.66, 17908.35, 17726.66, 17934.61, 75790000], ["2016-05-11", 17919.03, 17711.12, 17711.05, 17919.03, 87390000], ["2016-05-12", 17531.76, 17710.71, 17531.76, 17755.8, 88440000], ["2016-05-13", 18119.03, 17711.12, 17711.05, 17919.03, 87390000], ["2016-05-16", 17531.76, 17710.71, 17531.76, 17755.8, 88440000], ["2016-05-17", 17514.16, 17435.4, 17331.07, 17514.16, 95530000], ["2016-05-18", 17501.28, 17526.62, 17418.21, 17636.22, 79120000], ["2016-05-19", 17514.16, 17435.4, 17031.07, 17514.16, 95539000], ["2016-05-20", 17701.46, 17529.98, 17469.92, 17701.46, 103260000], ["2016-05-23", 17507.04, 17492.93, 17480.05, 17550.7, 87790000], ["2016-05-24",17826.85, 17873.22, 17824.73, 17873.22, 73190000], ["2016-05-25", 17735.09, 17851.51, 17735.09, 17891.71, 79180000], ["2016-05-26",  18525.19, 17706.05, 17525.19, 17742.59, 86480000], ["2016-05-27", 17826.85, 17873.22, 17824.73, 17873.22, 73190000], ["2016-05-31", 17891.5, 17087.2, 17724.03, 17899.24, 147390000], ["2016-06-01", 17754.55, 17789.67, 17664.79, 17809.18, 78530000], ["2016-06-02",17938.82, 17865.34, 17812.34, 17938.82, 90440000], ["2016-06-03", 17799.8, 17807.06, 17689.68, 17833.17, 82270000], ["2016-06-06", 17825.69, 17920.33, 17822.81, 17949.68, 71820000], ["2016-06-07", 17936.22, 17938.28, 17936.22, 18003.23, 78750000], ["2016-06-08", 17825.69, 17920.33, 17822.81, 17949.68, 71870000], ["2016-06-09", 17969.98, 17985.19, 17915.88, 18005.22, 69650000], ["2016-06-10", 17938.82, 17865.34, 17812.34, 17938.82, 90540000], ["2016-06-13", 17830.5, 17732.48, 17731.35, 17893.28, 101690000], ["2016-06-14", 17938.82, 17865.34, 17812.34, 17938.82, 90540000], ["2016-06-15", 17703.65, 17640.17, 17629.01, 17762.96, 94130000], ["2016-06-16", 17602.23, 17733.1, 17471.29, 17754.91, 91950000], ["2016-06-17", 17733.44, 17675.16, 17602.78, 17733.44, 248680000], ["2016-06-20", 17736.87, 17804.87, 17736.87, 17946.36, 99380000], ["2016-06-21", 17827.33, 17829.73, 17799.8, 17877.84, 85130000], ["2016-06-22", 17832.67, 17780.83, 17770.36, 17920.16, 89440000]],

                    w1: [ ["2016-04-21", 17985.05, 18003.75, 17909.89, 18026.85, 134120000], ["2016-04-22", 17985.05, 18003.75, 17909.89, 18026.85, 134120000], ["2016-04-25", 17990.94, 17977.24, 17855.55, 17990.94, 83770000], ["2016-04-26", 17987.38, 17990.32, 17934.17, 18043.77, 92570000], ["2016-04-27", 17813.09, 17773.64, 17651.98, 17814.83, 136070000], ["2016-04-28", 18023.88, 17830.76, 17796.55, 18035.73, 100920000], ["2016-04-29", 17813.09, 17773.64, 17651.98, 17814.83, 136670000], ["2016-05-02", 17783.78, 17891.16, 17773.71, 17912.35, 80100000], ["2016-05-03", 17870.75, 17050.91, 17670.88, 17870.75, 97060000], ["2016-05-04", 17735.02, 17651.26, 17609.01, 17738.06, 95020000], ["2016-05-05", 17664.48, 17660.71, 17615.82, 17736.11, 81530000], ["2016-05-06", 17783.78, 17891.16, 17773.71, 17912.35, 80100000], ["2016-05-09", 17919.03, 17711.12, 17711.05, 17919.03, 87390000], ["2016-05-10", 17726.66, 17928.35, 17726.66, 17934.61, 75790000], ["2016-05-11", 17919.03, 17711.12, 17711.05, 17919.03, 87390000], ["2016-05-12", 17711.12, 17720.5, 17625.38, 17798.19, 88560000], ["2016-05-13", 17711.12, 17535.32, 17512.48, 17734.74, 86640000], ["2016-05-16", 17531.76, 17710.71, 17531.76, 17755.8, 88440000], ["2016-05-17", 17701.46, 17529.98, 17469.92, 17701.46, 103260000], ["2016-05-18", 17501.28, 17826.62, 17418.21, 17636.22, 79120000], ["2016-05-19", 17514.16, 17435.4, 17331.07, 17514.16, 95500], ["2016-05-20", 17525.19, 17706.05, 17525.19, 17742.59, 86480000], ["2016-05-23", 17507.04, 17492.93, 17480.05, 17550.7, 87791000], ["2016-05-24", 17525.19, 17706.05, 17525.19, 17742.59, 86480000], ["2016-05-25", 17735.09, 17851.51, 17735.09, 17891.71, 79180000], ["2016-05-26", 17859.52, 17828.29, 17803.82, 17888.66, 68940000], ["2016-05-27", 17826.85, 17873.22, 17824.73, 17873.22, 73190000], ["2016-05-31", 17891.5, 17787.2, 17724.03, 17899.24, 147390000], ["2016-06-01", 17754.55, 17789.67, 17664.79, 17809.18, 78530000], ["2016-06-02", 17789.05, 17838.56, 17703.55, 17838.56, 75560000], ["2016-06-03", 17799.8, 17807.06, 17689.68, 17833.17, 82270000], ["2016-06-06", 17825.69, 17920.33, 17822.81, 17949.68, 71870000], ["2016-06-07", 17936.22, 17938.28, 17936.22, 18003.23, 78750000], ["2016-06-08", 17931.91, 18005.05, 17931.91, 18016, 71260000], ["2016-06-09", 17969.98, 17985.19, 17915.88, 18005.22, 69690000], ["2016-06-10", 17938.82, 17865.34, 17812.34, 17938.82, 90540000], ["2016-06-13", 17830.5, 17732.48, 17731.35, 17893.28, 101690000], ["2016-06-14", 17710.77, 17674.82, 17595.79, 17733.92, 93740000], ["2016-06-15", 17903.65, 17640.17, 17629.01, 17762.96, 94130000], ["2016-06-16", 17602.23, 17733.1, 17471.29, 17754.91, 91950000], ["2016-06-17", 17733.44, 17675.16, 17602.78, 17733.44, 248680000], ["2016-06-20", 17602.23, 17733.1, 17471.29, 17754.91, 91950000], ["2016-06-21", 17827.33, 17829.73, 17799.8, 17877.84, 85130000], ["2016-06-22", 17827.33, 17829.73, 17799.8, 17877.84, 85130000]]

                }

                var data = this.splitData(temp[this.period]);

                let option = {};
                this.myChart.setOption(option = {
                    backgroundColor: '#000000',
                    animation: true,
                    tooltip: {
                        trigger: 'axis',
                        axisPointer: {
                            type: 'cross'
                        },
                        backgroundColor: 'rgba(245, 245, 245, 0.8)',
                        borderWidth: 1,
                        borderColor: '#ccc',
                        padding: 10,
                        textStyle: {
                            color: '#000'
                        },
                        position: function (pos, params, el, elRect, size) {
                            var obj = {top: 10};
                            obj[['left', 'right'][+(pos[0] < size.viewSize[0] / 2)]] = 30;
                            return obj;
                        }
                        // extraCssText: 'width: 170px'
                    },
                    axisPointer: {
                        link: {xAxisIndex: 'all'},
                        label: {
                            backgroundColor: '#777'
                        }
                    },
                    toolbox:{
                        show:false
                    },
                    visualMap: {
                        show: false,
                        seriesIndex:3,
                        dimension: 2,
                        pieces: [{
                            value: 1,
                            color: downColor
                        }, {
                            value: -1,
                            color: upColor
                        }]
                    },
                    grid: [
                        {
                            left: '0%',
                            right: '0%',
                            top: '0%',
                            height: '75%'

                        },
                        {
                            left: '0%',
                            right: '0%',
                            top: '75%',
                            height: '25%'
                        }
                    ],
                    xAxis: [
                        {
                            type: 'category',
                            data: data.categoryData,
                            scale: true,
                            boundaryGap : false,
                            axisTick: {show: false},
                            axisLine: {onZero: false},
                            splitLine: {show: false},
                            splitNumber: 20,
                            min: 'dataMin',
                            max: 'dataMax',
                            axisPointer: {
                                z: 100
                            }
                        },
                        {
                            type: 'category',
                            gridIndex: 1,
                            data: data.categoryData,
                            scale: true,
                            boundaryGap : false,
                            axisLine: {onZero: false},
                            axisTick: {show: false},
                            splitLine: {show: false},
                            axisLabel: {show: false},
                            splitNumber: 20,
                            min: 'dataMin',
                            max: 'dataMax'
                        }
                    ],
                    yAxis: [
                        {
                            scale: true,
                            splitArea: {
                                show: false
                            },
                            axisLabel: {show: false},
                            axisLine: {show: false},
                            axisTick: {show: false},
                            splitLine: {show: false}

                        },
                        {
                            scale: true,
                            gridIndex: 1,
                            splitNumber: 2,
                            axisLabel: {show: false},
                            axisLine: {show: false},
                            axisTick: {show: false},
                            splitLine: {show: false}
                        }
                    ],
                    series: [
                        {
                            name: 'Dow-Jones index',
                            type: 'candlestick',
                            data: data.values,
                            itemStyle: {
                                normal: {
                                    color: upColor,
                                    color0: downColor,
                                    borderColor: null,
                                    borderColor0: null
                                }
                            },
                            tooltip: {
                                formatter: function (param) {
                                    param = param[0];
                                    return [
                                        'Date: ' + param.name + '<hr size=1 style="margin: 3px 0">',
                                        'Open: ' + param.data[0] + '<br/>',
                                        'Close: ' + param.data[1] + '<br/>',
                                        'Lowest: ' + param.data[2] + '<br/>',
                                        'Highest: ' + param.data[3] + '<br/>'
                                    ].join('');
                                }
                            }
                        },
                        {
                            name: 'MA5',
                            type: 'line',
                            data: this.calculateMA(5, data),
                            smooth: true,
                            lineStyle: {
                                normal: {opacity: 0.5}
                            }
                        },
                        {
                            name: 'MA10',
                            type: 'line',
                            data: this.calculateMA(10, data),
                            smooth: true,
                            lineStyle: {
                                normal: {opacity: 0.5}
                            }
                        },
                        {
                            name: 'Volume',
                            type: 'bar',
                            xAxisIndex: 1,
                            yAxisIndex: 1,
                            data: data.volumes
                        }
                    ]
                }, true);

            }
        }
    }
</script>

<style lang="sass" rel="stylesheet/scss">

    .comp-kline{
        .period-bar{
            width: 100%;
            display: flex;
            background-color: #171717;
            padding: 0.2rem 0;
            font-size: .26rem;
            .period-item{
                flex: 1;
                text-align: center;
                color: #fff;
                &--active{
                    color: #ffa424;
                }
            }
        }
    }
</style>