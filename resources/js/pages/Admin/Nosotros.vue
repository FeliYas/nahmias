<script setup>
import { ref, onMounted, inject } from 'vue';
import { useForm } from '@inertiajs/vue3';
import DashboardLayout from '@/layouts/DashboardLayout.vue';
import QuillEditor from '@/components/QuillEditor.vue';

defineOptions({
    layout: DashboardLayout
});

// Inyectar el sistema de notificaciones global
const notification = inject('noti');

const props = defineProps({
    logo: {
        type: String,
        required: true
    },
    nosotros: {
        type: Object,
        required: true
    }
});

const imagePreview = ref('');

// Funciones para detectar tipo de archivo
const getExtension = (filename) => {
    if (!filename) return '';
    return filename.split('.').pop().toLowerCase();
};

const isImage = (filename) => {
    const exts = ['jpg', 'jpeg', 'png', 'gif', 'svg', 'webp'];
    return exts.includes(getExtension(filename));
};

const isVideo = (filename) => {
    const exts = ['mp4', 'webm', 'ogg', 'avi', 'mov'];
    return exts.includes(getExtension(filename));
};

// Initialize the form with nosotros data
const form = useForm({
    descripcion: props.nosotros.descripcion,
    descripcionen: props.nosotros.descripcionen,
    descripcionport: props.nosotros.descripcionport,
    path: null // This will hold the file object
});

// Set initial image preview
onMounted(() => {
    imagePreview.value = props.nosotros.path;
});

// Preview the selected image or video
const previewImage = (event) => {
    const file = event.target.files[0];
    if (file) {
        form.path = file;

        // Para videos, usar URL.createObjectURL
        if (isVideo(file.name)) {
            imagePreview.value = URL.createObjectURL(file);
        } else {
            // Para imágenes, usar FileReader como antes
            const reader = new FileReader();
            reader.onload = (e) => {
                imagePreview.value = e.target.result;
            };
            reader.readAsDataURL(file);
        }
    }
};

// Submit the form for update
const submit = () => {
    form.post(route('nosotros.update', props.nosotros.id), {
        preserveScroll: true,
        onSuccess: (page) => {
            // Accede al mensaje flash de la respuesta
            if (page.props.flash && page.props.flash.message) {
                notification({ message: page.props.flash.message, type: "success" });
            } else {
                notification({ message: "Actualizado correctamente", type: "success" });
            }
        },
        onError: (errors) => {
            console.log(errors);
            notification({ message: errors[0], type: "error" });
        },
    });
};
</script>

<template>

    <div class="group relative h-full">
        <div class="py-3 text-xl text-gray-700">
            <h1>Nosotros</h1>
        </div>
        <!-- Línea expandible -->
        <hr class="border-t-[3px] border-main-color transition-all duration-500 ease-in-out opacity-70 rounded">

        <div class="w-full flex flex-col gap-8 py-4">
            <form @submit.prevent="submit"
                class="w-full transition-all duration-300 hover:shadow-lg hover:border-main-color transform hover:-translate-y-1">
                <div
                    class="w-full bg-gray-100 p-6 border border-gray-200 rounded-lg shadow-sm hover:shadow-md transition-all duration-300 group">
                    <div class="flex flex-col md:flex-row gap-8"> <!-- Contenedor de la imagen con vista previa -->
                        <div class="md:w-1/3 flex flex-col">
                            <h3
                                class="block text-sm font-medium text-gray-700 mb-2 group-hover:text-main-color transition-colors duration-300">
                                Imagen o Video principal</h3>
                            <div
                                class="relative overflow-hidden rounded-lg border-2 border-gray-200 group-hover:border-main-color transition-all duration-300">

                                <!-- Mostrar imagen -->
                                <img v-if="isImage(imagePreview)" :src="imagePreview" alt="Imagen"
                                    class="w-full h-auto object-cover rounded-md transition-all duration-500">

                                <!-- Mostrar video -->
                                <video v-else-if="isVideo(imagePreview)" :src="imagePreview" controls
                                    class="w-full h-auto object-cover rounded-md transition-all duration-500">
                                    Tu navegador no soporta el elemento video.
                                </video>

                                <!-- Mostrar placeholder si no hay nada -->
                                <div v-else class="w-full h-48 bg-gray-200 flex items-center justify-center rounded-md">
                                    <svg xmlns="http://www.w3.org/2000/svg" class="h-12 w-12 text-gray-400" fill="none"
                                        viewBox="0 0 24 24" stroke="currentColor">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                            d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z" />
                                    </svg>
                                </div>

                                <!-- Overlay con efecto al hover -->
                                <div
                                    class="absolute inset-0 bg-black bg-opacity-0 flex items-center justify-center transition-all duration-300 opacity-0 hover:bg-opacity-20 hover:opacity-100">
                                    <label for="path"
                                        class="cursor-pointer bg-white bg-opacity-80 text-main-color py-2 px-4 rounded-full flex items-center transform transition-transform duration-300 hover:scale-105">
                                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-1" fill="none"
                                            viewBox="0 0 24 24" stroke="currentColor">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                                d="M3 9a2 2 0 012-2h.93a2 2 0 001.664-.89l.812-1.22A2 2 0 0110.07 4h3.86a2 2 0 011.664.89l.812 1.22A2 2 0 0018.07 7H19a2 2 0 012 2v9a2 2 0 01-2 2H5a2 2 0 01-2-2V9z" />
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                                d="M15 13a3 3 0 11-6 0 3 3 0 016 0z" />
                                        </svg>
                                        Cambiar archivo
                                    </label>
                                </div>
                            </div>

                            <!-- Recomendación de tamaño -->
                            <span class="text-xs text-gray-400 mt-2 italic">
                                Imágenes: 670x600 px | Videos: MP4, WEBM, OGG
                            </span>

                            <!-- Input file oculto que acepta imágenes y videos -->
                            <input type="file" class="hidden" id="path" @change="previewImage" accept="image/*,video/*">
                        </div>

                        <!-- Contenedor de formulario -->
                        <div class="flex flex-col justify-between w-full md:w-2/3 text-black">
                            <div class="flex flex-col lg:flex-row gap-6">
                                <div class="lg:max-w-1/3">
                                    <label for="descripcion"
                                        class="block text-sm font-medium text-gray-700 mb-1 transition-colors duration-200 group-focus-within:text-main-color">Descripción</label>
                                    <QuillEditor unique_ref="descripcion_editor" placeholder="Descripción"
                                        :initial_content="form.descripcion"
                                        v-on:text_changed="form.descripcion = $event">
                                    </QuillEditor>
                                </div>
                                <div class="lg:max-w-1/3">
                                    <label for="descripcionen"
                                        class="block text-sm font-medium text-gray-700 mb-1 transition-colors duration-200 group-focus-within:text-main-color">Descripción
                                        en ingles</label>
                                    <QuillEditor unique_ref="descripcionen_editor" placeholder="Descripción"
                                        :initial_content="form.descripcionen"
                                        v-on:text_changed="form.descripcionen = $event">
                                    </QuillEditor>
                                </div>
                                <div class="lg:max-w-1/3">
                                    <label for="descripcionport"
                                        class="block text-sm font-medium text-gray-700 mb-1 transition-colors duration-200 group-focus-within:text-main-color">Descripción
                                        en portuges</label>
                                    <QuillEditor unique_ref="descripcionport_editor" placeholder="Descripción"
                                        :initial_content="form.descripcionport"
                                        v-on:text_changed="form.descripcionport = $event">
                                    </QuillEditor>
                                </div>
                            </div>

                            <!-- Botón de actualizar -->
                            <div class="mb-6.5 mt-4">
                                <button type="submit" class="btn-primary w-full flex items-center justify-center"
                                    :disabled="form.processing">
                                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" fill="none"
                                        viewBox="0 0 24 24" stroke="currentColor">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                            d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-8l-4-4m0 0L8 8m4-4v12" />
                                    </svg>
                                    {{ form.processing ? 'Actualizando...' : 'Actualizar nosotros' }}
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </form>
        </div>
    </div>
</template>