<template>
  <section>
    <div class="contenedor">
        <div class="titulo-pagina">
            CREA UNA PUBLICACIÓN
        </div>

        <div class="contenedor-dropdown"> 
          <b-dropdown append-to-body aria-role="menu" scrollable max-height="200" trap-focus>
            <template #trigger>
                <a class="navbar-item" role="button">
                    <span style="margin-right: 10px;">{{selectedCity}}</span>
                    <font-awesome-icon icon="fa-solid fa-caret-down" />
                </a>
            </template>

            <b-dropdown-item custom aria-role="listitem">
                <input type="text" v-model="searchTerm" autocomplete="on" id="buscador" placeholder="Buscar..." class="input">
            </b-dropdown-item>

            <b-dropdown-item 
              v-for="city in filteredData" :key="city[1]" 
              @click="setSelected(city[1])"
              aria-role="listitem">
                  {{city[0]}}
            </b-dropdown-item>
          </b-dropdown>
        </div>

        <b-tabs v-model="activeTab">

            <b-tab-item label="Texto">
              <b-field label="Título" >
                  <b-input maxlength="300" v-model="post.title"></b-input>
              </b-field>

              <b-field label="Texto">
                  <b-input minlength="0" style="width:550px;" type="textarea" class="custom-input" v-model="post.text"></b-input>
              </b-field>
            </b-tab-item>

            <b-tab-item label="Multimedia">
                <b-field>
                  <b-upload v-model="dropFiles" style="height:300px; width:550px;" multiple drag-drop>
                      <section class="custom-section">
                          <div class="content has-text-centered">
                              <font-awesome-icon id="upload-icon" icon="fa-solid fa-upload" style="opacity:0.8; width:40px; margin-bottom: 20px; height:40px"/>
                              <p>Arrastra y suelta o selecciona</p>
                          </div>
                      </section>
                  </b-upload>
              </b-field>
              <div class="tags">
                  <span v-for="(file, index) in dropFiles"
                      :key="index"
                      class="tag is-primary" >
                      {{file.name}}
                      <button class="delete is-small"
                          type="button"
                          @click="deleteDropFile(index)">
                      </button>
                  </span>
              </div>
              
            </b-tab-item>
            <!-- disabled -->
            <!-- <b-tab-item label="Enlace">  
              <b-field label="URL">
                  <b-input style="width:550px;" maxlength="300" type="url"></b-input>
              </b-field>
            </b-tab-item> -->
        </b-tabs>

        <button class="btn" @click="createPost()">
            <p style="text-align: center;">
                Publicar
            </p>
        </button>

    </div>
      <footer-web></footer-web>
  </section>
      


</template>

<script>
import filterBarHorizontal from './filterBarHorizontal.vue'
  export default {
  components: { filterBarHorizontal },
    props: {},
      data() {
        return {
            post: {
              title: null,
              text: null,
              img_url: null,
              city_id: null,
              user_id: null,
              post_id: null,
              file: File
            },
            activeTab: 0,
            searchTerm: '',
            availableCities: [],
            availableCitiesNames: [],
            selectedCity: 'Ciudad',
            dropFiles: []
        }
    },
    watch: {
      data: {
        immediate: true,
        deep: true,
        handler(val, oldVal) {
          //do something
        }
      },
    },
    computed: {
      filteredData() {
          this.availableCitiesNames = this.availableCities.map((item) => [item.name, item.id]);
          return this.availableCitiesNames.filter((item) => item[0].toLowerCase().indexOf(this.searchTerm.toLowerCase()) >= 0);
      },
    },
    methods: {
        deleteDropFile(index) {
          console.log(this.dropFiles);
            this.dropFiles.splice(index, 1)
        },
        getCities() {
          axios.get(`/api/cities`)
              .then(response => {
                  this.availableCities = response.data.cities;
              }).catch(error => {
                  console.info(error)
              });
        },
        getSelected(selected) {
            axios.post(`/api/get_city_by_id`, {
                id: selected
            })
                .then(response => {
                    this.post.city_id = response.data.city.id; // ID de la clase seleccionada
                    this.selectedCity = response.data.city.name;
                }).catch(error => {
                    console.info(error)
                });
        },
        setSelected(option) {
            console.log(this.dropFiles);
            this.getSelected(option);
        },
        createPost() {
          const formData = new FormData();
          formData.append('post', JSON.stringify(this.post));
          formData.append('file', this.dropFiles[0]);
          axios.post(`/api/posts`, formData).then(response => {
            // window.href.location = "/foro";
            console.log(response);
          }).catch(error => {
            if (error.response.status === 403) {
              window.location.href = "/login";
            } else if (error.response.status === 422) {
              alert("El titulo es obligatorio");
            }
          })
        }
    },
    mounted() {},
    created() {
      this.getCities();
    }
  }
</script>

<style>

  .custom-input:focus{
      border-color: #00309a !important;
      -webkit-box-shadow: 0 0 0 0.125em rgb(121 87 213 / 25%);
      box-shadow: 0 0 0 0.125em rgb(121 87 213 / 25%);
  }

  .input:focus{
      border-color: #00309a !important;
      -webkit-box-shadow: 0 0 0 0.125em rgb(121 87 213 / 25%);
      box-shadow: 0 0 0 0.125em rgb(121 87 213 / 25%);
  }
  .tag:not(body).is-primary{
    background-color: #00309a;
    color:white;
  }

</style>
<style lang="scss" scoped>

  .titulo-pagina {
      margin: 20px 0 15px 0;
      font-size: x-large;
      color:#00309a;
      font-family: sans-serif;
  }

  .upload-draggable{
    width: 550px;
    height: 300px;

  }

  .custom-section{
    width: 550px;
    height: 300px;
  }

  .contenedor-dropdown{
    margin-bottom: 15px;
  }

  .navbar-item {
    color: #00309a;
    padding: 0px;
  }
  a.navbar-item:hover {
      background-color: transparent;
  }
  .dropdown-item:active {
      background-color: transparent;
  }
  // Para cambiarle el borde a los input text
  .input:focus{
      border-color: #00309a !important;
      -webkit-box-shadow: 0 0 0 0.125em rgb(121 87 213 / 25%);
      box-shadow: 0 0 0 0.125em rgb(121 87 213 / 25%);
  }

  .contenedor{
      display:flex;
      flex-flow: column wrap;
      // justify-content: space-between;
  }

  .footer{
    display:flex;
  }


  
  section{
        margin-top: 130px;
        display:flex;
        flex-direction: column;
        flex-wrap: nowrap;
        align-items: center;
        height: auto;
        justify-content: space-between;
        height: calc(100vh - 130px);
  }

@media (max-height: 550px) {
  section{
        margin-top: 130px;
        display:flex;
        flex-direction: column;
        flex-wrap: nowrap;
        align-items: center;
        height: auto;
        justify-content: space-between;
        height: calc(100% - 130px);
  }
}




  .input-size{
    width: 550px;
  }

  b-input{
    width: 500px;
    align-self: center;
  }

  .btn {
	width: 40%;
  display:flex;
  background-color: #00309a;
  font-weight: bold;
  font-size: 0.9rem;
  font-family: Arial, Helvetica, sans-serif; 
	padding: .6rem 5.5rem;
	border-radius: 10rem;
	position: relative;
	overflow: hidden;
  color:white;
  align-self:center;
  text-align: center;
}

.btn:hover {
  color: yellow;
}

</style>