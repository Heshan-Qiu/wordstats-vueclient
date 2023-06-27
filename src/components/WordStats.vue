<template>
    <div v-if="wordStats">
        <h2>Total Words: {{ wordStats.TotalWords }}, Total Characters: {{ wordStats.TotalCharacters }}</h2>
    </div>

    <div v-if="wordStats">
        <h2>Largest 5 Words:</h2>
        <table class="stats-table">
            <tr>
                <td v-for="word in wordStats.LargestFiveWords">{{ word }}</td>
            </tr>
        </table>
    </div>

    <div v-if="wordStats">
        <h2>Smallest 5 Words:</h2>
        <table class="stats-table">
            <tr>
                <td v-for="word in wordStats.SmallestFiveWords">{{ word }}</td>
            </tr>
        </table>
    </div>

    <div v-if="wordStats">
        <h2>Most Frequent 10 Words:</h2>
        <table class="stats-table">
            <tr>
                <td v-for="item in wordStats.MostFrequentTenWords">{{ item.Key }}: {{ item.Value }}</td>
            </tr>
        </table>
    </div>

    <div v-if="wordStats">
        <h2>Characters:</h2>
        <table class="stats-table">
            <tbody>
                <tr v-for="row in chunkedItems">
                    <td v-for="cell in row">{{ cell.Key }}: {{ cell.Value }}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>
  
<script>
import axios from 'axios';

export default {
    data() {
        return {
            wordStats: null,
        };
    },
    computed: {
        chunkedItems() {
            const chunkSize = 5;
            const chunks = [];
            let index = 0;
            while (index < this.wordStats.Characters.length) {
                chunks.push(this.wordStats.Characters.slice(index, index + chunkSize));
                index += chunkSize;
            }
            return chunks;
        }
    },
    created() {
        this.fetchStats();
        setInterval(this.fetchStats, 1000);
    },
    methods: {
        fetchStats() {
            axios.get('https://wordstats20230625015043.azurewebsites.net/stats')
                .then(response => {
                    this.wordStats = response.data;
                })
                .catch(error => {
                    console.error(error);
                });
        },
    },
};
</script>

<style scoped>
.stats-table {
    width: 100%;
    border-collapse: collapse;
}

.stats-table th,
.stats-table td {
    padding: 8px;
    border: 1px solid #ccc;
}

.stats-table th {
    background-color: #f2f2f2;
    font-weight: bold;
}
</style>