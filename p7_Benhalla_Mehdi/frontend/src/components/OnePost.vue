<template>
    <div class="onePost">
        

        <div class="modify-wrapper">
            <label for="modify-title">Modifier le titre :</label>
            <input type="text" id="modify-title" v-model="this.post.title">

            <label for="modify-content">Modifier le contenu :</label>
            <input id="modify-content" v-model="this.post.content">
        </div>

        <button @click="modifyOnePost()">Publier les modifications</button>
        <button class="delete-btn" @click="deleteOnePost()">Supprimer le post</button>
    </div>
    
</template>

<script>
import axios from 'axios';

export default {
    name: 'OnePost',

    
    data(){
        return{
            modifiedContent: '',
            post: [],
            authorized: false,
            modify: false
        }
    },

    mounted(){
        this.getOnePost();
    },

    methods: {
        getOnePost(){
            const postId = this.$route.params.id;
            
            axios.get(`${this.$apiUrl}/posts/${postId}`,
                {
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': `Bearer ${this.$token}`
                    }
                }
            )
            .then(res => {
                this.post = res.data[0];

                if(this.$user.userId === this.post.userId || this.$user.admin == 1){
                    this.authorized = true;
                 }

                else{
                    this.authorized = false;
                }
            })
        },

        deleteOnePost(){
            const postId = this.$route.params.id;
            
            axios.delete(`${this.$apiUrl}/posts/${postId}`,
                {
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': `Bearer ${this.$token}`
                    }
                }
            )
            .then(location.href = "/");
        },

        modifyOnePost(){
            const postId = this.$route.params.id;
            const title = document.querySelector('#modify-title').value;
            const content = document.querySelector('#modify-content').value;
            
            axios.put(`${this.$apiUrl}/posts/${postId}`,
                {
                    postId,
                    title,
                    content
                },
                {
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': `Bearer ${this.$token}`
                    }
                }
            )
            .then(location.href = "/");
        },
    }
}
</script>

<style scoped>
    /* Post style */
    .post-wrapper{
        margin: 50px auto 30px auto;
        padding: 30px;
        max-width: 800px;
        text-align: left;
        box-shadow: 0px 0px 50px -7px rgba(0,0,0,0.1);
        border-bottom: solid 5px;
    }

    .post-title {
        margin: 0;
        font-size: 2rem;
    }

    .post-content{
        margin-top: 20px;
    }

    /* Modify style */

    /* .modify-wrapper{
        display: flex;
        flex-direction: column;
        margin: 50px auto;
        padding: 30px;
        max-width: 800px;
        text-align: left;
        box-shadow: 0px 0px 50px -7px rgba(0,0,0,0.1);
        border-bottom: solid  5px;
    } */

    #modify-title {
        margin: 0;
        margin-bottom: 20px;
        font-size: 2rem;
    }

    #modify-content{
        margin-top: 20px;
        height: 200px;
        width: calc(100% - 22px);
        padding: 10px;
        resize: none;
    }

    .onePost button{
        margin-top: 20px;
        padding: 10px;
        font-size: 1.1rem;
        color: white;
        background-color: rgb(43, 42, 42);
        border: none;
        border-radius: 10px;
        transition-duration: 0.2s;
        cursor: pointer;
        margin: 0px 20px 50px 20px;
    }

    /* .delete-btn{
        background-color: red !important;
    } */

    label{
        font-size: 0.8rem;
        font-weight: bold;
        color: rgb(109, 109, 109);
        text-align: left;
        border: 0;
        clip: rect(0 0 0 0);
        height: 1px;
        margin: -1px;
        overflow: hidden;
        padding: 0;
        position: absolute;
        width: 1px;
    }
</style>