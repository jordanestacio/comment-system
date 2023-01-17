<script setup>
    import { ref } from 'vue';

    const prop = defineProps({
        target: {},
    })

    const emit = defineEmits(["showModal"])
</script>

<template>
    <div class="backdrop-modal">
        <div class="modal">
            <h4 class="modal-title">Delete Comment</h4>
            <p class="content">Are you sure want to delete this comment? This will remove the comment and can't be undone.</p>
            <div class="d-flex gap-05">
                <button class="btn-grey" @click="$emit('showModal', false)">NO, CANCEL</button>
                <button class="btn-danger" @click="$emit('removeComment')">YES, DELETE</button>
            </div>
        </div>
    </div>
</template>