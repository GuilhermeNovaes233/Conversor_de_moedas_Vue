<template>
    <div class="conversor">
         <h2>{{moedaA}} Para {{moedaB}}</h2>   
         <input type="text" v-model="moedaA_value" v-bind:placeholder="moedaA">
         <input type="button" value="Converter" v-on:click="converter">
         <h2>{{moedaB_value}}</h2>
    </div>
</template>

<script>
export default {
    name: "Conversor",
    props: ["moedaA","moedaB"],
    data(){
        return{
            moedaA_value: "",
            moedaB_value: 0,
        }
    },
    methods: {
        getUrl(currencies) {
            return [
                "https://free.currconv.com/api/v7/convert?q=",
                currencies,
                "&compact=ultra&apiKey=cf7d04ce59b89a7f9b6e"
            ].join('');
        },
        converter() {
            const de_para = this.moedaA + "_" + this.moedaB;
            const url = this.getUrl(de_para);
            fetch(url)
                .then(res => {
                    return res.json()
                })
                .then(json => {
                    const cotacao = json[de_para];
                    const valor = (cotacao * parseFloat(this.moedaA_value)).toFixed(2)
                    this.moedaB_value = `${valor} ${this.moedaB}`;
                })
            }
        }
    }
</script>

<style scoped>
    .conversor{
        padding: 20px;
        max-width: 300px;
        box-shadow: 0 4px 8px 0;
    }

</style>