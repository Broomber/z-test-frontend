<template src="./index.html"></template>

<script>
import { axios } from '@/utils/request'

export default {
    name: 'list',
    data () {
        return {
            newsData: {},
            newsDataTemp: {},
            searchText: '',
            isError: false,
            isLoading: true
        }
    },
    watch: {
        searchText: function () {
            this.onSearchInput()
        }
    },
    methods: {
        fetchNewsByUrl (url) {
            axios
                .get(url)
                .then(response => {
                    this.newsData = response
                })
                .catch(error => {
                    console.error(error)
                    this.isError = true
                })
                .finally(() => {
                    this.isLoading = false
                })
        },
        fetchNews () {
            this.fetchNewsByUrl('https://my-json-server.typicode.com/bigfootdary/json-news/news')
        },
        fetchNewsEmpty () {
            this.fetchNewsByUrl('https://my-json-server.typicode.com/bigfootdary/json-news/news-not-found')
        },
        fetchNewsFiltered () {
            this.fetchNewsByUrl('https://my-json-server.typicode.com/bigfootdary/json-news/news-filtered')
        },
        // fetchNewsLastPage () {
        //     this.fetchNewsByUrl('https://my-json-server.typicode.com/bigfootdary/json-news/news-last-page')
        // },
        fetchNewsMore () {
            (this.newsData.page.current < this.newsData.page.total) &&
            axios
                .get('https://my-json-server.typicode.com/bigfootdary/json-news/news-last-page')
                .then(response => {
                    this.newsData.page = response.page
                    this.newsData.list = this.newsData.list.concat(response.list)
                })
                .catch(error => {
                    console.error(error)
                    this.isError = true
                })
                .finally(() => {
                    this.isLoading = false
                })
        },
        onSearchInput (event) {
            this.isLoading = true
            if (this.searchText.length > 0) {
                this.newsDataTemp = this.newsData
                this.fetchNewsFiltered()
            } else {
                this.newsData = this.newsDataTemp
                this.newsDataTemp = {}
                this.isLoading = false
            }
        }
    },
    mounted () {
        this.fetchNews()
    },
    updated () {}
}
</script>

<style lang="scss" src="./index.scss"></style>
