<template>
    <section class="flex justify-center items-center bg-gray-800 w-screen h-screen  ">
        <div class="w-1/2 h-3/5 shadow-lg bg-gray-200 flex flex-col">
            <div class="w-full h-20 ">
                <input type="text" placeholder="Search for a species"
                class="backdrop-filter backdrop-blur-lg b-opacity-.5 z-40 w-full box-border font-sans text-2xl h-20 box-border pl-5 shadow-lg" 
                v-on:keydown="onSearch($event.target.value)" v-on:keydown.delete="onUnSearch($event.target.value)"/>
            </div>
            <div class=" flex-1 overflow-y-scroll">

                <div v-for="whale of whales" class="border-b-2 border-gray-300 z-10 bg-gray-100 min-h-20 box-border p-5 text-lg font-sans mt-2 shadow-sm ">
                    <h1 class="text-2xl font-semibold capitalize mb-1">{{whale.species}}</h1></br>
                    <p>{{whale.description}}</p>
  
     
                    <p>
                        <a class="flex items-center w-20 text-blue-600" target="_blank" 
                        v-bind:href="'https://www.google.com/maps/search/?api=1&query=' + whale.latitude + ',' + whale.longitude "><p>View</p>   <svg xmlns="http://www.w3.org/2000/svg" class=" ml-2 h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M13 9l3 3m0 0l-3 3m3-3H8m13 0a9 9 0 11-18 0 9 9 0 0118 0z" />
                        </svg></a>
                    </p>
                     </br>
                    <p class="text-sm text-white mt-2 bg-green-600 w-48 flex justify-center p-1 rounded-full">{{whale.sighted_at}}</p>
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
