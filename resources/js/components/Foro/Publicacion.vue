<template>
  <section class="post-container">
    <div class="post-container-positioned">
      <filter-bar></filter-bar>
      <filter-bar-horizontal class="horizontal-menu"></filter-bar-horizontal>
      <div class="title-bar">
        <div class="title-bar-img">
          <img src="/images/arrow-left.svg" alt="Arrow left" width="14px" height="11px" @click="goBack();">
          <!-- <img src="images/arrow-left.svg" alt="Arrow left" width="14px" height="11px" @click="goBack();"> -->
          <div @click="goBack()">
            <font-awesome-icon icon="fa-solid fa-arrow-left" width="14px" height="11px"/>
          </div>
        </div>
        <p>Publicación</p>
      </div>
      <vista-previa-publicacion v-if="Object.entries(post).length!==0" class="post" :post="post"></vista-previa-publicacion>
      <div class="comments-container">
        <div class="post-comment">
          <img src="images/placeholders/default-profile-img.jpeg" class="comment-img" alt="" style="margin-right: 10px">
          <b-input class="post-comment-input" placeholder="Comenta..." rounded></b-input>
          <b-button class="information-personal-data-main-button" type="is-link" @click="sendComment()">Publicar</b-button>
        </div>
        <div class="comentarios" v-if="data">
          <div class="comentario" v-for="comment in comments" :key="comment.id">
            <div class="imagen">
              {{post.img_url}}
            </div>
            <div class="nickname">

            </div>
            <div class="comentario">
              {{post.title}}
            </div>
            <div class="likes">
              {{post.likes}}
            </div>
            <div class="boton me gusta">

            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
  export default {
    props:{post: Object},

    data() {
      return {
        comments: [{id: null}],
        data: false
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

    computed: {},

    methods: {
      goBack(){
        window.location.href = "/foro";
      },
      sendComment(){
        axios.post('/api/posts').catch(error => {
          console.info(error);
        })
      },
      getPostById(){
        axios.get('/api/posts/' + this.post.id).then(response => {
            this.post = response.data.post;
          }).catch(error => {
            console.info(error);
          })
      },
      getComments() {
        axios.get('/api/posts/'+ this.post.id +'/comments').then(response => {
            this.comments = response.data.comments;
            this.data = true;
          }).catch(error => {
            console.info(error);
          })
      },
      getPost() {
        return this.post;
      }
    },

    mounted() {
      this.getPostById()
      this.getComments()
    },

    created() {

    }
  }
</script>