<template>

    <footer class="footer">

        <a href="https://github.com/Amirmadany/todo-app" class="footer-link">
            <span class="footer-link-content">GitHub</span>
            <i class="fab fa-github footer-link-icon"></i>
        </a>

    </footer>

</template>

<script>
export default {

}
</script>

<style scoped>

    .footer{
        position: fixed;
        bottom: 0;
        background: white;
        box-shadow: 0 0 15px 5px rgba(0, 0, 0, 0.11);
        width: 100%;
        padding: 0.5rem;
    }

    .footer-link{
        display: flex;
        align-items: center;
        justify-content: center;
        text-decoration: none;
        color: #111;
        font-size: 1.1rem;
        font-weight: bold;
    }

    .footer-link-content{
        transform: translateY(0.1rem);
    }

    .footer-link-icon{
        font-size: 1.5rem;
        margin-left: 0.3rem;
    }

</style>