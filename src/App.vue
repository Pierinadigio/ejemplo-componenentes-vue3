<script setup>
import { ref , computed , onMounted } from "vue";

 import ButtonCounter from './components/ButtonCounter.vue';
 import BlogPost from './components/BlogPost.vue';
 import BlogPostPrueba from './components/BlogPostPrueba.vue';
 import PaginatePost from './components/PaginatePost.vue';
 import LoadingSpinner from './components/LoadingSpinner.vue'

 //voy a poner los post en un arreglo de objetos para luego en template recorrer y mostrar

const arrayposts = ref([
    {title: "Post 1", colorText: "info", body: "Lorem ipsum dolor sit amet consectetur adipisicing elit. Dolores fugiat reiciendis impedit? Quaerat assumenda"},
    {title: "Post 2", colorText: "warning", body: "Lorem ipsum dolor sit amet consectetur adipisicing elit. Dolores fugiat reiciendis eiciendis impedit? Quaerat", colorBody: "danger"},
    {title: "Post 3", colorText: "warning", body: "Lorem ipsum dolor sit amet consectetur adipisicing elit. Dolores fugiat reiciendis impedit? Quaerat assumenda", colorBody: "warning-emphasis"},
 ]);

 //Quiero agregar a Mis Favoritos el titulo del Post...lo que quiero pasar lo guardo en una variable que luego a traves de un metodo lo cambio dinamicamente
 const miPostFavorito = ref("") //es un String y reactivo

  //metodo para actualizar Favorito donde le paso por parametro el titulo del post favorito 
  //lo agrego al v-for con @ un evento personalizable (con el npmbre que yo quiera seguido del @) igualado al metodo en cuestion..... @propiedad= "nombre del metodo"
  //este metodo se va a activar con @click desde mi componente secundario... para eso el componente secundario debo emitir el nombre de la propiedad (@propiedad) y el parametro (@click= "$emit("propiedad", parametro)")
  //$emit  el $ activa eventos Globales... si quiero algo mas personalizado voy por defineEmits
 const actualizarFavorito= (title)=>{
    miPostFavorito.value= title;
 };
 
 //Para Prueba--------slice(0,3) Devuelve una copia de una parte del array , desde inicio a fin pero NO el ultimo elemento.. en este caso tres elementos index 0,1 y 2-El array original no se modifica
 const posts = ref([]);//array vacio porque lo voy a consumir de la API
    //opcion para pasar el tamanio del array... con propiedad computada... recordar importarlo juntoo con ref
    /*const finarray = computed (()=> {posts.value.length} ) */
 const idFavorito= ref()
 const miFavorito= (id)=>{
  idFavorito.value= id;
 };
    //para paginacion  post por paginas puede NO ser una variable reactiva ya que es un numero fijo que yo defino que se vea por pagina, Inicio y fin SI son reactivos
 const postPorPag = 3;
 const inicio = ref(0);
 const fin = ref(postPorPag);
    //metodo para avanzar en pagina
const next = ()=>{
  inicio.value = inicio.value + postPorPag;
  fin.value = fin.value + postPorPag;
};
    //metodo para retroceder en pagina
const preview = ()=>{
  inicio.value = inicio.value - postPorPag;
  fin.value += -postPorPag;
};

    //Loading
const loading = ref(true);

  //con onMounted recibe una funcion de coldback---esto se ejecuta una vez que ya esta montado el template---O sea luego quese monte el sitio, el componente se hac la peticion
/*onMounted (async() => {
    try{
    const res = await fetch ('https://jsonplaceholder.typicode.com/posts')
    posts.value = await res.json()
    } catch (error){
      console.log(error)
    } finally {
      setTimeout (()=>{    //pero que se ejecute luegoo de dos segundos porque 
        loading.value = false;
      }, 2000 )
    }
})*/
 
/* fetch ('https://jsonplaceholder.typicode.com/posts')
    .then (response=> response.json())
    .then (data=> 
          posts.value = data
    )
    .catch(e => console.log(e))
    .finally (()=>{     //luego de la promesa , ya sea con exito o falla de la proesa, finalmente SIEMPRE se va a ejecutar lo siguiente.//el loading inicializado en true quiero que pase a false
      setTimeout (()=>{    //pero que se ejecute luegoo de dos segundos porque 
        loading.value = false;
      }, 2000 )
      
    } );
*/
      //ALTERNATIVA CORRECTA
const fetchData = async()=>{
  try{
    const res = await fetch ('https://jsonplaceholder.typicode.com/posts')
    posts.value = await res.json()
    } catch (error){
      console.log(error)
    } finally {
      setTimeout (()=>{    //pero que se ejecute luegoo de dos segundos porque 
        loading.value = false;
      }, 2000 )
    }
};

fetchData()


  


</script>

<template>
  <div class="flex-column">
      <div v-if="loading">
       <LoadingSpinner  />
      </div>
      <div v-else>
        <div class = "container" >
            <h1>App</h1>
              <ButtonCounter />
              <h3>Mis Favoritos: {{ miPostFavorito }}</h3>
                <div class= "d-flex align-items-baseline">
                  <BlogPost 
                    v-for="post in arrayposts"
                    :key="post.title"
                    :title ="post.title"
                    :colorText = "post.colorText"
                    :body="post.body"
                    :colorBody="post.colorBody"
                    @actualizar="actualizarFavorito"
                  ></BlogPost>
                </div>
        </div>
        <div class = "container mt-4">
            <h2>Mi Post Favorito es el Post - {{ idFavorito }}</h2>
          
              <PaginatePost
                      @next="next" 
                      @prev="preview"
                      :inicio="inicio"
                      :fin="fin"
                      :finarray="posts.length"
                      >
              </PaginatePost>
            
              <div class= " align-items-baseline mt-2">
                <BlogPostPrueba 
                  v-for="post in posts.slice(inicio,fin)"
                  :key="post.id"
                  :id = "post.id"
                  :title ="post.title"
                  :body="post.body"
                  @actualizar="miFavorito"
                  class="mb-2"
                ></BlogPostPrueba>
              </div>
        </div>
      </div>
  </div>


</template>
