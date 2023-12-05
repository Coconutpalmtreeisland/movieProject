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
        </div>
    </header>
    <main id="main">
        <!-- 데이터 있는지 없는지 확인 후 데이터 넣기 props..? 어쩌구 저쩌구-->
        <DetailIntro v-if="movieBasic" :movieInfo="movieBasic" />
        <DetailCredits v-if="movieCredits" :movieCredits="movieCredits" />
        <DetailImages v-if="movieImages" :movie="movieImages" />
        <DetailSimilar v-if="movieSimilar" :movie="movieSimilar" />
    </main>
</template>

<script>
import { onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';
import axios from 'axios';

import DetailIntro from "../components/detail/DetailIntro.vue"
import DetailImages from "../components/detail/DetailImages.vue"
import DetialSimilar from "../components/detail/DetialSimilar.vue"
import DetailCredits from "../components/detail/DetailCredits.vue"

export default {
    name: "MovieDeatilPage",

    components: {
        DetailIntro,
        DetailImages,
        DetialSimilar,
        DetailCredits,
    },

    setup() {
        // useEffect 선언과 같음??
        const movieBasic = ref(null);
        const movieImages = ref(null);
        const movieSimilar = ref(null);
        const movieCredits = ref(null);

        const route = useRoute();

        // 영화 기본 정보 데이터 다운 받는 동안 기다리기
        // movieId 가져오기 위해 parmas 사용 -> route 설정
        const movieId = route.params.movieId;
        const apiKey = import.meta.env.VITE_API_KEY;
        const language = "ko-KR";

        onMounted(async () => {
            try {
                const resMovieBasic = await axios.get(`https://api.themoviedb.org/3/movie/${movieId}?language=${language}&api_key=${apiKey}`);
                // ?
                movieBasic.value = resMovieBasic.data;
                console.log(resMovieBasic.data);

                const resMovieImages = await axios.get(`https://api.themoviedb.org/3/movie/${movieId}/images?&api_key=${apiKey}`);
                movieImages.value = resMovieImages.data;
                console.log(resMovieImages.data);

                const resmovieSimilar = await axios.get(`https://api.themoviedb.org/3/movie/${movieId}/similar?language=${language}&api_key=${apiKey}`);
                movieSimilar.value = resmovieSimilar.data;
                console.log(resmovieSimilar.data);

                const resmovieCredits = await axios.get(`https://api.themoviedb.org/3/movie/${movieId}/credits?language=${language}&api_key=${apiKey}`);
                movieCredits.value = resmovieCredits.data;
                console.log(resmovieCredits.data);
            } catch (err) {
                console.log(err)
            }
        })
        return { movieBasic, movieCredits };
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

    // .detail__intro {
    //     padding: 30px;
    //     background-size: cover;
    //     background-repeat: no-repeat;
    //     background-position: center;
    //     position: relative;

    //     &::before {
    //         position: absolute;
    //         content: '';
    //         width: 100%;
    //         height: 100%;
    //         left: 0;
    //         top: 0;
    //         background-color: #dadada36;
    //         backdrop-filter: blur(7px);
    //         z-index: 1;
    //     }

    //     .container {
    //         position: relative;
    //         justify-content: space-between;
    //         display: flex;
    //         z-index: 10;

    //         .left {
    //             width: 350px;

    //             img {
    //                 width: 100%;
    //                 vertical-align: top;
    //             }
    //         }

    //         .right {
    //             width: calc(100% - 390px);
    //             background-color: #ffffff1a;

    //             h2 {
    //                 font-size: 30px;
    //                 margin-bottom: 10px;
    //             }

    //             .desc {
    //                 margin-bottom: 10px;
    //             }

    //             .date {
    //                 display: inline-block;
    //                 margin-right: 20px;
    //             }

    //             .average {
    //                 display: inline-block;
    //                 margin-bottom: 30px;
    //             }

    //             .credit {

    //                 ul {
    //                     display: flex;

    //                     li {
    //                         width: 50px;

    //                         img {
    //                             width: 100%;
    //                         }
    //                     }
    //                 }
    //             }
    //         }
    //     }
    // }


}
</style>