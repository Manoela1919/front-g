<template>
    <div>
        <b-modal id="modal" class="mymodal" hide-header-close centered hide-footer has-modal-card size="xl" visible>
            <template #modal-title>
                <div id="title">
                    <div id="div-title">
                        <h5>Links de redirecionamento <b-icon icon="globe2" variant="secundary"></b-icon></h5>
                        <p id="subtitle">Crie seus links de redirect em poucos passos</p>
                    </div>
                    <div id="buttons">
                        <button id="X" @click="$bvModal.hide('modal')">
                            <svg width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
                            <path d="M16.7844 1.30859L1.78442 16.3086M1.78442 1.30859L16.7844 16.3086" stroke="#B5B9C5" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                            </svg>
                        </button>
                        <br>
                        <button id="criar" @click="$bvModal.hide('modal')" v-b-toggle.sidebar-1>Criar um link</button>
                    </div>
                </div>                 
            </template>
            <div id="main">
                <div id="redirect">
                    <div id="red-info">
                        <p id="number-links">{{ redirects.length }} links</p>
                        <p id="click-p">Cliques em tempo real</p>    
                    </div>
                    <div class="redirects" >
                        <div class="red-links" v-for="redirect in redirects" :key="redirect.id" :redirect="redirect">
                            <div class="name-date">
                                <p class="name">{{ redirect.name }}</p>
                                <p class="date">{{ getDate(redirect.created_at) }}</p>
                            </div>
                            <div class="url-click">
                                <p class="url">{{ redirect.hash }}</p>
                                <p class="cliques">&#128073 {{redirect.total_limit}}/000</p>
                            </div>
                        </div>
                    </div>
                </div>
                <div id="links">
                    <div id="links-info">
                        <div id="name-date-links">
                            <p id="name-links">Nome selecionado</p>
                            <p id="date-links">Criado em DD/MM/YYYY ás XX:XX</p>
                        </div>
                        <div id="url-buttons-links">
                            <p id="url-links">ur.principal.com/sdkjksdjkdsj</p>
                            <b-button id="copiar-links" variant="outline-primary" size="sm">Copiar</b-button>
                            <b-button id="editar-links" variant="outline-primary" size="sm">Editar</b-button>
                        </div>
                    </div>
                    <div id="links-links">
                        <div id="false-table-links">
                            <p id="id-links">01</p>
                            <div id="url-limit">
                                <input type="text" id="url-link">
                                <p id="limit-links">00/250</p>    
                            </div>
                            <b-button id="editar-link-button" variant="outline-primary" size="sm">editar</b-button>
                        </div>
                    </div>
                </div>
            </div>
        </b-modal>
        <b-sidebar id="sidebar-1" shadow right no-header>
            <div id="header-side">
                <h4>Criação de Link</h4>
                <svg v-b-toggle.sidebar-1 width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M16.7844 1.30859L1.78442 16.3086M1.78442 1.30859L16.7844 16.3086" stroke="#B5B9C5" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
            </div>
            <div id="body-side">
                <div id="name-div-side">
                    <p id="h5-side">Título do Link</p>
                    <input class="input-side" type="text" placeholder="Nome do Link" v-model="newTitle">
                </div>
                <div>
                    <p class="title-info-side">URL original</p>
                    <p class="info-side">Você poderá inserir uma ou várias URL’s, faça como desejar. Lembre-se de inserir a quantidade de cliques junto à URL.</p>
                </div>
                <div id="false-table">
                    <p id="id-side">01</p>
                    <input type="text" id="url-side" v-model="link">
                    <input id="limit-side" type="text" v-model="limit">
                </div>
                <b-button variant="outline-primary" id="adicionar-side" @click="saveLinks()"> <b-icon icon="plus"></b-icon> Adicionar mais URL</b-button>
                <div>
                    <p class="title-info-side">URL Default</p>
                    <p class="info-side">Essa URL será associada ao redirecionamento apenas quando todas as outras chegarem ao limite de cliques. Ela será a uma url fixa sem limitação.</p>
                    <input class="input-side" placeholder="Insira a URL Default">
                </div>
            </div>
            <div id="div-salvar">
                <b-button id="salvar-side" variant="primary" size="lg"> Salvar Link &#128170</b-button>
            </div>
        </b-sidebar>
    </div>
</template>

<script>
    import axios from "axios";
    import moment from "moment";
    moment.locale("pt-br");
    
    
    export default {
        name: 'modal',
        data() {
            return {
                redirects: [],
                newTitle: "",
                link: "",
                limit:"",
            };
        },

        methods: {
            getDate: function (date) {
            return moment(date, "YYYY-MM-DD").format(
                "DD/MM/YYYY"
            );
            },
            getRedirect() {
            var that = this;
            axios.get("http://127.0.0.1:8000/api/redirect").then(function (resp) {
                console.log(resp);
                that.redirects = resp.data.redirects;
            });
            },
            
            saveLinks() {
            var data = {
                id_redirect: this.id_redirect,
                limit: this.limit,
                click: this.click,
                link: this.link,
            };

            var that = this;

            axios.post("http://127.0.0.1:8000/api/links/", data).then(function () {
                that.id_redirect = "";
                that.limit = "";
                that.click = "";
                that.link = "";
            });
                that.buscandolinks();
            },
        },

        mounted() {
        this.getRedirect();
        },
    };
</script>

<style>
    @import url("https://fonts.googleapis.com/css2?family=Fira+Code:wght@300;500&family=Lato:ital,wght@0,300;0,400;0,700;0,900;1,100;1,300;1,400;1,700;1,900&family=Montserrat:wght@500;600&display=swap");
    * {
    font-family: "Montserrat", sans-serif!important;
    }
    div.modal.my-modal .modal-dialog {
    width: 90% !important;
    }

    #modal {
        width: 100%;
    }

    #subtitle {
        font-weight: 400;
        font-size: small;
        align-items: center;
        color: #81858E;
    }

    #main {
        height: 68vh;
        width: 100%;
        display: flex;
        flex-direction: row;
    }

    #title {
        width: 59vw;
        justify-content: space-between;
    }

    #div-title {
        padding: 1vh;
        margin-top: 2vh;
        margin-left: 1.5vw;
        display: inline-block;
        
    }

    #redirect {
        height: 100%;
        width: 40%;
        padding: 3vh;
    }

    #criar {
        padding: 1vh;
        border: none;
        padding-left: 5vh;
        padding-right: 5vh;
        border-radius: 8px;
        background-color: rgba(33, 51, 210, 0.1);
        color: #2133D2;
        margin-right: 2vw;
    }

    #buttons {
        width: fit-content;
        float: right;
        justify-content: right;
        padding: 1vh;
    }

    #X {
        float: right;
        border: none;
        background-color: transparent;
        margin-top: -0.8vh;
        margin-bottom: 1vh
    }

    #red-info {
        display: flex;
        margin-top: -2vh;
        border-bottom: 1px solid #E5E5E5;
    }

    #number-links {
        color: #2133D2;
        font-weight: 600;
        font-family: 'Montserrat';
    }

    #click-p {
        text-align: right;
        width: 85%;
        font-size: small;
        font-weight: 200;
        letter-spacing: 0.4px;
    }

    .redirects {
        height: 97%;
        overflow-y: auto;
    }

    .red-links{
        padding-top: 2vh;
        padding-bottom: 1vh;
        border-bottom: 1px solid #E5E5E5;
    }

    .name-date {
        display: flex;
        gap: 15px;
        align-items: center;
    }

    .name {
        font-weight: 600;
    }

    .date {
        font-size: small;
    }

    #links {
        height: 100%;
        width: 60%;
        padding: 40px;
        margin-left: 10px;
    }

    .url-click {
        display: flex;
        justify-content: space-between;
        gap: 20px;
        align-items: center;
        font-size: small;
        margin-top: -5px;
    }

    #name-date-links {
        display: flex;
        gap: 20px;
        align-items: center;
    }

    #name-links {
        font-size: x-large;
        font-weight: 600;
    }

    #date-links {
        font-size: small;
    }

    #url-links {
        color: #2133D2;
    }

    #url-buttons-links {
        display: flex;
        gap: 20px;
        height: 30px;

    }

    #copiar-links{
        width: 90px;
    }

    #editar-links{
        width: 90px;
    }

    #false-table-links {
        display: flex;
        justify-content: space-between;     
        height: 60px;
        margin-top: 20px;
        align-items: top;
    }

    #id-links {
        font-weight: 800;
        color: #2133D2;
        margin-left: 10px;
        margin-right: 10px;
    }

    #url-limit {
        height: 40px;
    }

    #url-link {
        font-size: small;
        width: 420px;
        border: none;
        background-color: transparent;
        color: #81858E;
    }

    #limit-links {
        font-size: small;
        color: #2133D2;
    }

    #editar-link-button {
        height: 30px;
        width: 90px;
    }

    #sidebar-1 {
        width: 45vw;
    }

    #header-side {
        width: 100%;
        background-color: #191B28;
        color: #E5E5E5;
        position: relative;
        padding: 20px;
        justify-content: space-between;
        display: flex;
        align-items: center;
        padding-left: 40px;
        padding-right: 40px;
    }

    #name-div-side {
        margin-bottom: 2vh;
    }

    #body-side {
        margin: 55px;
        height: 73%;
    }

    #h5-side {
        color: #191B28;
        font-weight: 600;
        font-size: large;
    }

    .input-side {
        border: none;
        background-color: transparent;
        padding: 10px;
        width: 100%;
        border-radius: 8px;
    }

    .input-side:focus, .input-side:hover {
        border-color: transparent;
        background-color: #2133d21a;
    }

    .info-side {
        color: #81858E;
    }

    .title-info-side {
        color: #2133D2;
        font-size: x-large;
        font-weight: 600;
        margin-bottom: 0;
    }

    #false-table {
        display: flex;
        gap: 10px;
        justify-content: space-between;
        width: 90%;
        height: 70px;
        align-items: center;
        margin-left: 10px;
    }

    #id-side {
        width: 10%;
        font-weight: 800;
    }

    #url-side {
        width: 80%;
        color: #81858E;
    }

    #limit-side {
        width: 10%;
        color: #81858E;
    }

    #adicionar-side {
        margin-bottom: 30px;
        border-color: #2133D2;
        color: #2133D2;
    }

    #adicionar-side:hover {
        border-color: #2133D2;
        color: #ffffff;
        background-color: #2133D2;
    }

    #div-salvar {
        justify-items: right;
        margin-right: 60px;
    }

    #salvar-side {
        float: right;
        width: 250px;
        background-color: #2133D2;
        border: #2133D2;
    }   

    #salvar-side:hover {
        background-color: #0f1c97;
        border: #0f1c97;
    }
    
</style>