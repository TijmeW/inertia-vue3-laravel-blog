 <script setup>  
 import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
 import { library } from '@fortawesome/fontawesome-svg-core'
 import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
 import { faTrash, faPenToSquare } from '@fortawesome/free-solid-svg-icons'
 import {faSquareFacebook, faSquareTwitter, faSquareGooglePlus, faSquareInstagram, faLinkedin} from '@fortawesome/free-brands-svg-icons'
 import { Head, Link, useForm } from '@inertiajs/vue3';

 library.add(faTrash, faPenToSquare, faSquareFacebook, faSquareTwitter, faSquareGooglePlus, faSquareInstagram, faLinkedin)

 const props = defineProps({
     blog: Array,
     user_name: String,
     logged_in: Number,

     comments: Array,
     user_name_comment: String,
    });

 const created_at = props.blog.created_at.slice(0, 10);

 const form = useForm({
     content: null,
 });

function storeComment() {
    form.post(`/blog/${props.blog.id}/comment`);
    form.reset();
}

 </script>

 <template>  
    <Head title="Blog" />  
     <AuthenticatedLayout>
         <div class="pb-12">
             <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="flex m-2 p-2">
                    <Link href="/blog" class="px-4 py-2 bg-indigo-500 hover:bg-indico-600 text-white rounded">Back</Link>
                </div>
                 <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg mb-5">
                     <div class="mt-3" v-if="blog.user_id == logged_in">
                         <Link :href="`/blog/${blog.id}`" method="delete" as="button" type="button" class=" float-right mr-3 inline-flex w-full justify-center rounded-md border border-transparent bg-red-600 px-4 py-2 text-base font-medium text-white shadow-sm hover:bg-red-700  sm:ml-3 sm:w-auto sm:text-sm">Delete</Link>
                         <Link :href="`/blog/${blog.id}/edit`"><button type="button" class=" float-right inline-flex w-full justify-center rounded-md border border-gray-300 bg-white px-4 py-2 text-base font-medium text-gray-700 shadow-sm hover:bg-gray-50 sm:mt-0 sm:ml-3 sm:w-auto sm:text-sm">Edit</button></Link>
                     </div>
                        <div class="container mx-auto flex flex-wrap pt-6">
                            <div class="w-full md:w-1/2 flex flex-col flex-grow flex-shrink">
                                <div class="flex-1 bg-white rounded-t rounded-b-none overflow-hidden shadow">
                                    <div class="w-full flex flex-col items-center">
                                        <p class="w-10/12 text-gray-600 text-sm md:text-base">Posted by {{ user_name }} on {{ created_at }}</p>
                                        <div class="w-10/12 font-bold text-3xl text-gray-900 mb-6">{{ blog.title }}</div>
                                    </div>
                                    <div class="flex justify-center"><img :src="/storage/ + blog.image" class="w-10/12 h-auto"/></div>
                                    <div class="w-10/12 py-4 flex flex-row m-auto">
                                        <div class="flex flex-col text-gray-800 font-serif text-base mb-5 mt-5 w-1/12 whitespace-pre-wrap">
                                            <p class="text-gray-400 font-sans mb-2">Share</p>
                                            <font-awesome-icon class="fa-3x mr-10 mb-2" style="color: rgb(59 89 152)" icon="fa-brands fa-square-facebook" />
                                            <font-awesome-icon class="fa-3x mr-10 mb-2" style="color: rgb(29 161 242)" icon="fa-brands fa-square-twitter" />
                                            <font-awesome-icon class="fa-3x mr-10 mb-2" style="color: rgb(221 75 57)" icon="fa-brands fa-square-google-plus" />
                                            <font-awesome-icon class="fa-3x mr-10 mb-2" style="color: rgb(225 48 108)" icon="fa-brands fa-square-instagram" />
                                            <font-awesome-icon class="fa-3x mr-10 mb-2" style="color: rgb(14 118 168)" icon="fa-brands fa-linkedin" />
                                        </div>
                                        <div class="text-gray-800 font-serif text-base mb-5 mt-5 w-6/12 whitespace-pre-wrap">
                                            {{ blog.content }}
                                        </div>
                                        <div class="flex justify-center text-gray-800 font-serif text-base mb-5 mt-5 w-5/12 whitespace-pre-wrap">
						                    <img src="https://blogs.nvidia.com/wp-content/uploads/2023/01/drive-reg-nv-blog-406x350-1.jpg" class="w-11/12 h-80">
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <hr>
                        <div class="flex justify-center flex-col items-center mb-5">
                            <h1 class="text-lg mt-6">Comments</h1>
                            <div class="flex flex-col w-4/6">
                                <div v-for="comment in comments" :key="comment.id" class="flex flex-col w-full">
                                    <div class="flex flex-col w-full border shadow rounded my-2">
                                        <p class="ml-4 mt-3 text-indigo-500"> {{ comment.user_name_comment }} </p>
                                        <p class="ml-4 mt-3 leading-relaxed mb-5"> {{ comment.content }} </p>
                                        <div class="flex justify-between">
                                            <p class="ml-4 text-xs text-gray-500 leading-relaxed mb-5">posted on {{ comment.created_at.slice(0, 10) }} </p>
                                            <div v-if="comment.user_id == logged_in">
                                                <Link :href="`/comment/${comment.id}`" method="delete" as="button" type="button" class="text-lg mr-4"><font-awesome-icon icon="fa-solid fa-trash" style="color: rgb(129 140 248)" /></Link>
                                                <Link :href="`/comment/${comment.id}/edit`" as="button" type="button" class="text-lg mr-4"><font-awesome-icon icon="fa-solid fa-pen-to-square" style="color: rgb(129 140 248)" /></Link>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="flex justify-center flex-col items-center mb-5">
                            <h1 class="text-lg">Write a comment</h1>
                            <form @submit.prevent="storeComment" class="flex flex-col items-center w-4/6">
                                <textarea name="content" id="content" v-model="form.content" class="mb-5 w-full rounded resize-none" maxlength="255" rows="4"></textarea>
                                <button type="submit" class="inline-flex w-2/6 justify-center rounded-md border border-transparent bg-indigo-600 px-4 py-2 text-base font-medium text-white shadow-sm hover:bg-indigo-700 sm:ml-3 sm:text-sm">Comment</button>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
        </AuthenticatedLayout>
</template>