<template>
    <div class="mega-table">
        <h1>Отсортировано по Price</h1>
        <table class="table table-bordered">
            <thead>
                <tr>
                    <td>Image</td>
                    <td>FullName</td>
                    <td>Name</td>
                    <td>PRICE</td>
                    <td>SUPPLY</td>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(elem, key) in sortedData" :key="key">
                    <td><img :src="URL+elem.CoinInfo.ImageUrl" /></td>
                    <td>{{ elem.CoinInfo.FullName }}</td>
                    <td>{{ elem.CoinInfo.Name }}</td>
                    <td>{{ elem.RAW.USD.PRICE }}</td>
                    <td>{{ elem.RAW.USD.SUPPLY }}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>
 
<script>
import axios from 'axios'

export default {
    name: 'result',
    data: () => ({
        URL: 'https://cryptocompare.com',
        list: [],
        interval: null
    }),
    computed: {
        sortedData: function() {
            function compare(a, b) {
                if (a.RAW.USD.PRICE > b.RAW.USD.PRICE)
                    return -1;
                if (a.RAW.USD.PRICE < b.RAW.USD.PRICE)
                    return 1;
                return 0;
            }
            return this.list.sort(compare);
        }
    },
    created () {
        this.updateData();
        this.interval = setInterval(this.updateData, 5000*60);
    },
    beforeDestroy () {
        clearInterval(this.interval)
    },
    methods: {
        updateData () {
            axios.get("https://min-api.cryptocompare.com/data/top/mktcapfull?limit=20&tsym=USD")
                .then(response => {
                    this.list = response.data.Data;
                    console.log(this.list[0].CoinInfo.ImageUrl);
                });
        }
    }
}
</script>
 
<style>
    #hello {
        font-family: Verdana;
        color: navy;
    }
    img{
        width: 30px;
        height: 30px;
    }
</style>