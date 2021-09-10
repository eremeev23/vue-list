<template>
  <div class="conteiner">
      <ul class="list">
            <li class="list__item" v-for="item in messages__data__all" 
                                  :key="item.id"
                                  :item_data="item"
            >
                <div class="list__item__box">
                    <span>{{ date(item.date) }} </span> /
                    <span class="author"> {{ item.authorName }} </span> /
                    <a :href="`${item.authorUrl}`"> {{ item.authorUrl }}</a>
                    <p>{{ item.content }}</p>
                </div>
                <hr>
            </li>
      </ul>
  </div>
</template>

<script>
import moment from 'moment'
export default {
    props: {
        messages__data__first: {
            type: Array,
            default: () => []
        },
        messages__data__rest: {
            type: Array,
            default: () => []
        },
        messages__data__all: {
            type: Array,
            default: () => []
        },
    },
    methods: {
        date: function (date) {
            moment.locale('ru');
            return moment(date).format('h:mm:ss, D MMMM YYYY г.');
        },
        scrollMore: function () {
            const windowHeight = document.documentElement.clientHeight;
            const loadMoreItems = document.querySelector('.lsit');
            const loadMore = loadMoreItems.getBoundingClientRect().top + pageYOffset;
            const loadMoreItemsHeight = loadMoreItems.offsetHeight;

            if (pageYOffset > (loadMore + loadMoreItemsHeight) - windowHeight) {
                getContent();
            }

            async function getContent() {
                if (!document.querySelector('.loading-icon')) {
                    loadMoreItems.insertAdjacentHTML(
                        'beforeend',
                        '<span class="loading-icon">loading...</span>'
                    );
                }
                loadMoreItems.classList.add('_loading');

                let response = await fetch('http://localhost:3001/data', {
                    method: 'GET',
                });
                if (response.ok) {
                    let result = await response.text();
                    loadMoreItems.insertAdjacentHTML('beforeend', result);
                }

            }
        }
    },
    mounted() {

        addEventListener('scroll', this.scrollMore);     
    }
}
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
.list__item__box {
    margin: 5vh 5vw;
}
.list__item__box a {
    color: #6f7575;
    text-decoration: none;
}
.list__item__box p {
    margin: 3vh 0;
}
.author {
    font-weight: bold;
    color: #834e1d;
}
</style>