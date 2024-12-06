
<script lang="ts" setup>
    import { computed, ref } from "vue";
    const emits = defineEmits(['close']);
    const props = defineProps({
        title: {
            type: String,
            default: "",
        },
        width: {
            type: String,
            default: "full",
            validator: (value: string) => ["xs", "sm", "md", "lg", "full"].indexOf(value) !== -1,
        }
    });
    
    const closeModal = () => {
        console.log("closeModal")
        emits('close');
    }
    const maxWidth = computed(() => {
    switch (props.width) {
        case "xs":
            return "w-2/12";
        case "sm":
            return "w-3/12";
        case "md":
            return "w-6_12";
        case "lg":
            return "w-9/12";
        case "full":
            return "w-full";
        default:
            return null;
    }
});

</script>

<template>
    <div class="modal-overlay is-open">
    <div class="modal">
        <div class="modal-header">
            <h3>{{ title }}</h3>
            <span class="modal-close" @click="closeModal">&#215;</span>
        </div>
        <div class="modal-body">
            <slot name="body"></slot>
        </div>
    </div>
    </div>

</template>

<style>
    .modal-overlay {
    display: none; /* Par défaut, la modale est masquée */
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.75); /* Fond noir semi-transparent */
    z-index: 9999;
    justify-content: center;
    align-items: center;
    }

    .modal-overlay.is-open {
    display: flex;
    }

    .modal {
    background: white;
    border-radius: 8px;
    box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1);
    max-width: 600px;
    width: 90%;
    overflow: hidden;
    animation: fadeIn 0.3s ease;
    }

    .modal-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 1rem;
        background: #f1f5f9; /* Couleur slate-100 */
        border-bottom: 1px solid #ddd;
        font-weight: bold;
    }

    .modal-close {
    font-size: 1.5rem;
    cursor: pointer;
    color: #333;
    transition: color 0.2s;
    }

    .modal-close:hover {
    color: #475569; /* Couleur slate-600 */
    }

    .modal-body {
    padding: 1rem;
    max-height: 400px;
    overflow-y: auto;
    }

    .modal-footer {
    padding: 1rem;
    text-align: right;
    border-top: 1px solid #ddd;
    }

    /* Animation d'apparition */
    @keyframes fadeIn {
    from {
        opacity: 0;
        transform: translateY(-10px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
    }

</style>