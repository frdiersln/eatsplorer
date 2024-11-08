<script>
export default {
    name: 'ResultCard',
    props: {
        image: {
            type: String,
            required: true
        },
        title: {
            type: String,
            required: true
        },
        likes: {
            type: Number,
            required: true
        },
        missedIngredientsCount: {
            type: Number,
            required: true
        },
        unusedIngredientsCount: {
            type: Number,
            required: true
        }
    }
}
</script>

<template>
    <div class="result-card">
        <div class="imgWrapper">
            <img :src="image" alt="Result Image" class="result-image"/>
            <p v-if="unusedIngredientsCount != 0 && missedIngredientsCount != 0" class="info" >{{missedIngredientsCount }} Missed | {{ unusedIngredientsCount}} Unused</p>
            <p v-else-if="missedIngredientsCount != 0" class="info">{{missedIngredientsCount }} Missed</p>
            <p v-else class="info">{{unusedIngredientsCount }} Unused</p>
            <p class="like" >{{ likes }}❤️</p>
        </div>
        <div class="result-details">
            <h2>{{ title }}</h2>
        </div>
    </div>
</template>

<style scoped>
.result-card {
    border-radius: 8px;
    padding: 16px;
    width: 22rem;
    background-color: transparent;
    box-shadow: 0 0 4px rgba(0, 0, 0, 0.1);
}

.imgWrapper {
    position: relative;
    min-height: calc(100% - 89px);
}

.result-image {
    width: 100%;
    border-radius: 8px;
}

.imgWrapper .like, .imgWrapper .info {
    position: absolute;
    right: 14px;
    background-color: rgba(255, 255, 255, 0.781);
    border-radius: 8px;
    padding: 2px 6px;
}

.imgWrapper .like {
    top: 14px;
    color: red;
    font-size: 1.04em;
}

.imgWrapper .info {
    bottom: 14px;
    font-size: 0.89em;
}

.result-details {
    margin-top: 12px;
}

.result-details h2 {
    margin: 0;
    font-size: 1.5em;
    color: #333;
}

.result-details p {
    margin: 4px 0;
    color: #666;
}
</style>