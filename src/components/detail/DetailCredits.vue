<template>
    <div class="detail__credits" v-if="movieCredits">
        <ul>
            <li v-for="(credit, index) in movieCredits.cast.slice(0, 5)" :key="index">
                <img :src="'https://image.tmdb.org/t/p/w500' + credit.profile_path" :alt="credit.name">
                <p>{{ credit.name }} / {{ credit.character }}역</p>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    props: {
        movieCredits: {
            type: Object,
            required: true,
        },
    },
}
</script>

<style>
.detail__credits {

    ul {
        display: flex;

        li {

            img {
                width: 50px;
            }
        }
    }
}
</style>