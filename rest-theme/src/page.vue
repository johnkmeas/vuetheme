<style>

</style>

<template>
    <div class="page">
        <h1 class="entry-title">{{ page.title.rendered }}</h1>
        <div v-if="page.id === 5">
            ha
            <about-component></about-component>
        </div>
        <div v-if="page.id === 2">
            <h2>{{variable}}</h2>
            This is the sample page stuff
        </div>

        <h3>{{page.id}}</h3>
        <h3>{{page.slug}}</h3>


        <div class="entry-content">
            {{{ page.content.rendered }}}
        </div>
    </div>
</template>

<script>
// import About from './about.vue'
import aboutComponent from './about.vue';
    export default {
        ready() {
            this.getPage();
        },

        data() {
            return {
                page: {
                    id: null,
                    slug: '',
                    title: { rendered: '' },
                    content: { rendered: '' },
                },
                variable: wp.some_var
            }
        },

        methods: {
            getPage() {
                console.log('this.$route', this.$route)
                this.$http.get(wp.root + 'wp/v2/pages/' + this.$route.postId).then(function(response) {
                    this.page = response.data;
                    this.$dispatch('page-title', this.page.title.rendered);
                }, function(response) {
                    console.log(response);
                });
            }
        },

        route: {
            canReuse() {
                return false;
            }
        },
        components: {
            aboutComponent
        }
    }
</script>