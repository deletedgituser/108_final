<template>
    <div class="container">
        <h2 class="text-2xl font-bold mb-4">Borrowed Books</h2>
        <div class="overflow-x-auto">
            <table class="w-full bg-white border border-white">
                <thead class="bg-gray-800 text-white">
                    <tr>
                        <th class="px-6 py-2 text-left border border-gray-300">Book Name</th>
                        <th class="px-6 py-2 text-left border border-gray-300">Borrower</th>
                        <th class="px-6 py-2 text-left border border-gray-300">Borrowed Qty</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="borrowed in props.borrowedBooks" :key="borrowed.id">
                        <td class="px-6 py-2 border border-gray-300">{{ borrowed.book_name }}</td>
                        <td class="px-6 py-2 border border-gray-300">{{ borrowed.student_name }}</td>
                        <td class="px-6 py-2 border border-gray-300">{{ borrowed.qty }}</td>
                    </tr>
                    <tr v-if="props.borrowedBooks.length === 0"> 
                        <td colspan="5" class="border border-gray-300 px-6 py-2 text-center">
                            No borrowed books yet.
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script setup>
import { ref, defineProps } from "vue";

const props = defineProps({
    borrowedBooks: {
        type: Array,
        default: () => [],
    },
});


</script>

<style scoped>
.container {
    width: 100%;
}
</style>
