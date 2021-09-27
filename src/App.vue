<template>

    <main class="main">

        <Tasks />

    </main>

    <Footer />

</template>

<script>
// components
import Footer from '@/components/Footer'
import Tasks from '@/components/Tasks'

export default {
    components: {
        Footer,
        Tasks
    }
}
</script>

<style>
    
    @font-face{
        font-family: 'vazir';
        src: url('./assets/Vazir.woff');
    }

    *{
        box-sizing: border-box;
        font-family: 'vazir', sans-serif;
        margin: 0;
    }

    body{
        background: #e8eaed;
    }

    .main{
        max-width: 30rem;
        margin: 1rem auto;
    }

</style>