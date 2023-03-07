<script>
import Game from '../components/Game.vue'

export default {
    components: {
        Game
    }
}
</script>

<template>
    <section>
        <div class="container">
            <Game />
        </div>
    </section>
</template>

<style lang="less">
* {
    box-sizing: border-box;
}

img {
    display: block;
    max-width: 100%;
    margin: 0;
}

body {
    margin: 0;
    font-size: 16px;
    line-height: 1.2;
    font-family: Arial, Helvetica, sans-serif;
}

h1, h2, h3, h4, h5, h6 {
    margin: 0;
    font-size: 16px;
    line-height: 1.6;
}

.container {
    width: 100%;
    max-width: 1400px;
    padding: 0 16px;
}

section {
    padding: 16px 0;
}

form {
    display: flex;
    flex-wrap: wrap;
    flex-direction: column;
    gap: 16px;
    margin: 0 0 16px;
}

.button-group {
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    gap: 16px;
}

.button {
    font-size: 14px;
    line-height: 40px;
    -webkit-appearance: none;
    display: inline-flex;
    background: #000;
    color: #FFF;
    padding: 0 24px;
    border-radius: 3px;
    border: 0;

    @media (max-width: 640px) {
        font-size: 16px;
        line-height: 48px;
    }

    &.right {
        margin-left: auto;
    }

    &.active {
        background: red;
    }

    &[disabled] {
        background: #ddd;
        color: #888;

        &:hover {
            cursor: default;
        }
    }

    &:hover {
        cursor: pointer;
    }
}

.input {
    -webkit-appearance: none;
    font-family: Arial, Helvetica, sans-serif;
    width: 100%;
    font-size: 16px;
    line-height: 24px;
    padding: 7px 16px;
    border: 1px solid #000;
    background-color: #FFF;
    color: #000;
    border-radius: 3px;

    @media (max-width: 640px) {
        padding: 11px 16px;
    }
}

hr {
    margin: 0;
    border: 0;
    border-top: 1px solid #000;
}
</style>