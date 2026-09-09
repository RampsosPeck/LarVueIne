<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import { Head, Link, useForm, usePage } from '@inertiajs/vue3';
import InputLabel from '@/Components/InputLabel.vue';
import TextInput from '@/Components/TextInput.vue';
import InputError from '@/Components/InputError.vue';
import FileInput from '@/Components/FileInput.vue';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import { ref } from 'vue';

const page = usePage()
const contact = ref(page.props.contact);

const initialValues = {
    name: contact.value.name,
    phone: contact.value.phone,
    avatar: null,
    privacity: contact.value.privacity
}

const form = useForm(initialValues)

const onSelectAvatar = (e) =>{
    /*console.log(e.target.files); */
    const files = e.target.files;
    if(files.length){
        form.avatar = files[0]
    }
    /*console.log(form.avatar) */
}

const submit = () => {
    form.post(route('contact.update', contact.value),{
        onSuccess: (e) => {
            /*console.log(e.props.contact)*/
            contact.value = e.props.contact
        }
    })
}


</script>

<template>
    <Head title="Crear Contacto" />

    <AuthenticatedLayout>
        <template #header>  
            <div class="flex justify-between">

                <h2
                    class="text-xl font-semibold leading-tight text-gray-800"
                >
                    Actualizar Contacto
                </h2>
                <Link :href="route('contact.index')">
                    Lista de Contactos
                </Link>
            </div>
        </template>

        <div class="py-12">
            <div class="mx-auto max-w-7xl sm:px-6 lg:px-8">
                <div class="flex justify-center bg-white overflow-hidden bg-white shadow-sm sm:rounded-lg">
                    <form class="w-1/3 py-5 space-y-3" @submit.prevent="submit">
                        <Transition
                            enter-active-class="transition ease-in-out"
                            enter-from-class="opacity-0"
                            leave-active-class="transition ease-in-out"
                            leave-to-class="opacity-0"
                        >
                            <p
                                v-if="form.recentlySuccessful"
                                class="text-sm text-green-600 text-center"
                            >
                                Contacto Actualizado
                            </p>
                        </Transition>
                        <div>
                            <InputLabel for="name" value="Nombre" />
                            <TextInput
                                id="name"
                                type="text"
                                class="mt-1 block w-full"
                                v-model="form.name"
                                required
                                autofocus
                                autocomplete="name"
                                placeholder="Jhon Doe"/>
                            <InputError class="mt-2" :message="form.errors.name" />
                        </div>
                        <div>
                            <InputLabel for="phone" value="Teléfono" />
                            <TextInput
                                id="phone"
                                type="text"
                                class="mt-1 block w-full"
                                v-model="form.phone" 
                                autofocus
                                autocomplete="phone"
                                placeholder="76543210"/>
                            <InputError class="mt-2" :message="form.errors.phone" />
                        </div>
                        <div>
                            <img class="h-165" :src="`/storage/${contact.avatar}`" alt="">
                        </div>
                        <div>
                            <InputLabel for="avatar" value="Avatar" />
                            <FileInput name="avatar" @change="onSelectAvatar" />
                            <InputError class="mt-2" :message="form.errors.avatar" />
                        </div>
                        <div>
                            <InputLabel for="privacity" value="Privacidad" /> 
                            <select v-model="form.privacity" name="privacity" id="privacity" class="w-full mt-1 rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500">
                                <option value="private">Privado</option>
                                <option value="public">Público</option>
                            </select>
                            <InputError class="mt-2" :message="form.errors.privacity" />
                        </div>
                        <div class="flex justify-center">
                            <PrimaryButton>Actualizar Contacto</PrimaryButton>
                        </div>                        
                    </form>                    
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
