<script setup>
  import {onMounted, reactive, ref} from "vue";
  import Card from "./components/Card.vue";
  import Modal from "./components/Modal.vue";

  const data = reactive(ref(''));
  const textComment = ref('');
  const showModal = ref(false)

  function getData(){
    fetch('./data.json').then(function(response){
      response.json().then(function(result){
        data.value = result
        localStorage.setItem('comments', JSON.stringify(result))
      })
    })
  }

  getData()

  function sendComment(){
    if(textComment.value.length > 10){
      const dataComment = {
        id: 0,
        content: textComment,
        createdAt: 'ddd',
        score: 0,
        user: {
          image: { 
            png: data.value.currentUser.image.png,
            webp: data.value.currentUser.image.webp
          },
          username: data.value.currentUser.username
        },
        replies:[]
      }

      data.value.comments.push(dataComment);

      updateComments()
    }
    
  }

  function removeComment(el){
    data.value.comments = data.value.comments.filter((elCurrent)=>{
      elCurrent.replies = elCurrent.replies.filter((reply)=>{
        return reply !== el
      })
      return el !== elCurrent
    })
    
    updateComments()
  }

  function updateComments(){
    localStorage.setItem('comments', JSON.stringify(data.value.comments))
  }

  function toggleModal(isShow){
    showModal.value = isShow

    const body = document.querySelector("body")

    if(showModal.value === true){
      body.style.overflowY = "hidden"
      return
    }

    body.style.overflowY = "auto"
  }

  onMounted(()=>{
    data.value = localStorage.getItem('comments')
    data.value = JSON.parse(data.value);
  })

</script>

<template>
  <Modal v-if="showModal" @show-modal="toggleModal" />

  <main class="container vertical gap-1">
    <div v-for="comment in data.comments">
      <Card :comment="comment" :name-user="data.currentUser.username" @show-modal="toggleModal"/>
      <div class="d-flex" style="margin-top: 1rem;" v-if="comment.replies.length > 0">
        <hr class="line-replying">
        <div class="vertical gap-1">
          <Card class="child-comment" v-for="reply in comment.replies" :comment="reply" :name-user="data.currentUser.username" @show-modal="toggleModal" />
        </div>
      </div>
    </div>

    <div class="card d-flex gap-05 justify-content-between">
      <img :src="data.currentUser.image.webp" alt="" class="thumb">
      <textarea v-model="textComment" class="input-comment" placeholder="Add a comment..."></textarea>
      <button @click="sendComment" class="btn-primary">Send</button>
    </div>
  </main>
</template>

<style lang="css">
    @import './main.css'
</style>
