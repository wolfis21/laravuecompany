<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import DangerButton from '@/Components/DangerButton.vue';
import { Head, Link, useForm } from '@inertiajs/vue3';
import Swal from 'sweetalert2';

const props = defineProps({
    departments: {type:Object}
});

const form = useForm({
    id:''
});

const deleteDepartments = (id, name) => {
    const alerta = Swal.mixin({
        buttonStyling:true
    });
    alerta.fire({
        title: 'Are you sure delete '+name+'?',
        icon: 'question', showCancelButton: true,
        confirmButtonText: '<i class="fa-solid fa-check"></i> Yes, delete',
        cancelButtonText: '<i class="fa-solid fa-ban"></i> Cancel'
    }).then((result) => {
        if(result.isConfirmed) {
            form.delete(route('departments.destroy', id));
        }
    });

}
</script>

<template>
    <Head title="Departments" />

    <AuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">Departments</h2>
        </template>

        <div class="py-12">
            <div class="bg-white grid v-screen place-items-center">
                <div class="mt-3 mb-3 flex">
                    <Link :href="route('departments.create')" :class="'px-4 py-2 bg-gray-800 text-white border rounded-md font-semibold text-xs'">
                        <i class="fa-solid fa-plus-circle"></i> Add
                    </Link>
                </div>
            </div>
            <div class="bg-white grid v-screen place-items-center">
                <div class="table-auto border border-gray-400">
                    <thead>
                        <tr class="bg-gray-100">
                            <th class="px-4 py-4">#</th>
                            <th class="px-4 py-4">Department</th>
                            <th class="px-4 py-4"></th>
                            <th class="px-4 py-4"></th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="dep, i in departments" :key="dep.id">
                            <th class="border border-gray-400 px-4 py-4">{{ (i+1) }}</th>
                            <th class="border border-gray-400 px-4 py-4">{{ dep.name }}</th>
                            <th class="border border-gray-400 px-4 py-4">
                                <Link :href="route('departments.edit', dep.id)" :class="'px-4 py-2 bg-yellow-400 text-white border rounded-md font-semibold text-xs'">
                                    <i class="fa-solid fa-edit"></i>
                                </Link>
                            </th>
                            <th class="border border-gray-400 px-4 py-4">
                                <DangerButton @click="$event => deleteDepartments(dep.id, dep.name)">
                                    <i class="fa-solid fa-trash"></i>
                                </DangerButton>
                            </th>
                        </tr>
                    </tbody>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
