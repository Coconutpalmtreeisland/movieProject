<template>
    <section class="movie__search">
        <h2 class="blind">검색하기</h2>
        <input type="search" v-model="searchKeyword" placeholder="검색어를 입력하세요" ref="inputRef" @keyup.enter="searchMovies" />
        <button type="submit" @click.prevent="searchMovies">검색</button>
    </section>
</template>

<script>
export default {

}
</script>