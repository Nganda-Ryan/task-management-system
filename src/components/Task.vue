<script setup>
  
  const emits = defineEmits(['select', 'delete']);
  const props = defineProps({
    payload: {
        type: Object,
    }
  });
  const handleDelete = (event) => {
    emits('delete', props.payload.id);
  }
  const handleSelect = (payload) => {
    emits('select', props.payload.id);
  }
</script>

<template>
  <div class="container rounded-lg text-xs min-w-92 max-w-110 h-36 px-2_5 py-2_5 m-2_5" :style="`background-color: ${payload.color};`">
    <img src="/images/eagle.jpeg" alt="image" class="mr-2_5 w-2-6 rounded-lg">
    <div class="flex flex-col justify-between w-full">
      <div class="h-6 font-medium text-base cursor-pointer" @click="handleSelect">{{ payload.title }}</div>
      <div class="text-slate-700 h-18 mb-2_5 text-truncate-two-line italic font-medium">{{ payload.description }}</div>
      <div class="h-6 flex justify-between">
        <div class="pill rounded-md cursor-pointer p-1 bg-slate-50">
          {{ payload.todos.length }} todos
        </div>
        <div class="cursor-pointer h-5 w-5" @click="handleDelete">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="size-6">
            <path fill-rule="evenodd" d="M16.5 4.478v.227a48.816 48.816 0 0 1 3.878.512.75.75 0 1 1-.256 1.478l-.209-.035-1.005 13.07a3 3 0 0 1-2.991 2.77H8.084a3 3 0 0 1-2.991-2.77L4.087 6.66l-.209.035a.75.75 0 0 1-.256-1.478A48.567 48.567 0 0 1 7.5 4.705v-.227c0-1.564 1.213-2.9 2.816-2.951a52.662 52.662 0 0 1 3.369 0c1.603.051 2.815 1.387 2.815 2.951Zm-6.136-1.452a51.196 51.196 0 0 1 3.273 0C14.39 3.05 15 3.684 15 4.478v.113a49.488 49.488 0 0 0-6 0v-.113c0-.794.609-1.428 1.364-1.452Zm-.355 5.945a.75.75 0 1 0-1.5.058l.347 9a.75.75 0 1 0 1.499-.058l-.346-9Zm5.48.058a.75.75 0 1 0-1.498-.058l-.347 9a.75.75 0 0 0 1.5.058l.345-9Z" clip-rule="evenodd" />
          </svg>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
  .container {
    box-shadow:
      0 1px 1px hsl(0deg 0% 0% / 0.075),
      0 2px 2px hsl(0deg 0% 0% / 0.075),
      0 4px 4px hsl(0deg 0% 0% / 0.075),
      0 8px 8px hsl(0deg 0% 0% / 0.075),
      0 16px 16px hsl(0deg 0% 0% / 0.075)
    ;
  }

  .pill {
    color: #99154B;
    border: solid #99154B 1px;
  }
</style>
