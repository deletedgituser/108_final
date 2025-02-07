<template>
    <div class="flex-1 p-6 max-h-full">
        <h1 class="text-2xl font-bold">Book Inventory</h1>

        <!-- Search Bar -->
        <div class="mt-4 flex items-center">
            <input 
                v-model="searchQuery"
                type="text"
                placeholder="Search for a book..."
                class="px-4 py-2 border rounded-md w-full"
            />
            <button 
                @click="performSearch"
                class="ml-2 bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
            >
                Search
            </button>
        </div>

        <!-- Book Table Section -->
        <div class="mt-4">
            <table class="min-w-full table-auto bg-white border-collapse border border-gray-200 shadow-md">
                <thead>
                    <tr class="bg-gray-800 text-white">
                        <th class="px-4 py-2 border border-gray-300 text-left">Book Name</th>
                        <th class="px-4 py-2 border border-gray-300 text-left">Description</th>
                        <th class="px-4 py-2 border border-gray-300 text-left">Remaining Quantity</th>
                        <th class="px-4 py-2 border border-gray-300 text-left">Action</th>
                    </tr>
                </thead>
                <tbody>
                    <tr class="bg-gray-50" v-for="record in filteredRecords" :key="record.id">
                        <td class="px-4 py-2 border border-gray-300">{{ record.name }}</td>
                        <td class="px-4 py-2 border border-gray-300">{{ record.description }}</td>
                        <td class="px-4 py-2 border border-gray-300">{{ record.qty }}</td>
                        <td class="px-4 py-2 border border-gray-300 text-center">
                            <button @click="editRecord(record)" class="text-blue-500 hover:underline">Edit</button>
                        </td>
                    </tr>
                    <tr v-if="props.records.length === 0"> 
                        <td colspan="5" class="border border-gray-300 px-6 py-2 text-center">
                            No books found.
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>

        <!-- Edit Form -->
        <div v-if="isEditing" class="mt-4 p-4 border border-gray-300 rounded-md bg-gray-50">
            <h3 class="text-lg font-bold mb-4">Edit Quantity</h3>
            <form @submit.prevent="updateRecord">
                <p><b>Book name: </b>{{ currentRecord.name }}</p>
                <div class="mb-4">
                    <label for="edit-quantity" class="text-gray-700"><b>Quantity: </b></label>
                    <input
                        id="edit-quantity"
                        v-model="currentRecord.qty"
                        type="number"
                        class="px-4 py-2 border rounded-md"
                        required
                    />
                </div>
                <div class="text-center">
                    <button type="submit" class="bg-green-500 text-white px-4 py-2 rounded hover:bg-green-600">Save</button>
                    <button @click="cancelEdit" type="button" class="ml-2 bg-gray-500 text-white px-4 py-2 rounded hover:bg-gray-600">Cancel</button>
                </div>
            </form>
        </div>
    </div>
</template>

<script setup>
import { ref, computed } from "vue";
import Layout from "../Layouts/Layout.vue";
import { useForm } from "@inertiajs/vue3";
import { defineProps } from "vue";

const props = defineProps({
    records: {
        type: Array,
        default: () => [],
    },
});

// Define the layout using Composition API
defineOptions({ layout: Layout });

const searchQuery = ref("");
const isEditing = ref(false);
const currentRecord = useForm({
    id: 0,
    name: "",
    qty: 0,
});

// Computed property for filtered records based on search query
const filteredRecords = computed(() => {
    if (!searchQuery.value) {
        return props.records;
    }
    return props.records.filter(record =>
        record.name.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
        record.description.toLowerCase().includes(searchQuery.value.toLowerCase())
    );
});

const editRecord = (record) => {
    isEditing.value = true;
    currentRecord.id = parseInt(record.id, 10);
    currentRecord.qty = parseInt(record.qty, 10);
    currentRecord.name = record.name;
};

const cancelEdit = () => {
    isEditing.value = false;
    currentRecord.qty = 0;
    currentRecord.id = 0;
};

const updateRecord = () => {
    currentRecord.put(`/dashboard/inventory/${currentRecord.id}`);
    isEditing.value = false;
    currentRecord.value = null;
};

// Function to perform search (optional if you need to trigger additional actions)
const performSearch = () => {
    console.log("Performing search for:", searchQuery.value);
};
</script>

<style scoped>
/* You can add custom styles here if necessary */
</style>
