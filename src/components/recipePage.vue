<script>

document.addEventListener('click', function (event) {
    if (event.target.classList.contains('backDrop')) {
        document.querySelector('.backDrop').style.display = 'none'
        document.getElementById('recipePage').style.display = 'none'
    }
})

</script>

<template>
    <div class="backDrop">
    </div>
        <div id="recipePage">
            <h1 id="title"> Title </h1>
            <img id="image" src="" alt="" />
            <ul>
                <li><strong>Servings:</strong> <p id="servings"></p></li>
                <li><strong>Ready in:</strong> <p id="readyIn"></p></li>
                <li><strong>Health Score:</strong> <p id="healthScore"></p></li>
                <li><strong>Source:</strong> <a id="source" href="" target="_blank"></a></li>
            </ul>
            <p id="summary"> Summary </p>
        </div>
</template>

<style scoped>

.backDrop {
    display: none;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    z-index: 1;
}

#recipePage {
    display: none;
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    max-width: 100dvw;
    min-width: 41dvw;
    max-height: 66dvh;
    overflow: auto;
    padding: 4rem;
    border: 1px solid #ddd;
    border-radius: 8px;
    background-color: #fff;
    z-index: 2;
}

#title{
    text-align: center;
}

#image{
    display: block;
    margin: 0 auto;
}

#recipePage img {
    max-width: 100%;
    height: auto;
    border-radius: 8px;
}

#recipePage ul {
    list-style-type: none;
    padding: 0;
    display: flex;
    gap: 1rem;
    justify-content: center;
}

#recipePage li {
    display: flex;
}

#recipePage li strong, #recipePage li a{
    margin: 1rem 0;
}
</style>