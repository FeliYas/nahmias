<script setup>
import DashboardLayout from '@/layouts/DashboardLayout.vue';
import DataTable from '@/components/DataTable.vue';

defineOptions({
    layout: DashboardLayout
});

// Definición de las columnas
const columns = ['orden', 'path'];

// Definición de rutas dinámicas
const createRoute = route('banner.store');
const updateRoute = (id) => route('banner.update', { id });
const deleteRoute = (id) => route('banner.destroy', { id });

const props = defineProps({
    logo: {
        type: String,
        required: true
    },
    banners: {
        type: Array,
        required: true
    }
});
</script>

<template>
    <div>
        <div class="py-3 text-xl text-gray-700">
            <h1>Banners de nosotros</h1>
        </div>
        <!-- Línea -->
        <hr class="border-t-[3px] border-main-color rounded">
        <DataTable :columns="columns" :data="banners" :createRoute="createRoute" :updateRoute="updateRoute"
            :deleteRoute="deleteRoute" recomendacion="1400x450"/>
    </div>
</template>
