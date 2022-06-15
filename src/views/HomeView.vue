<script>
export default {
    data() {
        return {
            page: 1,
            portfolio: {}
        }
    },
    methods: {
        loadPics(page) {
            this.page=page;
            fetch('http://127.0.0.1:8000/api/picture?page=' + page + '&limit=6')
                .then((res) => res.json())
                .then((json) => {
                    this.portfolio = json;
                    console.log(json);
                })
        },
        nextPage(way = '') {
            switch (way) {
                case 'next':
                    this.page++;
                    break;
                case 'prev':
                    this.page > 1 ? this.page-- : null;
                    break;
            }
            console.log('page:', this.page);
            this.loadPics(this.page);
        }
    },
    mounted() {
        this.loadPics(1);
    }
}
</script>

<template>
    <div class="container-fluid">
        <div class="row">
            <div class="col-12 text-center mt-2">
                <h1>Home</h1>
            </div>
        </div>

        <!-- début ligne pagination -->
        <div class="row">
            <div class="col-12">
                <nav aria-label="Page navigation example">
                    <ul class="pagination justify-content-center">
                        <li v-bind:class="{ 'page-item': true, 'disabled': this.page == 1 ? true : false }">
                            <a class="page-link" href="#" @click="this.nextPage('prev')">Previous</a>
                        </li>
                        <li v-if="this.page>2" class="page-item">
                        <a class="page-link" href="#" @click="this.loadPics(this.page-2)">{{this.page - 2}}</a>
                        </li>
                        <li v-if="this.page>1" class="page-item">
                        <a class="page-link" href="#" @click="this.loadPics(this.page-1)">{{this.page - 1}}</a>
                        </li>
                        <li class="page-item active"><a class="page-link" href="#" @click="this.loadPics(this.page)">{{this.page}}</a></li>
                        <li v-if="this.page<30" class="page-item"><a class="page-link" href="#" @click="this.loadPics(this.page + 1)">{{this.page + 1}}</a></li>
                        <li v-if="this.page<29" class="page-item"><a class="page-link" href="#" @click="this.loadPics(this.page + 2)">{{this.page + 2}}</a></li>
                        <li v-bind:class="{ 'page-item': true, 'disabled': this.page > 29 ? true : false }">
                            <a class="page-link" href="#" @click="this.nextPage('next')">Next</a>
                        </li>
                    </ul>
                </nav>
            </div>
        </div>
        <!-- fin ligne pagination -->
        <!-- début ligne galerie -->
        <div class="row">
            <!-- début colonne -->
            <div v-for="pic in portfolio" class="col-12 col-md-4 mb-3">
                <!-- début card -->
                <div class="card">
                    <RouterLink v-bind:to="'/portfolio/' + pic.id">
                        <img v-bind:src="pic.url" class="card-img-top" v-bind:alt="pic.title">
                    </RouterLink>
                    <div class="card-body">
                        <h5 class="card-title">{{ pic.title }}</h5>
                        <p class="card-text text-truncate">{{ pic.description }}</p>
                        <a v-bind:href="pic.url" target="_blank" class="btn btn-primary">Go somewhere</a>
                    </div>
                </div>
                <!-- fin card -->
            </div>
            <!-- fin colonne -->
        </div>
        <!-- fin ligne pagination -->
        <!-- début ligne pagination -->
        <div class="row">
            <div class="col-12">
                <nav aria-label="Page navigation example">
                    <ul class="pagination justify-content-center">
                        <li v-bind:class="{ 'page-item': true, 'disabled': this.page == 1 ? true : false }">
                            <a class="page-link" href="#" @click="this.nextPage('prev')">Previous</a>
                        </li>
                        <li v-if="this.page>2" class="page-item">
                        <a class="page-link" href="#" @click="this.loadPics(this.page-2)">{{this.page - 2}}</a>
                        </li>
                        <li v-if="this.page>1" class="page-item">
                        <a class="page-link" href="#" @click="this.loadPics(this.page-1)">{{this.page - 1}}</a>
                        </li>
                        <li class="page-item active"><a class="page-link" href="#" @click="this.loadPics(this.page)">{{this.page}}</a></li>
                        <li v-if="this.page<30" class="page-item"><a class="page-link" href="#" @click="this.loadPics(this.page + 1)">{{this.page + 1}}</a></li>
                        <li v-if="this.page<29" class="page-item"><a class="page-link" href="#" @click="this.loadPics(this.page + 2)">{{this.page + 2}}</a></li>
                        <li v-bind:class="{ 'page-item': true, 'disabled': this.page > 29 ? true : false }">
                            <a class="page-link" href="#" @click="this.nextPage('next')">Next</a>
                        </li>
                    </ul>
                </nav>
            </div>
        </div>
        <!-- fin ligne pagination -->
    </div>
</template>
