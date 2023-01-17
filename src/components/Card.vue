<script setup>
    import Modal from "./Modal.vue";

    import { ref } from 'vue';

    const emit = defineEmits(["modalShow"])

    const props = defineProps({
        comment:{},
        nameUser: '',
    })

    const isYourComment = props.nameUser === props.comment.user.username
    const showModal = ref(false);

    function increment(){
        return props.comment.score++
    }

    function decrement(){
        return props.comment.score--
    }
       
</script>

<template>
  <div class="card gap-1">
    <div class="input-amount vertical bg-light-gray">
        <button @click="increment" class="btn-text-grey">+</button>
        <span class="text-primary bold">{{props.comment.score}}</span>
        <button @click="decrement" class="btn-text-grey">-</button>
    </div>

    <div class="vertical w-100 gap-05">
        <div class="d-flex justify-content-between">
            <div class="d-flex aling-items-center">
                <img :src="props.comment.user.image.webp" alt="" class="mr-1 thumb">
                <span class="text-dark bold">{{props.comment.user.username}}</span>
                <span class="badge-primary" v-if="isYourComment">you</span>
                <span class="ml-1">{{props.comment.createdAt}}</span>
            </div>
            <div class="d-flex gap-1">
                <!-- <button v-if="isYourComment" @click="$emit('removeComment')" class="btn-text-danger gap-05"> -->
                <button v-if="isYourComment" @click="$emit('showModal', true)" class="btn-text-danger gap-05">
                    <img src="" alt="">
                    Delete
                </button>
                <button class="btn-text-primary gap-05">
                    <img src="" alt="">
                    Reply
                </button>
            </div>
        </div>
        <p>{{props.comment.content}}</p>
    </div>
  </div>
</template>