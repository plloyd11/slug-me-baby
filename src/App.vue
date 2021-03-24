<template>
    <main class="flex flex-col justify-center min-h-screen py-12 bg-gray-50 sm:px-6 lg:px-8">
        <Toast v-show="slugged" :formattedSlug="formattedSlug" />

        <div class="sm:mx-auto sm:w-full sm:max-w-lg">
            <img src="./assets/slug.png" alt="Slug on a computer" class="mx-auto" />
            <h2 class="mt-6 text-3xl font-extrabold text-center text-gray-900">
                Slug Me Baby One More Time
            </h2>
            <p class="p-4 mt-4 text-2xl text-center">
                {{ formattedSlug }}
            </p>
        </div>

        <div class="mt-8 sm:mx-auto sm:w-full sm:max-w-md">
            <div class="px-4 py-8 bg-white shadow sm:rounded-lg sm:px-10">
                <form @submit.prevent class="space-y-6" action="#" method="POST">
                    <div>
                        <label for="email" class="block text-sm font-medium text-gray-700">
                            Text to Slug
                        </label>
                        <div class="mt-1">
                            <input
                                id="email"
                                v-model="slug"
                                name="text"
                                type="text"
                                required
                                class="block w-full px-3 py-2 placeholder-gray-400 border border-gray-300 rounded-md shadow-sm appearance-none focus:outline-none focus:ring-yellow-500 focus:border-yellow-500 sm:text-sm"
                            />
                        </div>
                    </div>

                    <div>
                        <button
                            @click="slugMe"
                            type="submit"
                            class="flex justify-center w-full px-4 py-2 text-lg font-bold text-yellow-800 transition bg-yellow-300 border border-transparent rounded-md shadow-sm hover:bg-yellow-500 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-yellow-500"
                        >
                            Slug Me
                        </button>
                    </div>
                    <hr />
                    <div class="flex items-center justify-between">
                        <button
                            type="button"
                            class="inline-flex items-center px-10 py-2 text-base font-medium text-white bg-gray-600 border border-transparent rounded-md shadow-sm hover:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-500"
                            @click="copyText"
                        >
                            <svg
                                class="w-5 h-5 mr-3 -ml-1"
                                xmlns="http://www.w3.org/2000/svg"
                                fill="none"
                                viewBox="0 0 24 24"
                                stroke="currentColor"
                            >
                                <path
                                    stroke-linecap="round"
                                    stroke-linejoin="round"
                                    stroke-width="2"
                                    d="M8 5H6a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2v-1M8 5a2 2 0 002 2h2a2 2 0 002-2M8 5a2 2 0 012-2h2a2 2 0 012 2m0 0h2a2 2 0 012 2v3m2 4H10m0 0l3-3m-3 3l3 3"
                                />
                            </svg>
                            Copy Slug
                        </button>
                        <button
                            type="button"
                            class="inline-flex items-center px-4 py-2 text-base font-medium text-gray-700 bg-white border border-gray-300 rounded-md shadow-sm hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-yellow-500"
                            @click="resetInput"
                        >
                            Clear
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </main>
</template>

<script>
import Toast from './components/Toast.vue'
export default {
    components: {
        Toast,
    },
    data() {
        return {
            slug: '',
            formattedSlug: '',
            slugged: false,
        }
    },
    methods: {
        slugMe() {
            let formatThis = this.slug
                .toString()
                .normalize('NFD')
                .replace(/[\u0300-\u036f]/g, '')
                .replace(/\s+/g, '-')
                .toLowerCase()
                .replace(/&/g, '-and-')
                .replace(/[^a-z0-9-]/g, '')
                .replace(/-+/g, '-')
                .replace(/^-*/, '')
                .replace(/-*$/, '')

            return (this.formattedSlug = formatThis)
        },
        copyText() {
            try {
                navigator.clipboard.writeText(this.formattedSlug)
                if (this.formattedSlug) {
                    this.slugged = true
                }
            } catch (error) {
                console.log(error)
            }
        },
        resetInput() {
            this.slug = ''
            this.formattedSlug = ''
            this.slugged = false
        },
    },
}
</script>
