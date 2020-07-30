<template>
    <div class="home">
        <div class="news-design container">
            <div class="news-design__item" v-for="(article, index) in articles" :key="index">
                <div class="row">
                    <div class="col-4">
                        <div class="news-design__image">
                            <img :src="article.image" alt="">
                        </div>
                    </div>
                    <div class="col-8">
                        <div class="news-content news-design__content">
                            <div class="row">
                                <div class="news-content__title">
                                    <h3>{{article.title}}</h3>
                                </div>

                            </div>
                            <div class="row">
                                <div class="news-content__description">
                                    <p>{{article.description}}</p>
                                </div>
                            </div>
                            <div class="row">
                                <div class="news-content__footer">
                                    <div class="news-content__pubslish">
                                        {{article.date}}
                                    </div>


                                    <div class="news-content__link">
                                        <a :href="article.link" target="_blank" class="btn btn-primary">Читать...</a>
                                    </div>
                                </div>

                            </div>
                        </div>
                    </div>

                </div>

            </div>
        </div>
    </div>
</template>

<script>


    export default {
        name: 'Home',
        data() {
            return {
                articles: [],
            }
        },
        created() {

            const RSS_URL = `https://m.lenta.ru/rss`;

            fetch(RSS_URL)
                .then(response => response.text())
                .then(str => new window.DOMParser().parseFromString(str, "text/xml"))
                .then(data => {
                    const items = data.querySelectorAll("item");
                    let index = 0;
                    items.forEach(el => {
                        if (index < 20) {
                            this.articles.push(
                                {
                                    title: el.querySelector("title").innerHTML,
                                    description: el.querySelector("description").textContent,
                                    link: el.querySelector("link").innerHTML,
                                    image: el.childNodes[11].getAttribute('url'),
                                    date: el.querySelector('pubDate').innerHTML.split('+')[0]
                                }
                            )
                            index += 1;
                        }
                    })

                })

        },
    }
</script>

<style lang="scss" scoped>
    .news-design__item {
        margin: 1em;
        border: 1px solid #eaeaea;
        padding: 0.8em;

        .news-design__image img {
            max-width: 100%;
        }

        .news-content.news-design__content {
            word-break: break-word;
            text-align: left;

            .news-content__description {
                font-size: 14px;
                padding: .5em;
                text-overflow: ellipsis;
                white-space: pre-wrap;
                display: flex;
            }
        }

        .news-content__footer{
            width: 95%;
            display: flex;
            flex-direction: row;
            justify-content: space-between;
            align-items: center;

            .news-content__pubslish {
                font-size: 10px;
                color: #848484;
                align-self: flex-end;
            }
        }
    }
</style>