<script setup>
import { RouterLink, RouterView } from 'vue-router'
import navbar from '../components/navbar.vue'
</script>

<template>
    <main>
        <navbar />
        <div style="padding: 5rex">
            <div>
                <h4>My Task</h4>
            </div>
            <div style="padding: 5rex">
                <div class="card" style="width: 18rem;">
                    <img src="https://tse2.mm.bing.net/th?id=OIP.VvPs5fCe9dinwDAbtmmXsQHaEK&pid=Api&P=0&h=180" class="card-img-top" alt="...">
                    <div class="card-body">
                        <h5 class="card-title">Posterior Shoulder Stretch</h5>
                        <!-- <p class="card-text">Some quick example text to build on the card title and make up the bulk of
                            the
                            card's content.</p> -->
                    </div>
                    <ul class="list-group list-group-flush">
                        <li class="list-group-item">Disease: Frozen Shoulder</li>
                        <li class="list-group-item">Repetition: 10</li>
                        <!-- <li class="list-group-item">A third item</li> -->
                    </ul>
                    <div class="card-body">
                        <button type="button" class="btn btn-success">Practise</button>
                        <!-- <a href="#" class="card-link">Card link</a>
                        <a href="#" class="card-link">Another link</a> -->
                    </div>
                </div>
            </div>
        </div>
    </main>

</template>