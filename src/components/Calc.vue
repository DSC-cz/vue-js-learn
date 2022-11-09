<template>
    <p class="bg-danger p-1 text-white mb-3" v-if="error.length > 0">{{error}}</p>
    <form @submit.prevent="calc">
        <label for="numbersCount">Počet čísel:</label>
        <input type="number" id="numbersCount" v-model="input_number" class="form-control" placeholder="Počet čísel" @change="checkNumber()"/>
        <label for="operator">Vyber matematickou operaci:</label>
        <select id="operator" @change="()=>{calc()}" v-model="operator" class="form-select">
            <option value="0">+</option>
            <option value="1">-</option>
            <option value="2">*</option>
            <option value="3">/</option>
        </select>
        <input type="number" v-for="index in input_number" class="numbers form-control mt-3" :key=index :placeholder="'Číslo ' + (index)" /> 
    
        <button class="btn btn-primary mt-3 w-100">Vypočítat</button>

        <p class="bg-success text-white p-1 mt-3" v-if="result !== null">Výsledek {{operatorStr[operator]}}: {{result}}</p>
    </form>
</template>

<script>
    export default {
        name: 'CalcComponent',
        data(){
            return{
                input_number: 2,
                max_number: 6,
                numbers: [],
                operator: '0',
                operatorStr: ["sčítání", "odčítání", "násobení", "dělení"],
                error: '',
                result: null
            }
        },
        methods:{
            checkNumber(){
                if(this.input_number < 2){
                    this.throwError("Nelze provádět operace pouze s jedním číslem.");
                    this.input_number = 2;
                } else if(this.input_number > this.max_number){
                    this.throwError("Lze provádět operace pouze s "+this.max_number+" čísly.");
                    this.input_number = this.max_number;
                } else if(this.error.length > 0){
                    this.error = "";
                }
            },
            add(){
                let final = this.numbers[0];
                for(let i = 1; i < this.numbers.length; i++){
                    final+=this.numbers[i];
                }
                return final;
            },
            sub(){
                let final = this.numbers[0];
                for(let i = 1; i < this.numbers.length; i++){
                    final-=this.numbers[i];
                }
                return final;
            },
            multi(){
                let final = this.numbers[0];
                for(let i = 1; i < this.numbers.length; i++){
                    final*=this.numbers[i];
                }
                return final;
            },
            division(){
                let final = this.numbers[0];
                for(let i = 1; i < this.numbers.length; i++){
                    if(this.numbers[i] === 0){
                        this.throwError("Nulou nelze dělit, byla automaticky předělána na jedničku !");
                        this.numbers[i] = 1;
                    }

                    final/=this.numbers[i];
                }
                return final;
            },
            pushNumbers(){
                var numbers = document.querySelectorAll(".numbers");
                for(let i = 0; i < numbers.length; i++){
                    this.numbers.push(parseInt(numbers[i].value !== '' ? numbers[i].value : 0));
                }
            },
            calc(){
                this.pushNumbers();
                this.operator = parseInt(this.operator);
                
                if(this.operator < 0 || this.operator > this.operatorStr.length-1){
                    this.throwError("Matematický operátor neexistuje, bylo automaticky vybráno sčítání.");
                    this.operator = 0;
                }

                if(this.operator === 0) this.result = this.add();
                else if(this.operator === 1) this.result = this.sub();
                else if(this.operator === 2) this.result = this.multi();
                else if(this.operator === 3) this.result = this.division();

                this.numbers = [];
            },
            throwError(message){
                this.error = message;

                setTimeout(() => {
                    this.error = "";
                }, 3000);
            }
        }
    }
</script>