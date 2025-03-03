
<template>
    <div class="form-container">
        <h2>Add New RSS Feed</h2>
        <form action="#" method="POST">
            <label for="feed-url">RSS Feed URL:</label>
            <input type="url" id="feed-url" name="feed-url" required>

            <label for="feed-title">Title:</label>
            <input type="text" id="feed-title" name="feed-title" required>

            <label for="description">Description:</label>
            <textarea id="description" name="description" rows="4"></textarea>

            <button type="submit">Add Feed</button>
        </form>
    </div>
</template>

<style scoped>
    .form-container {
        background: white;
        padding: 20px;
        border-radius: 10px;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        max-width: 400px;
    }
    h2 {
        text-align: center;
        color: black;
    }
    label {
        color: black;
        font-weight: bold;
        display: block;
        margin-top: 10px;
    }
    input, textarea, select {
        width: 100%;
        padding: 8px;
        margin-top: 5px;
        border: 1px solid #ccc;
        border-radius: 5px;
    }
    button {
        background: #28a745;
        color: white;
        padding: 10px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        margin-top: 15px;
        width: 100%;
    }
    button:hover {
        background: #218838;
    }
</style>