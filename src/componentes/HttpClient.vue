<template>

  <section class="src-componentes-http-client">
    <div class="jumbotron">
      <h2>Llamado a Mockapi</h2>
      <br>

      <button class="btn btn-warning my-3 mr-3" @click="getPostsXHRCb()">XHR (cb)</button>
      <button class="btn btn-warning my-3 mr-3" @click="getPostsXHRPromise()">XHR (Promise)</button>
      
      <button class="btn btn-success my-3 mr-3" @click="getPostsFetch()">Fetch</button>
      <button class="btn btn-success my-3 mr-3" @click="getPostsAxios()">Axios</button>


      <button class="btn btn-danger my-3 mr-3" @click="posts = []">Clear</button>
      <br>

      <div v-if="posts.length" class="table-responsive">
        <table class="table table-dark">
          <tr>
            <th>ID</th>
            <th>Nombre</th>
            <th>Email</th>
            <th>Telefono</th>
          </tr>
          <!-- <tr v-for="(post,index) in posts" :key="index"> -->
          <tr v-for="post in posts" :key="post.id">
            <td>{{ post.id }}</td>
            <td>{{ post.nombre }}</td>
            <td>{{ post.email }}</td>
            <td>{{ post.numtel }}</td>
          </tr>
        </table>
      </div>
      <h4 v-else class="alert alert-danger text-center">No se encuentran datos</h4>
      
    </div>
  </section>

</template>

<script>

  export default  {
    name: 'src-componentes-http-client',
    props: [],
    mounted () {

    },
    data () {
      return {
        url: 'https://6372d0af348e947299fd572c.mockapi.io/users',
        posts : []
      }
    },
    methods: {
      getPostsXHRCb() {

        const xhr = new XMLHttpRequest()
        xhr.open('get', this.url)
        xhr.addEventListener('load', () => {
          if(xhr.status == 200) {
            let respuesta = JSON.parse(xhr.response)
            
            this.posts = respuesta
          }
        })
        xhr.send()
      },
      xhrPromise() {
        return new Promise((resolve,reject) => {

          const xhr = new XMLHttpRequest()
          xhr.open('get', this.url)
          xhr.addEventListener('load', () => {
            if(xhr.status == 200) {
              let respuesta = JSON.parse(xhr.response)

              resolve(respuesta)
            }
            else {
              reject(`Error http: ${xhr.status}`)
            }
          })
          xhr.send()
        })
      },
      async getPostsXHRPromise() {
          try {
            let respuesta = await this.xhrPromise()
            this.posts = respuesta
          }
          catch(error) {
            console.error(error)
          }
      },
      async getPostsFetch() {

        try {
          let response = await fetch(this.url)
          console.log(response)
          let respuesta = await response.json()
          this.posts = respuesta
        }
        catch(error) {
          console.error(error)
        }
      },
      async getPostsAxios() {
        
        try {
          let respuesta = await this.axios(this.url)
          this.posts = respuesta.data
        }
        catch(error) { console.error(error) } 

      }

    },
    computed: {

    }
}


</script>

<style scoped lang="css">
  .src-componentes-http-client {

  }

  .jumbotron {
    background-color: rgb(128, 109, 0);
    color: white;
  }

  hr {
    background-color: #bbb;
  }

</style>
