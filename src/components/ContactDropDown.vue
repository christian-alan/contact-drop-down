<template>
    <div class="contact-drop-down">
        <!-- Button -->
        <button id="contact-btn" @click="$emit('dummy-event')">

            <span v-if="picked === null" id="default-contact" class="text-sm text-gray-400">-- Select a contact --</span>
            <span v-if="picked != null" id="contact-chip" class="contact-chip">
                <img height=24 width=24 class="object-cover rounded-full h-6 w-6 important" :src="contacts[picked].src"
                    alt="profile picture" />
                {{ contacts[picked].name }}
            </span>
            <!-- Drop down arrow -->
            <span>
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M12 15L7 10H17L12 15Z" fill="#1D1B20" />
                </svg>
            </span>
        </button>

        <!-- Drop down menu -->
        <ul id="contact-drop-down-menu">
            <li v-for="(contact, index) in contacts"
                :class="index === picked ? 'bg-gray-50 border-[0.5px] border-gray-200' : ''">
                <div>
                    <input type="radio" :id="index" :value="index" v-model.lazy="picked" />
                    <label :for=index>
                        <img height=32 width=32 class="object-cover rounded-full h-8 w-8 important" :src=contact.src
                            alt="profile picture" />
                        {{ contact.name }}
                    </label>
                </div>


                <span v-if="index === picked">
                    <svg width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <circle cx="9" cy="9" r="9" fill="#74D37C" />
                        <path d="M4 9L7 12L13 6" stroke="white" stroke-width="1.4" stroke-linecap="round" />
                    </svg>
                </span>
            </li>
        </ul>
    </div>
</template>

<script>
import { ref } from 'vue'
import { contactIndex  } from '../store/store';
export default {
    props: {
        contacts: [{
            src: String,
            value: String,
            name: String
        }]
    },
    data() {
        return {
            picked:ref(null)
        }
    },
    watch:{
        picked(newValue,oldValue){
            if(newValue!=null) {
                contactIndex.set(newValue)
            }
        }
    }
}

</script>

<style>
@tailwind components;

@layer components {

    /* Contact drop down */
    .contact-drop-down {
        @apply relative;
    }

    .contact-drop-down button {
        @apply px-2 py-2 rounded-md border-[0.5px] border-gray-300 appearance-none w-full flex justify-between items-center h-11;

    }

    .contact-drop-down button:focus {
        @apply outline-none border-red-400;
        box-shadow: 0 0 5px #dc2626;
    }

    /* If the button is in focus then open ul --- "~" is used to select the subsequent child */
    .contact-drop-down button:focus~ul {
        @apply visible;
        -webkit-transition: all 0.1s ease-in-out;
        -moz-transition: all 0.1s ease-in-out;
        -ms-transition: all 0.1s ease-in-out;
        -o-transition: all 0.1s ease-in-out;
    }

    .contact-drop-down ul li input[type="radio"] {
        @apply appearance-none border-none absolute w-full h-full;
    }

    .contact-drop-down ul li label {
        @apply font-medium;
    }

    .contact-drop-down ul {
        @apply invisible flex py-3 px-2 bg-white rounded-lg mt-1 shadow-lg absolute w-full flex-col gap-2 h-40 overflow-y-auto;

    }

    .contact-drop-down ul li {
        @apply w-full p-2 rounded-lg flex justify-between items-center;
        -webkit-transition: all 0.01s ease-in-out;
        -moz-transition: all 0.01s ease-in-out;
        -ms-transition: all 0.01s ease-in-out;
        -o-transition: all 0.01s ease-in-out;
    }

    .contact-drop-down ul li:hover {
        @apply bg-gray-50 border-[0.5px] border-gray-200;
    }

    .contact-drop-down ul li label {
        @apply flex flex-row gap-2 items-center;
    }

    .contact-chip {
        @apply flex flex-row gap-2 items-center border-[0.5px] border-gray-300 py-1 px-2 rounded-lg bg-gray-50 text-sm font-medium; 
    }
}
</style>
