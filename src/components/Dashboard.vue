<template>
	<table>
	    <tbody>
	        <tr>
	            <th>ID</th>
	            <th>Title</th>
	            <th>Count</th>
	            <th>Actions</th>
	        </tr>
	        <template v-for="movie in movies">
	            <tr v-bind:key="movie.id">
	                <td>{{ movie.id }}</td>
	                <td>{{ movie.title }}</td>
	                <td>{{ movie.count }}</td>
                    <button @click="increaseCount(movie)">Increase Count</button>
	            </tr>
	        </template> 
	    </tbody>
	    <div>
	    	<input type="text" placeholder="enter movie title..." v-model="title" />
	    	<button @click="postMovie()">Add movie</button>
	    </div>
	</table>
</template>

<script>
import axios from 'axios';

export default {

    data() {
        return {
            movies: {},
            title:''
            
        }
    },
    async created () {
	    const response = await axios.get('http://localhost/bad-puns/public/index.php/movies');	
	    this.movies = response.data;
	    console.log(response); 
    },
    methods: {
        async increaseCount(movie) {
            const response = await axios.post('http://localhost/bad-puns/public/index.php/movies/' + movie.id + '/count');
            movie.count = response.data.count;
        },
        async postMovie(){
        	const response = await axios.post('http://localhost/bad-puns/public/index.php/movies',this.$data);
        	this.title = '';
        	this.movies.push(response.data);
        }
    }
}
</script>
