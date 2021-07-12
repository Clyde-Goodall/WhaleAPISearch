<template>
    <section class="flex justify-center items-center bg-gray-800 w-screen h-screen  ">
        <div class="w-1/2 h-3/5 shadow-lg bg-gray-200 flex flex-col">
            <div class="w-full h-20 ">
                <input type="text" placeholder="Search for a species"
                class="backdrop-filter backdrop-blur-lg b-opacity-.5 z-40 w-full box-border font-sans text-2xl h-20 box-border pl-5 shadow-lg" 
                v-on:keydown="onSearch($event.target.value)" v-on:keydown.delete="onUnSearch($event.target.value)"/>
            </div>
            <div class=" flex-1 overflow-y-scroll">

                <div v-for="whale of whales" class="border-b-2 border-gray-300 z-10 bg-gray-100 min-h-20 box-border p-5 font-sans mt-2 shadow-sm ">
                    <h1 class="text-2xl font-semibold capitalize mb-1">{{whale.species}}</h1></br>
                    <p>{{whale.description}}</p>
                        </br>   
     
                    <p>
                        <a class=" pl-2 pr-0 flex items-center w-20 text-blue-600 rounded-full border-blue-600 border w-48 flex justify-center p-1" target="_blank" 
                        v-bind:href="'https://www.google.com/maps/search/?api=1&query=' + whale.latitude + ',' + whale.longitude "><p>View</p>   <svg xmlns="http://www.w3.org/2000/svg" class="ml-2 h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
                        </svg>
                        </a>
                        </p>
                     <p class=" text-green-600 mt-2 border-green-600 border w-48 flex justify-center p-1 rounded-full">{{whale.sighted_at}}</p>
                 
                 </div>

            </div>
        </div>
    </section>
</template>

<script>

    export default {

        data() {
            return {
                whales: [],
                localCopy: []
            }
        },
        methods: {
            onSearch: function (val) {
                let temp = [];
                this.whales.forEach(function(search) {
                    if(search.species.includes(val)) {
                        temp.push(search);
                    }
                });

              
                this.whales = temp;
            },

            onUnSearch: function(val) {
             
                let temp = [];
                this.localCopy.forEach(function(search) {
                    if(search.species.includes(val)) {
                        temp.push(search);
                    }
                });

                this.whales = temp;
            }
        },
        async fetch() {
            this.whales = await fetch(
                'http://hotline.whalemuseum.org/api.json?limit=100'
            ).then(res => res.json());
            this.localCopy = this.whales;
        }



    }

</script>
