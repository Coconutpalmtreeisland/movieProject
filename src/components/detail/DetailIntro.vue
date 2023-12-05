<template>
    <div class="detail__intro" v-if="movieInfo"
        :style="{ backgroundImage: 'url(https://image.tmdb.org/t/p/w500' + movieInfo.poster_path + ')' }">
        <div class="container">
            <div class="left">
                <img :src="'https://image.tmdb.org/t/p/w500' + movieInfo.poster_path" alt="movieInfo.title">
            </div>
            <div class="right">
                <h3><em>영화 한줄!</em> {{ movieInfo.tagline }}</h3>
                <h2>{{ movieInfo.title }}</h2>
                <p>장르:
                    <span v-for="(genre, index) in movieInfo.genres" :key="index">
                        {{ genre.name }}<span v-if="index < movieInfo.genres.length - 1"> / </span>
                    </span>
                </p>
                <p class="desc">{{ movieInfo.overview }}</p>
                <p class="date">개봉: {{ formatDate(movieInfo.release_date) }}</p>
                <p>상영시간: {{ movieInfo.runtime }}분</p>
                <p class="average">평점: {{ movieInfo.vote_average.toFixed(1) }}점</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        movieInfo: {
            type: Object,
            required: true,
        },
    },
    methods: {
        formatDate(dateString) {
            const date = new Date(dateString);
            const year = date.getFullYear();
            const month = date.getMonth() + 1;
            const day = date.getDate();
            return `${year}년 ${month}월 ${day}일`;
        },
    }
}
</script>

<style lang="scss">
.detail__intro {
    padding: 30px;
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center;
    position: relative;

    &::before {
        position: absolute;
        content: '';
        width: 100%;
        height: 100%;
        left: 0;
        top: 0;
        background-color: #dadada36;
        backdrop-filter: blur(7px);
        z-index: 1;
    }

    .container {
        position: relative;
        justify-content: space-between;
        display: flex;
        z-index: 10;

        .left {
            width: 350px;

            img {
                width: 100%;
                vertical-align: top;
            }
        }

        .right {
            width: calc(100% - 390px);
            background-color: #ffffff1a;

            h2 {
                font-size: 30px;
                margin-bottom: 10px;
            }

            .desc {
                margin-bottom: 10px;
            }

            .date {
                display: inline-block;
                margin-right: 20px;
            }

            .average {
                display: inline-block;
                margin-bottom: 30px;
            }

            .credit {

                ul {
                    display: flex;

                    li {
                        width: 50px;

                        img {
                            width: 100%;
                        }
                    }
                }
            }
        }
    }
}
</style>