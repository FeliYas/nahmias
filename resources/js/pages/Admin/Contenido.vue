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
    contenido: {
        type: Array,
        required: true
    }
});

const imagePreview1 = ref('');
const imagePreview2 = ref('');

// Initialize the form with contenido[0] data
const form1 = useForm({
    titulo: props.contenido[0]?.titulo || '',
    tituloen: props.contenido[0]?.tituloen || '',
    tituloport: props.contenido[0]?.tituloport || '',
    descripcion: props.contenido[0]?.descripcion || '',
    descripcionen: props.contenido[0]?.descripcionen || '',
    descripcionport: props.contenido[0]?.descripcionport || '',
    path: null // This will hold the file object
});
const form2 = useForm({
    titulo: props.contenido[1]?.titulo || '',
    tituloen: props.contenido[1]?.tituloen || '',
    tituloport: props.contenido[1]?.tituloport || '',
    descripcion: props.contenido[1]?.descripcion || '',
    descripcionen: props.contenido[1]?.descripcionen || '',
    descripcionport: props.contenido[1]?.descripcionport || '',
    path: null // This will hold the file object
});

// Set initial image preview
onMounted(() => {
    imagePreview1.value = props.contenido[0]?.path;
    imagePreview2.value = props.contenido[1]?.path;
});

// Preview the selected image for form1
const previewImage1 = (event) => {
    const file = event.target.files[0];
    if (file) {
        form1.path = file;
        const reader = new FileReader();
        reader.onload = (e) => {
            imagePreview1.value = e.target.result;
        };
        reader.readAsDataURL(file);
    }
};

// Preview the selected image for form2
const previewImage2 = (event) => {
    const file = event.target.files[0];
    if (file) {
        form2.path = file;
        const reader = new FileReader();
        reader.onload = (e) => {
            imagePreview2.value = e.target.result;
        };
        reader.readAsDataURL(file);
    }
};

// Submit the form1 for update
const submit1 = () => {
    form1.post(route('contenido.update', props.contenido[0]?.id), {
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

// Submit the form2 for update
const submit2 = () => {
    form2.post(route('contenido.update', props.contenido[1]?.id), {
        preserveScroll: true,
        onSuccess: () => {
            notification({ message: "Contenido actualizado correctamente", type: "success" });
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
            <h1>Contenido Home</h1>
        </div>
        <!-- Línea expandible -->
        <hr class="border-t-[3px] border-main-color transition-all duration-500 ease-in-out opacity-70 rounded">
        <div class="w-full flex flex-col gap-8 py-4">
            <form @submit.prevent="submit1"
                class="w-full transition-all duration-300 hover:shadow-lg hover:border-main-color transform hover:-translate-y-1">
                <div
                    class="w-full bg-gray-100 p-6 border border-gray-200 rounded-lg shadow-sm hover:shadow-md transition-all duration-300 group">
                    <div class="flex flex-col md:flex-row gap-8">
                        <!-- Contenedor de la imagen con vista previa -->
                        <div class="md:w-1/3 flex flex-col">
                            <h3
                                class="block text-sm font-medium text-gray-700 mb-2 group-hover:text-main-color transition-colors duration-300">
                                Imagen principal</h3>
                            <div
                                class="relative overflow-hidden rounded-lg border-2 border-gray-200 group-hover:border-main-color transition-all duration-300">
                                <img :src="imagePreview1" alt="Imagen"
                                    class="w-full h-auto object-cover rounded-md transition-all duration-500">

                                <!-- Overlay con efecto al hover -->
                                <div
                                    class="absolute inset-0 bg-black bg-opacity-0 flex items-center justify-center transition-all duration-300 opacity-0 hover:bg-opacity-20 hover:opacity-100">
                                    <label for="path1"
                                        class="cursor-pointer bg-white bg-opacity-80 text-main-color py-2 px-4 rounded-full flex items-center transform transition-transform duration-300 hover:scale-105">
                                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-1" fill="none"
                                            viewBox="0 0 24 24" stroke="currentColor">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                                d="M3 9a2 2 0 012-2h.93a2 2 0 001.664-.89l.812-1.22A2 2 0 0110.07 4h3.86a2 2 0 011.664.89l.812 1.22A2 2 0 0018.07 7H19a2 2 0 012 2v9a2 2 0 01-2 2H5a2 2 0 01-2-2V9z" />
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                                d="M15 13a3 3 0 11-6 0 3 3 0 016 0z" />
                                        </svg>
                                        Cambiar imagen
                                    </label>
                                </div>
                            </div>

                            <!-- Recomendación de tamaño -->
                            <span class="text-xs text-gray-400 mt-2 italic">Recomendación: 670x600 px</span>

                            <!-- Input file oculto -->
                            <input type="file" class="hidden" id="path1" @change="previewImage1">
                        </div>

                        <!-- Contenedor de formulario -->
                        <div class="flex flex-col justify-between w-full md:w-2/3 text-black">
                            <div class="flex flex-col gap-4">
                                <div class="flex flex-col lg:flex-row gap-6 ">
                                    <div class="relative group w-full">
                                        <label for="titulo"
                                            class="block text-sm font-medium text-gray-700 mb-1 transition-colors duration-200 group-focus-within:text-main-color">Título</label>
                                        <input type="text" id="titulo" v-model="form1.titulo"
                                            class="p-2 bg-white block border border-gray-300 w-full rounded-lg shadow-sm transition-all duration-200 focus:border-main-color focus:ring focus:ring-main-color focus:ring-opacity-20"
                                            required>
                                    </div>
                                    <div class="relative group w-full">
                                        <label for="tituloen"
                                            class="block text-sm font-medium text-gray-700 mb-1 transition-colors duration-200 group-focus-within:text-main-color">Título
                                            en ingles</label>
                                        <input type="text" id="tituloen" v-model="form1.tituloen"
                                            class="p-2 bg-white block border border-gray-300 w-full rounded-lg shadow-sm transition-all duration-200 focus:border-main-color focus:ring focus:ring-main-color focus:ring-opacity-20"
                                            required>
                                    </div>
                                    <div class="relative group w-full">
                                        <label for="tituloport"
                                            class="block text-sm font-medium text-gray-700 mb-1 transition-colors duration-200 group-focus-within:text-main-color">Título
                                            en portuges</label>
                                        <input type="text" id="tituloport" v-model="form1.tituloport"
                                            class="p-2 bg-white block border border-gray-300 w-full rounded-lg shadow-sm transition-all duration-200 focus:border-main-color focus:ring focus:ring-main-color focus:ring-opacity-20"
                                            required>
                                    </div>
                                </div>
                                <div class="flex flex-col lg:flex-row gap-6">
                                    <div class="w-full">
                                        <label for="descripcion"
                                            class="block text-sm font-medium text-gray-700 mb-1 transition-colors duration-200 group-focus-within:text-main-color">Descripción</label>
                                        <QuillEditor unique_ref="descripcion_editor" placeholder="Descripción"
                                            :initial_content="form1.descripcion"
                                            v-on:text_changed="form1.descripcion = $event">
                                        </QuillEditor>
                                    </div>
                                    <div class="w-full">
                                        <label for="descripcionen"
                                            class="block text-sm font-medium text-gray-700 mb-1 transition-colors duration-200 group-focus-within:text-main-color">Descripción
                                            en ingles</label>
                                        <QuillEditor unique_ref="descripcionen_editor" placeholder="Descripción"
                                            :initial_content="form1.descripcionen"
                                            v-on:text_changed="form1.descripcionen = $event">
                                        </QuillEditor>
                                    </div>
                                    <div class="w-full">
                                        <label for="descripcionport"
                                            class="block text-sm font-medium text-gray-700 mb-1 transition-colors duration-200 group-focus-within:text-main-color">Descripción
                                            en portugues</label>
                                        <QuillEditor unique_ref="descripcionport_editor" placeholder="Descripción"
                                            :initial_content="form1.descripcionport"
                                            v-on:text_changed="form1.descripcionport = $event">
                                        </QuillEditor>
                                    </div>
                                </div>

                            </div>

                            <!-- Botón de actualizar -->
                            <div class="mb-6.5 mt-4">
                                <button type="submit" class="btn-primary w-full flex items-center justify-center"
                                    :disabled="form1.processing">
                                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" fill="none"
                                        viewBox="0 0 24 24" stroke="currentColor">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                            d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-8l-4-4m0 0L8 8m4-4v12" />
                                    </svg>
                                    {{ form1.processing ? 'Actualizando...' : 'Actualizar contenido' }}
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </form>
        </div>
        <div class="w-full flex flex-col gap-8 py-4">
            <form @submit.prevent="submit2"
                class="w-full transition-all duration-300 hover:shadow-lg hover:border-main-color transform hover:-translate-y-1">
                <div
                    class="w-full bg-gray-100 p-6 border border-gray-200 rounded-lg shadow-sm hover:shadow-md transition-all duration-300 group">
                    <div class="flex flex-col md:flex-row gap-8">
                        <!-- Contenedor de la imagen con vista previa -->
                        <div class="md:w-1/3 flex flex-col">
                            <h3
                                class="block text-sm font-medium text-gray-700 mb-2 group-hover:text-main-color transition-colors duration-300">
                                Imagen principal</h3>
                            <div
                                class="relative overflow-hidden rounded-lg border-2 border-gray-200 group-hover:border-main-color transition-all duration-300">
                                <img :src="imagePreview2" alt="Imagen"
                                    class="w-full h-auto object-cover rounded-md transition-all duration-500">

                                <!-- Overlay con efecto al hover -->
                                <div
                                    class="absolute inset-0 bg-black bg-opacity-0 flex items-center justify-center transition-all duration-300 opacity-0 hover:bg-opacity-20 hover:opacity-100">
                                    <label for="path2"
                                        class="cursor-pointer bg-white bg-opacity-80 text-main-color py-2 px-4 rounded-full flex items-center transform transition-transform duration-300 hover:scale-105">
                                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-1" fill="none"
                                            viewBox="0 0 24 24" stroke="currentColor">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                                d="M3 9a2 2 0 012-2h.93a2 2 0 001.664-.89l.812-1.22A2 2 0 0110.07 4h3.86a2 2 0 011.664.89l.812 1.22A2 2 0 0018.07 7H19a2 2 0 012 2v9a2 2 0 01-2 2H5a2 2 0 01-2-2V9z" />
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                                d="M15 13a3 3 0 11-6 0 3 3 0 016 0z" />
                                        </svg>
                                        Cambiar imagen
                                    </label>
                                </div>
                            </div>

                            <!-- Recomendación de tamaño -->
                            <span class="text-xs text-gray-400 mt-2 italic">Recomendación: 670x600 px</span>

                            <!-- Input file oculto -->
                            <input type="file" class="hidden" id="path2" @change="previewImage2">
                        </div>

                        <!-- Contenedor de formulario -->
                        <div class="flex flex-col justify-between w-full md:w-2/3 text-black">
                            <div class="flex flex-col gap-4">
                                <div class="flex flex-col lg:flex-row gap-6 ">
                                    <div class="relative group w-full">
                                        <label for="titulo"
                                            class="block text-sm font-medium text-gray-700 mb-1 transition-colors duration-200 group-focus-within:text-main-color">Título</label>
                                        <input type="text" id="titulo" v-model="form2.titulo"
                                            class="p-2 bg-white block border border-gray-300 w-full rounded-lg shadow-sm transition-all duration-200 focus:border-main-color focus:ring focus:ring-main-color focus:ring-opacity-20"
                                            required>
                                    </div>
                                    <div class="relative group w-full">
                                        <label for="tituloen"
                                            class="block text-sm font-medium text-gray-700 mb-1 transition-colors duration-200 group-focus-within:text-main-color">Título
                                            en ingles</label>
                                        <input type="text" id="tituloen" v-model="form2.tituloen"
                                            class="p-2 bg-white block border border-gray-300 w-full rounded-lg shadow-sm transition-all duration-200 focus:border-main-color focus:ring focus:ring-main-color focus:ring-opacity-20"
                                            required>
                                    </div>
                                    <div class="relative group w-full">
                                        <label for="tituloport"
                                            class="block text-sm font-medium text-gray-700 mb-1 transition-colors duration-200 group-focus-within:text-main-color">Título
                                            en portuges</label>
                                        <input type="text" id="tituloport" v-model="form2.tituloport"
                                            class="p-2 bg-white block border border-gray-300 w-full rounded-lg shadow-sm transition-all duration-200 focus:border-main-color focus:ring focus:ring-main-color focus:ring-opacity-20"
                                            required>
                                    </div>
                                </div>
                                <div class="flex flex-col lg:flex-row gap-6">
                                    <div class="w-full">
                                        <label for="descripcion"
                                            class="block text-sm font-medium text-gray-700 mb-1 transition-colors duration-200 group-focus-within:text-main-color">Descripción</label>
                                        <QuillEditor unique_ref="descripcion2_editor" placeholder="Descripción"
                                            :initial_content="form2.descripcion"
                                            v-on:text_changed="form2.descripcion = $event">
                                        </QuillEditor>
                                    </div>
                                    <div class="w-full">
                                        <label for="descripcionen"
                                            class="block text-sm font-medium text-gray-700 mb-1 transition-colors duration-200 group-focus-within:text-main-color">Descripción
                                            en ingles</label>
                                        <QuillEditor unique_ref="descripcionen2_editor" placeholder="Descripción"
                                            :initial_content="form2.descripcionen"
                                            v-on:text_changed="form2.descripcionen = $event">
                                        </QuillEditor>
                                    </div>
                                    <div class="w-full">
                                        <label for="descripcionport"
                                            class="block text-sm font-medium text-gray-700 mb-1 transition-colors duration-200 group-focus-within:text-main-color">Descripción
                                            en portugues</label>
                                        <QuillEditor unique_ref="descripcionport2_editor" placeholder="Descripción"
                                            :initial_content="form2.descripcionport"
                                            v-on:text_changed="form2.descripcionport = $event">
                                        </QuillEditor>
                                    </div>
                                </div>
                            </div>

                            <!-- Botón de actualizar -->
                            <div class="mb-6.5 mt-4">
                                <button type="submit" class="btn-primary w-full flex items-center justify-center"
                                    :disabled="form2.processing">
                                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" fill="none"
                                        viewBox="0 0 24 24" stroke="currentColor">
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                            d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-8l-4-4m0 0L8 8m4-4v12" />
                                    </svg>
                                    {{ form2.processing ? 'Actualizando...' : 'Actualizar contenido' }}
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </form>
        </div>
    </div>
</template>