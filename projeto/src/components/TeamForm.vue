<template>
    <div>
        <Message :msg="msg" v-show="msg" />
        <div class="form-container">
            <form id="team-form" @submit="createTeam">
                <h1>Cadastre o time:</h1>
                <div class="input-container">
                    <label for="nome">Nome do time</label>
                    <input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite o nome">
                </div>
                <div class="input-container">
                    <input type="submit" class="submit-btn " value="Cadastrar o time">
                </div>
            </form>
            <div class="divider"></div>
            <form id="player-form" @submit="createPlayer">
                <h1>Cadastre o jogador:</h1>
                <div class="input-container">
                    <label for="time">Escolha o time:</label>
                    <select name="time" id="time" v-model="time">
                    <option v-for=" time in times" :key="time.id" :value="time.nome">
                        {{time.nome}}
                    </option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="jogador">Nome do jogador</label>
                    <input type="text" id="jogador" name="jogador" v-model="jogador" placeholder="Digite o nome">
                </div>
                <div class="input-container">
                    <input type="submit" class="submit-btn " value="Cadastrar o jogador">
                </div>
            </form>
        </div>
    </div>
</template>

<script>
import Message from './Message.vue';

export default{
    name: "TeamForm",
    data(){
        return{
            nome: '',
            times: null,
            jogadores: null,
            time: '',
            jogador: '',
            msg: null,
        }
    },
    methods: {
        async createTeam(e){

            e.preventDefault ();

            if (this.nome === '') {
        alert('Por favor, digite um nome para o time antes de enviar.');
        return;
      }

            const datat ={
                nome: this.nome,
            }

            const dataJsont = JSON.stringify(datat);
            const req = await fetch("http://localhost:3000/times", {
                method: "POST",
                headers: {"Content-Type": "application/json"},
                body: dataJsont
            });

            const res = await req.json();

            console.log(res);

            this.nome = "";

            window.location.reload();


        },
        async createPlayer(e){

            e.preventDefault ();

            if (this.jogador === '') {
        alert('Por favor, digite o nome do jogador antes de enviar.');
        return;  
      }
      if (this.time === '') {
        alert('Por favor, selecione o time antes de enviar.');
        return;
      }

            const dataj ={
                jogador: this.jogador,
                time: this.time,
        }

        const dataJsonj = JSON.stringify(dataj)

        const req = await fetch("http://localhost:3000/jogadores", {
                method: "POST",
                headers: {"Content-Type": "application/json"},
                body: dataJsonj
            });

            const res = await req.json();

            console.log(res);

            this.jogador = "";
            this.time = "";

            this.msg = 'Jogador cadastrado com sucesso'

            setTimeout(()=> this.msg = "", 3000);

},
            async getTeam(){

                const req = await fetch("http://localhost:3000/times");
                const data = await req.json();

                console.log(data);
                this.times = data.times;
            }
    },
    mounted(){
        fetch("http://localhost:3000/times")
        .then(resp => resp.json())
        .then(data=> this.times = data)
    },
    components:{
        Message
    }
}

</script>

<style scoped>
    #team-form, #player-form{
        max-width: 400px;
        margin: 0 auto;
    }

    .input-container{
        display: flex;
        flex-direction: column;
        margin-bottom: 20px;
    }

    label{
        font-weight:bold;
        margin-bottom: 15px;
        color: #000000;
        padding: 5px 10px;
        border-left: 4px solid rgb(24, 224, 24);
    }

    input, select{
        padding: 5px 10px;
        width: 300px;
    }

    .submit-btn {
        background-color: #1a1919;
        color: rgb(24, 224, 24);
        font-weight: bold;
        border: 2px solid #222;
        border-radius: 20px;
        padding: 10px;
        font-size: 16px;
        margin: 1 auto;
        cursor: pointer;
        transition:.5s;
    }

    .submit-btn:hover{
        background-color: transparent;
        color: #222;
    }

    .form-container {
    display: flex;
    align-items: center;
    
  }

  .divider {
    border-left: 3px solid black;
    height: 300px;
    margin: 0 20px;
    border-radius: 30px;
  }
</style>