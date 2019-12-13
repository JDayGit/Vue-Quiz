<template>
    <div>
         <b-nav tabs>
             <b-nav-item disabled>Vue Quiz</b-nav-item>
             <b-nav-item disabled>Counter: {{ numCorrect }}/{{ numTotal }}</b-nav-item>
         </b-nav>
    </div>
</template>

<script>
export default {
    name: "Header",
    props: [
        'numCorrect',
        'numTotal',
    ]
}
</script>

<style scoped>
.nav {
    margin-bottom: 5vh;
}
.nav-item {
    font-size:20px;
    font-weight:500;
}
</style>