<template>
    <div id="team-table">
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
</template>

<script>
export default{
    name: "Dashboard",
    data(){
        return{
        jogadores: null,
    }
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

        this.getRegistros(); 
    }
},
mounted(){
    this.getRegistros();
}
}

</script>

<style scoped>
    #team-table{
        max-width: 900px;
        margin: 0 auto;
    }

    #team-table-heading,
    #team-table-rows,
    .team-table-row{
        display: flex;
        flex-wrap: wrap;
    }

    #team-table-heading{
        font-weight: bold;
        padding: 12px;
        border-bottom: 3px solid #333;
    }

    #team-table-heading div,
    .team-table-row div{
        width: 33%;
    }

    .team-table-row{
        width: 100%;
        padding: 12px ;
        border-bottom: 1px solid #ccc;
    }

    .delete-btn{
        background-color: #222;
        color: #fcba03;
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