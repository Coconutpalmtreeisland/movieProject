<template>
    <header id="header" role="banner">
        <div class="header__inner">
            <div class="header__bar">
                <h1>
                    <a href="/">CinemaDream</a>
                </h1>
                <nav>
                    <ul>
                        <li><a href="#">Movie top 10</a></li>
                        <li><a href="#">Drama top 10</a></li>
                    </ul>
                </nav>
            </div>
            <!-- movieInfo가 null이 아닐 때에만 아래 내용을 렌더링합니다 -->
            <div class="detail__intro" v-if="movieInfo"
                style="background-image: url('https://image.tmdb.org/t/p/w500' + movieInfo.poster_path);">
                <div class="container">
                    <div class="left">
                        <img :src="'https://image.tmdb.org/t/p/w500' + movieInfo.poster_path" alt="movieInfo.title">
                    </div>
                    <div class="right">
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
                        <div class="credit">
                            <ul>
                                <li>
                                    <img src="https://image.tmdb.org/t/p/w500/liV9OXUeo7T19hhjFlqTELtETnW.jpg"
                                        alt="Leah Lewis">
                                </li>
                                <li>
                                    <img src="https://image.tmdb.org/t/p/w500/iPx1s7EuBEmty7MXdKSBpEBsGYT.jpg"
                                        alt="Mamoudou Athie">
                                </li>
                                <li>
                                    <img src="https://image.tmdb.org/t/p/w500/lPCmkQK76DOgkmcRjg9394QPyAu.jpg"
                                        alt="Ronnie del Carmen">
                                </li>
                                <li>
                                    <img src="https://image.tmdb.org/t/p/w500/i9m2RGrANNxidj0bVKlSs0zHPNX.jpg"
                                        alt="Shila Ommi">
                                </li>
                                <li>
                                    <img src="https://image.tmdb.org/t/p/w500/d8VKC8Ms3u9XiW4e4jsy2grP02d.jpg"
                                        alt="Wendi McLendon-Covey">
                                </li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </header>
    <main id="main">

    </main>
</template>

<script>
import { onMounted, ref } from "vue";
import { useRoute } from "vue-router";

export default {
    setup() {
        const movieInfo = ref(null);
        const route = useRoute();

        const movieInfoFetch = async () => {
            const movieId = route.params.movieId;
            const apiKey = import.meta.env.VITE_API_KEY;
            const language = "ko-KR"
            const url = `https://api.themoviedb.org/3/movie/${movieId}?api_key=${apiKey}&language=${language}`;
            try {
                const response = await fetch(url)
                const data = await response.json();
                movieInfo.value = data;
                console.log(data);
            } catch (err) {
                console.error(err);
            }
        }

        // 날짜 형식 변환 함수
        const formatDate = (dateString) => {
            const date = new Date(dateString);
            const year = date.getFullYear();
            const month = date.getMonth() + 1;
            const day = date.getDate();
            return `${year}년 ${month}월 ${day}일`;
        }

        onMounted(() => {
            movieInfoFetch();
        })
        return { movieInfo, formatDate };
    }

}
</script>

<style lang="scss">
.header__inner {
    .header__bar {
        display: flex;
        justify-content: space-between;
        width: 100%;
        height: 60px;
        background: #222;

        h1 {
            padding: 15px 25px;
            font-size: 22px;
            text-align: center;
            color: var(--white);
        }

        nav {

            ul {
                display: flex;
                justify-content: space-between;
                padding: 20px 25px;

                li {
                    display: inline;

                    a {
                        display: inline-block;
                        color: var(--white);
                        padding: 0 10px;
                    }
                }
            }
        }
    }

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


}
</style>