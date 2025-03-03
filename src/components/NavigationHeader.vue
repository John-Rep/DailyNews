<template>
        <nav class="header">
            <ul>
                <li><a href="#" @click.prevent="$emit('navigate', 'add-feed')">Add Feed</a></li>
                <li><a href="#" @click.prevent="$emit('navigate', 'list-feeds')">List Feeds</a></li>
                <li><a href="#" @click.prevent="$emit('navigate', 'preferences')">Preferences</a></li>
            </ul>
        </nav>
</template>

<style scoped>
    .header {
        background: #333;
        width: 100vw;
        color: white;
        padding: 15px;
        text-align: center;
        position: fixed;
    }
    nav ul {
        list-style: none;
        padding: 0;
        margin: 0;
        display: flex;
        justify-content: center;
        gap: 20px;
    }
    nav ul li {
        display: inline;
    }
    nav ul li a {
        color: white;
        text-decoration: none;
        font-weight: bold;
        transition: color 0.3s;
    }
    nav ul li a:hover {
        color: #28a745;
    }
</style>
  