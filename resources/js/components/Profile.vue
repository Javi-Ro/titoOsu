<template>
    <div id="perfil">
        <!-- Columna en la que aparece la foto y el resto de opciones de editar -->
        <div class="columna" id="izq">
            <div class="profile-img">
                <img
                    src="/images/default-profile-img.jpeg"
                    alt="Imagen de perfil"
                    style="border-radius: 100px; max-width: 200px"
                >
            </div>
            <div class="nickname column-item">
                <p style="
                    font-weight: bold;
                    font-size: 20px;
                ">
                {{this.name}}
                </p>
                <p style="
                    opacity: 0.5;
                ">
                    @{{this.nickname}}
                </p>
            </div>
            <!-- Aparecen todas las opciones del perfil -->
            <div class="opciones">
                <!-- Si el nickname coincide con el de la ruta entonces es su perfil -->
                <button v-if="user == nickname"
                class="edit" type="button">
                    <span class="edit-icon"></span>
                    <span>Editar perfil</span>
                </button>
                
                <button v-else
                class="edit" type="button">
                    <span style="margin-left: 20px;">Añadir a amigos</span>
                </button>

                <a v-if="user == nickname"
                class="column-item btn-start" href="#">
                    Gestionar amigos
                </a>
                <a v-if="user == nickname"
                class="column-item btn-start" href="#">
                    Privacidad y seguridad
                </a>
                <a v-if="user == nickname" 
                class="column-item btn-start" href="#" >
                    Cambiar contraseña
                </a>                                
            </div>
        </div>
        <!-- Columna en la que aparece el nombre, la biografia y debajo las publicaciones  -->
        <div class="right-column">
            <div class="columna" id="der">
                <div class="background-img">
                </div>
                <!-- biografía, pais, amigos-->
                <div class="user-info">
                    <div class="biografia">
                        <p>
                            {{ this.bio }}
                        </p>
                    </div>
                    <div class="amigos-ciudad">
                        <div class="amigos">
                            <b-button type="is-info is-light">Ver amigos</b-button>
                        </div>
                        <div class="ciudad">
                            <template>
                                <section>
                                    <b-field>
                                        <b-tag rounded size="is-medium">
                                            <strong>{{ this.city }}</strong>, {{ this.country }}</b-tag>
                                    </b-field>
                                </section>
                            </template>
                            <!-- <strong>{{ this.city }}</strong>, {{ this.country }} -->
                        </div>
                    </div>
                </div>
                <!-- Permite cambiar entre los distintos tipos de publicaciones -->
                <section>
                    <b-tabs v-model="activeTab" position="is-centered" size="is-medium">
                        <!-- Ver todas las publicaciones del usuario (incluido comentarios) -->
                        <b-tab-item label="Mis publicaciones">
                            <div class="publicacion" v-for='post in myPosts' :key='post.id'>
                                <vista-previa-publicacion></vista-previa-publicacion>
                            </div>
                        </b-tab-item>
                        <!-- Ver publicaciones a las que le has dado me gusta -->
                        <b-tab-item label="Me gusta">
                            <div class="publicacion" v-for='post in LikedPosts' :key='post.id'>
                                <vista-previa-publicacion></vista-previa-publicacion>
                            </div>
                        </b-tab-item>
                        <!-- Ver publicaciones guardadas -->
                        <b-tab-item label="Guardados">
                            <div class="publicacion" v-for='post in SavedPosts' :key='post.id'>
                                <vista-previa-publicacion></vista-previa-publicacion>
                            </div>
                        </b-tab-item>
                    </b-tabs>
                </section>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    props: {
        // nickname del perfil que estamos viendo
        nickname: String,
        // Es solo el nickname del usuario que ha iniciado sesión
        user: String
    },
    data() {
        return {
            // Nick del usuario que ha iniciado sesión
            // actualNickname: "Willyrex",

            name: "",
            // nickname: "Willyrex",
            city: "Madrid",
            bio: "Curabitur nibh leo, venenatis at sodales ac, volutpat eget lectus. Donec ut est vel lorem sodales ultricies. Nullam a metus a odio rutrum posuere at a magna.Fusce neque nisl, vestibulum sed est vel, porta euismod dolor. Quisque egestas tristique leo pharetra bibendum. Praesent sit amet lacus risus. Duis non nisl a ligula tincidunt tincidunt. Morbi sed lorem dolor. Nullam dignissim tempus odio et egestas. Nunc nulla odio, congue a lorem non, eleifend accumsan lectus. Pellentesque habitant morbi tristique senectus et "
            ,
            country: "ESPAÑA",
            activeTab: 0,
            // Lista con las distintas publicaciones
            myPosts: [1],
            LikedPosts: [1, 2],
            SavedPosts: [1, 3, 4],
        }
    },
    created() {
        console.log(this.nickname);
        console.log(typeof(this.user));
        console.log(this.user);
        this.name = this.user;
    }
    // methods: {
    //     getNickName(nickname) {
    //         console.log("Perfil de: " + nickname);
    //         this.nickname = nickname;
    //     }
    // },
    // mounted() {
    //     this.getNickName(this.$route);
    // }
}
</script>
<style lang="scss">
    .tabs li.is-active a {
    border-bottom-color: #00309a !important;
    color: #00309a !important;
    }
</style>
<style lang="scss" scoped>
$blue: #00309a;
$margen-column: 10%;
// cc es column-content
$padding-cc: 2%;
$izq-column-width: 258px;

#perfil {
    display: flex;
    margin: 92px 0 0 0;
    width: 100%;
    // justify-content: center;
    flex-direction: row;
}
.columna {
    display: flex;
    flex-direction: column;
    border: 3px solid $blue;
    border-radius: 4px;
    padding: $padding-cc $padding-cc $padding-cc $padding-cc;
    box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
}
#izq {
    height: 100%;
    padding-left: 30px;
    border-radius: 0px;
    border-left: 0px;
    border-top: 0px;
    border-bottom: 0px;
    position: fixed !important;
    min-width: $izq-column-width;
    
}
// Padre de #der
.right-column {
    display:flex;
    width:100%;
    justify-content:center;
    margin: 38px 0 0 $izq-column-width;
    overflow-y: auto;
}
#der {
    width:90%;
}
// Opciones de la columna de la izquierda
.opciones {
    display: flex;
    justify-content: center;
    flex-direction: column;
}
.profile-img, .edit, .column-item{
    margin-bottom: 15px;
}
.column-item {
    font-size: 15px;
    display: flex;
    align-items: center;
    flex-grow: 0;
    flex-shrink: 0;
    padding: 0.5rem 0.75rem 0.75rem 0.75rem;
    color: $blue;
}
.nickname {
    color: #000;
    flex-direction: column;
    justify-content: center;
}

// CSS relacionado con la columna derecha

.background-img {
    min-height: 200px;
    border-radius: 3px;
    display: flex;
    background-image: url('/images/placeholders/bg-profile-default.jpg'); 
    background-repeat: no-repeat;
    background-size: cover;
}

.user-info {
    display: flex;
    margin: 30px 0 20px 0;
    flex-direction: column;
}
.amigos-ciudad {
    margin: 10px 30px 0 30px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.amigos {
    display:flex;
}

.ciudad {
    display:flex;
}

// CSS relacionado con ver las Publicaciones
.publicacion {
    display: flex;
    justify-content: center;
    margin: 10px 0 20px 0;
}
// Mucho texto para el botón de editar perfil con animación: https://codepen.io/FluidOfInsanity/pen/RpgvGW
*:before,
*:after {
  box-sizing: border-box;
}

.edit {
    display: flex;
    background: #79DB79;
    border: 0;
    border-radius: 5px;
    color: #fff;
    cursor: pointer;
    font-size: 15px;
    height: 40px;
    line-height: 40px;
    outline: none;
    padding: 0;
    padding-right: 10px;
    position: relative;
    -webkit-transition: background .4s;
    transition: background .4s;
}
.edit:hover {
    background: #4F9E4F;
}
.edit span {
    display: flex;
    float: left;
}
.edit .edit-icon {
    height: 40px;
    position: relative;
    width: 40px;
}
.edit .edit-icon:before {
    border: 3px solid #fff;
    border-radius: 3px;
    content: "";
    display: flex;
    height: 50%;
    left: 50%;
    position: absolute;
    top: 50%;
    width: 50%;
    transform: translate(-50%, -50%);
}
.edit .edit-icon:after { 
    background: #79DB79;
    border: 2px #fff solid;
    border-bottom-left-radius: 5px 15px;
    border-bottom-right-radius: 5px 15px;
    border-top-left-radius: 3px;
    border-top-right-radius: 3px; 
    box-shadow: 0 0 0 2px #79DB79;
    content: "";
    display: flex;
    height: 55%;
    position: absolute;
    top: 7%;
    left: 45%;
    width: 6px; 
    transform: translate(0, 0) rotate(45deg);
    transform-origin: 50% 75%;
    transition: background .4s, box-shadow .4s;
}
.edit:hover span:after {
    background: #4F9E4F;
    box-shadow: 0 0 0 2px #4F9E4F;
    animation: wiggle .25s 3 linear;
}

@keyframes wiggle {
    0% {transform: translate(0,0) rotate(45deg);}
    25% {transform: translate(0,0) rotate(25deg);}
    50% {transform: translate(0,0) rotate(45deg);}
    75% {transform: translate(0,0) rotate(65deg);}
    100% {transform: translate(0,0) rotate(45deg);}
}
</style>
