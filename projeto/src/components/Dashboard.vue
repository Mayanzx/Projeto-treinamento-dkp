<template>
    <div id="team-table">
        <Message :msg="msg" v-show="msg" />
        <div>
            <div id="team-table-heading">
                <div>Jogadores:</div>
                <div>Times pertencentes:</div>
                <div>Opção:</div>
            </div>
        </div>
        <div id="team-table-rows">
            <div class="team-table-row" v-for="jogador in jogadores" :key="jogador.id">
                <div>{{jogador.jogador}}</div>
                <div>{{jogador.time}}</div>
                <button class="delete-btn" @click="deleteJogador(jogador.id)">Deletar registro</button>
            </div>
        </div>
    </div>

    <br>
    <br>
    <h1>Gerenciar times</h1>

    <div id="teams-table">
        <div>
            <div id="teams-table-heading">
                <div>Times:</div>
                <div>Opção:</div>
            </div>
        </div>
        <div id="teams-table-rows">
            <div class="teams-table-row" v-for="time in times" :key="time.id">
                <div>{{time.nome}}</div>
                <button class="delete-btn" @click="deleteTime(time.id)">Deletar registro</button>
            </div>
        </div>
    </div>
</template>

<script>
import Message from './Message.vue';

export default{
    name: "Dashboard",
    data(){
        return{
        jogadores: null,
        times: null,
        msg: null
    }
},
components: {
    Message
},
methods:{
    async getRegistros(){

const req = await fetch("http://localhost:3000/jogadores");
const data = await req.json();

console.log(data);
this.jogadores = data;
},
    async deleteJogador(id){
        const req = await fetch(`http://localhost:3000/jogadores/${id}`, {
            method: "DELETE"
        });

        const res = await req.json();

        this.msg = 'Jogador deletado com sucesso'

        setTimeout(()=> this.msg = "", 3000);

        this.getRegistros(); 
    },
    async deleteTime(id){
        const req = await fetch(`http://localhost:3000/times/${id}`, {
            method: "DELETE"
        });

        const res = await req.json();

        this.msg = 'Time deletado com sucesso'

        setTimeout(()=> this.msg = "", 3000);

        this.getTimes(); 
    },

    async getTimes(){

const req = await fetch("http://localhost:3000/times");
const data = await req.json();

console.log(data);
this.times = data;
},

},
mounted(){
    this.getRegistros();
    this.getTimes();
}
}

</script>

<style scoped>
    #team-table{
        max-width: 900px;
        margin: 0 auto;
    }

    #teams-table{
        max-width: 600px;
        margin: 0 auto;
    }

    #team-table-heading,
    #team-table-rows,
    .team-table-row,
    #teams-table-heading,
    #teams-table-rows,
    .teams-table-row{
        display: flex;
        flex-wrap: wrap;
    }

    #team-table-heading,
    #teams-table-heading{
        font-weight: bold;
        padding: 12px;
        border-bottom: 3px solid #333;
    }

    #team-table-heading div,
    .team-table-row div{
        width: 33%;
    }

    #teams-table-heading div,
    .teams-table-row div{
        width: 50%;
    }

    .team-table-row,
    .teams-table-row{
        width: 100%;
        padding: 12px ;
        border-bottom: 1px solid #ccc;
    }

    .delete-btn{
        background-color: #222;
        color: rgb(24, 224, 24);
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 16px;
        margin: 0 auto;
        cursor: pointer;
        transition: .5s;
    }

    .delete-btn:hover{
        background-color: transparent;
        color: #222;
    }
    


</style>