<template>
    <div id="app">
        <img :src=link alt="" ref="myData">
        <button @click="switchLink">切换链接</button>
        <table>
            <tr>
                <th v-for="(row,index) in rowArray" :key="index">{{row}}</th>
            </tr>
            <tr v-for="(colum,index) in columnArray" :key="index">
                <td v-for="(item,subkey) in colum" :key="subkey">{{item}}</td>
            </tr>
        </table>
    </div>
</template>
<script>
const d3 = require('d3-dsv')
export default {
    name: 'App',
    data(){
        return{
            link:'js/test1.csv',
            name:'liting',
            rowArray:[],
            columnArray:[]
        }
    },
    mounted(){
        this.handleTableData()
    },
    watch:{
        'link'(newValue,oldValue){
            console.log(newValue,oldValue)
            this.handleTableData()
        }
    },
    methods:{
        handleTableData(){
            let href = this.$refs.myData.src
            this.$axios.get(href).then(res => {
                let list = res.data
                let arr = this._CSVToArray(list,',')
                this.rowArray = arr[0]
                for(var i=1;i<arr.length;i++){
                    this.columnArray.push(arr[i])
                }
            }).catch(err=>{
                console.log(err)
            })
        },
        _CSVToArray( strData, strDelimiter ) {
            strDelimiter = (strDelimiter || ",");
            var objPattern = new RegExp(
                (
                    "(\\" + strDelimiter + "|\\r?\\n|\\r|^)" +
                    "(?:\"([^\"]*(?:\"\"[^\"]*)*)\"|" +
                    "([^\"\\" + strDelimiter + "\\r\\n]*))"
                    ),
                "gi"
            );
            var arrData = [[]];
            var arrMatches = null;
            while (arrMatches = objPattern.exec( strData )){
                var strMatchedDelimiter = arrMatches[ 1 ];
                if (
                    strMatchedDelimiter.length &&
                    (strMatchedDelimiter != strDelimiter)
                    ){
                    arrData.push( [] );
                }
                if (arrMatches[ 2 ]){
                    var strMatchedValue = arrMatches[ 2 ].replace(
                        new RegExp( "\"\"", "g" ),
                        "\""
                    );
                } else {
                    var strMatchedValue = arrMatches[ 3 ];
                }
                arrData[ arrData.length - 1 ].push( strMatchedValue );
            }
            return arrData;
        },
        switchLink(){
            if(this.link == 'js/test1.csv'){
                this.rowArray = []
                this.columnArray = []
                this.link = 'js/test2.csv'
            }else{
                this.rowArray = []
                this.columnArray = []
                this.link = 'js/test1.csv'
            }
        }
    }
    
}
</script>