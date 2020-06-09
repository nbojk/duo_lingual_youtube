<template>
    <div>
        <button type="button" @click="changeLang('rs')">Serbian</button>
        <button type="button" @click="changeLang('en')">English</button>
        <h1>{{ translations.mainHeading[this.lang] }} ({{ $route.params.lang }})</h1>

        <h2>{{ translations.listOfVideos[this.lang] }}</h2>

        <ul>
            <li v-for="(video, index) in videos[this.lang]" v-bind:key="index">
                <img @click="toggleModal(video.id)" :src="generateThumbnailUrl(video.id)" alt="">
                <h3 @click="toggleModal(video.id)">
                    {{ video.title }}
                </h3>
            </li>
        </ul>

        <div @click="toggleModal" v-if="showModal" id="modal">
            <iframe width="560" height="315" :src="embedVideo" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
        </div>
        
    </div>
</template>

<script>
export default {
    name: 'Videos',
    data() {
        return {
            lang: this.$route.params.lang,
            showModal: false,
            embedVideo: null,
            videos: {
                en: 
                [
                    {
                        title: "Billie Eilish - bad guy", 
                        id: "DyDfgMOUjCI"
                    },
                    {
                        title: "Eric Clapton - I Shot The Sheriff [Crossroads 2010] (Official Live Video)", 
                        id: "APWhx97QvxE"
                    },
                    {
                        title: "Billie Eilish - No Time To Die (Live From The BRIT Awards, London)", 
                        id: "2I1ZU5g1QNo"
                    },

                ],
                rs: [
                    {
                        title: "Smak - Blues U Parku", 
                        id: "2k2vmwOU2HM"
                    },
                    {
                        title: "Parni Valjak - Sve jos mirise na nju", 
                        id: "YrJvkquw7CM"
                    },
                    {
                        title: "Goran Bregovic - Bella Ciao - ( LIVE ) Paris 2013", 
                        id: "OyMA84-mowI"
                    }
                ]
            },
            translations: {
                mainHeading: {
                    en: 'Welcome to Duo Lingual YouTube',
                    rs: 'Dobrodosli na dvojezicni Jutjub'
                },
                listOfVideos: {
                    en: 'List of videos',
                    rs: 'Lista video snimaka'
                }
            }
            
        }
    },
    methods: {
        toggleModal: function(videoIdOrUrl) {
            if(this.showModal === false) {
                this.embedVideo = this.generateEmbedUrl(videoIdOrUrl);
                this.showModal = true;
            } else {
                this.showModal = false;
                this.embedVideo = null;
            }
        },
        /**
        * Expects an argument that is either a youtube URL or a ID,
        * and returns back the ID.
        */
        getIdFromUrl: function(videoIdOrUrl) {
            if (videoIdOrUrl.indexOf('http') === 0) {
                return videoIdOrUrl.split('v=')[1];
            } else {
                return videoIdOrUrl;
            }
        },
                
        /**
        * Expects an argument that is either a youtube URL or a ID,
        * and returns back the URL of the thumbnail for that video.
        */
        generateThumbnailUrl: function(videoIdOrUrl) {
            return 'https://i3.ytimg.com/vi/' + this.getIdFromUrl(videoIdOrUrl) + '/default.jpg';
        },

        /**
        * Expects an argument that is either a youtube URL or a ID,
        * and returns back the URL for that video.
        */
        generateWatchUrl: function(videoIdOrUrl) {
            return 'https://www.youtube.com/watch?v=' + this.getIdFromUrl(videoIdOrUrl);
        },
                
        /**
        * Expects an argument that is either a youtube URL or a ID,
        * and returns back the embed URL for that video.
        */
        generateEmbedUrl: function(videoIdOrUrl) {
            return 'https://www.youtube.com/embed/' + this.getIdFromUrl(videoIdOrUrl);
        },

        changeLang(inputLang) {
            this.lang = inputLang;
    
        }
    },
    mounted() {

    }
}



</script>

<style lang="scss" scoped>


    ul {
        padding: 0;
        list-style-type: none;
        width: 600px;
        text-align: left;
        margin: 0 auto;

        li {
            border: 1px solid rgb(167, 164, 164);
            margin: 20px;
            border-radius: 10px;
        }

        img {
            border-radius: 10px;
        }
    }

    #modal {
        position: fixed;
        top: 0;
        left: 0;
        height: 100%;
        width: 100%;
        background: rgba($color: #000000, $alpha: 0.5);
        display: flex;
        justify-content: center;
        align-items: center;
    }

</style>