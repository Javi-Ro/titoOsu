<template>
    <div class="filter-bar">
        <div class="cabecera">
            <p>
                Temas
            </p>
        </div>
        <div class="tags">
            <div class="tag-body" :id="setTagID(index)" 
            v-for="(tag,index) in tags" :key="tag.id" @click="getPostsByTag(tag.id)"
            >
                <div class="tag">
                    <p class="tagname">
                        {{tag.name}}
                    </p>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {
            tags: [],
            nTags: 0
        }
    },
    methods: {
        getTags() {
            axios.get(`/api/tags/posts`).then(response => {
                this.tags = response.data.tags;
                this.nTags = this.tags.length - 1;
            }).catch(error => {
                console.info(error);
            });
        },
        setTagID(index) {
            // Es necesario distinguir entre la primera etiqueta y el resto
            if(index == 0)
                return "first-tag";
            // Es necesario distinguir entre la última etiqueta y el resto
            else if(index == this.nTags)
                return "last-tag";
            else
                return "";
        },
        // Evento on-click para cuando se pulse una etiqueta
        getPostsByTag(tag) {
            axios.get(`/api/posts/filter-by-tag`, {
                params: {
                    tag: tag
                }
            }).then(response => {
                this.$parent.posts = response.data.posts;
            }).catch(error => {
                console.info(error);
            });
        },
        cardModal() {
            this.$buefy.modal.open({
                parent: this,
                component: showAllTags,
                hasModalCard: true,
                customClass: 'custom-class custom-class-2',
                // trapFocus: true
            })
        }
    },

    created() {
        this.getTags();
    }
}
</script>
<style lang="scss" scoped>
$blue:#00309a;
// Indica el radio de la barra de filtros
$radio: 10px;
.filter-bar {
    display:flex;
    border: 1px solid #dbdbdb;
    //flex-direction: column;
    overflow: hidden;
    background-color: white;
    position: fixed;
    height: fit-content;
    left: 0;
    padding: 20px 15px;
    top: 87px;
    width: 100%;
    z-index: 5;

    @media(min-width: 1500px){
        display: none;
    }

}
.tags {
    display: flex;
    flex-direction: row;
    justify-content: flex-start;
    overflow-y: auto;
    flex-wrap: nowrap;
}
.tag {
    display: flex;
    font-size: 1rem;
    justify-content: center;
    color: #000;
    background-color: #e4e6eb;
    padding: 15px;
    margin-right: 15px;
    border-radius: 20px;
    font-weight: 500;
}

.tag:hover{
    background-color: #d8dadf;
}

.tag-body {
    width: 100%;
    cursor: pointer;
}

// Primera etiqueta
// #first-tag {
//     border-radius: ($radio - 4px) ($radio - 4px) 0 0;
//     cursor: default;
    
// }
// #first-tag:hover {
//     background-color: transparent;
// }
// Última etiqueta
#last-tag{
    border-radius: 15px;
}

.cabecera {
    // display: flex;
    font-weight: bold;
    font-size: 1.5rem;
    color: #000;
    margin-right: 30px;
}
    // /* Track */
    // ::-webkit-scrollbar-track {
    // background: #f1f1f1; 
    // }
    
    // // /* Handle */
    // // ::-webkit-scrollbar-thumb {
    // // background: rgb(175, 175, 175); 
    // // }

    // // /* Handle on hover */
    // // ::-webkit-scrollbar-thumb:hover {
    // // background: #888; 
    // // }
</style>