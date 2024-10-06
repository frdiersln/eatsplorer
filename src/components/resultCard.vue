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
        <img :src="image" alt="Result Image" class="result-image" />
        <div class="result-details">
            <h2>{{ title }}</h2>
            <p>Likes: {{ likes }}</p>
            <p>Missed Ingredients: {{ missedIngredientsCount }}</p>
            <p>Unused Ingredients: {{ unusedIngredientsCount }}</p>
        </div>
    </div>
</template>

<style scoped>
.result-card {
    border: 1px solid #ddd;
    border-radius: 8px;
    padding: 16px;
    max-width: 300px;
    background-color: #fff3e0;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.result-image {
    width: 100%;
    border-radius: 8px;
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