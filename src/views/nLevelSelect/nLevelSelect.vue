<template>
    <div class="select">
        <div>
             一级选中值
            <input type="text" v-model="province">
            
            <select v-model="province">
                <option :value="item.id" v-for="(item, idx) in arrProvince">{{item.name}}</option>
            </select>
        </div>
       
       <div>
             二级选中值：
            <input type="text" v-model="city">
             
            <select v-model="city">
                <option :value="item.id" v-for="(item, idx) in arrCity">{{item.name}}</option>
            </select>
        </div>
        
        <div>
            三级选中值：
            <input type="text" v-model="area">

            <select v-model="area">
                <option :value="item.id" v-for="(item, idx) in arrArea">{{item.name}}</option>
            </select>
        </div>
    </div>
</template>
<script>
const dataProvince = require('./data/province.js');
const dataCity = require('./data/city');
const dataArea = require('./data/area');

export default {
    data() {
        return {
            province: '',
            city: '',
            area: ''
        };
    },
    computed: {
        arrProvince() {
            // 有数据时，则加载省的数据
            if (!dataProvince.province) {
                return;
            }
            return dataProvince.province;
        },
        arrCity() {
            // 只有在省级选择完成后，才能加载市的数据
            if (!dataProvince.province || !this.province) {
                return;
            }
            return dataCity.city[this.province];
        },
        arrArea() {
            // 只有在省和区都有数据的情况下，才能加载区的数据。
            if (!this.province || !this.city) {
                return;
            }
            return dataArea.area[this.city];
        }
    },
    watch: {
        province(old, newVal) {
            this.city = '';
            this.area = '';
        },
        city(old, newVal) {
            this.area = '';
        }
    }
};
</script>
<style lang="less">
.select {
    & > div {
        margin-bottom: 10px;
    }
}
</style>


