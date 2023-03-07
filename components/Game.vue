<script>

import axios from 'axios'

export default {
    data() {
        return {
            frame: 'egg',
            dim: [],
            all_dims: [],
            dim_card: 'dinosaur-roar',
            digimon: {},
            current_digimon: 0,
            prev_digimon: '',
            images: {},
            wallpaper: '',
            timer: ''
        }
    },
    computed: {

    },
    mounted() {
        this.loadDim()

        axios
            .get('../dims.json')
            .then(response => {
                this.all_dims = response.data
            })
    },
    updated() {
    },
    methods: {

        async loadDim() {

            clearTimeout(this.timer)

            this.frame = 'egg'
            
            axios
                .get('../json/' + this.dim_card + '.json')
                .then(response => {
                    this.images = []
                    this.dim = response.data

                    // Load all images

                    this.wallpaper = this.newImage('../images/' + this.dim_card + '/DIM_1_.png').src

                    for (let i = 0; i < this.dim.length; i++) {
                        const images = {}

                        if (this.dim[i].images.default) {
                            images.default_1 = this.newImage('../images/' + this.dim_card + '/' + this.dim[i].images.default[0])
                            images.default_2 = this.newImage('../images/' + this.dim_card + '/' + this.dim[i].images.default[1])
                        }

                        if (this.dim[i].images.walk) {
                            images.walk_1 = this.newImage('../images/' + this.dim_card + '/' + this.dim[i].images.walk[0])
                            if (this.dim[i].images.walk[1]) {
                                images.walk_2 = this.newImage('../images/' + this.dim_card + '/' + this.dim[i].images.walk[1])
                            }
                        }

                        if (this.dim[i].images.run) {
                            images.run_1 = this.newImage('../images/' + this.dim_card + '/' + this.dim[i].images.run[0])
                            images.run_2 = this.newImage('../images/' + this.dim_card + '/' + this.dim[i].images.run[1])
                        }

                        if (this.dim[i].images.train) {
                            images.train_1 = this.newImage('../images/' + this.dim_card + '/' + this.dim[i].images.train[0])
                            images.train_2 = this.newImage('../images/' + this.dim_card + '/' + this.dim[i].images.train[1])
                        }

                        if (this.dim[i].images.happy) {
                            images.happy = this.newImage('../images/' + this.dim_card + '/' + this.dim[i].images.happy[0])
                        }

                        if (this.dim[i].images.sleep) {
                            images.sleep = this.newImage('../images/' + this.dim_card + '/' + this.dim[i].images.sleep[0])
                        }

                        if (this.dim[i].images.attack) {
                            images.attack_1 = this.newImage('../images/' + this.dim_card + '/' + this.dim[i].images.attack[0])
                            images.attack_2 = this.newImage('../images/' + this.dim_card + '/' + this.dim[i].images.attack[1])
                        }

                        if (this.dim[i].images.wallpaper) {
                            images.wallpaper = this.newImage('../images/' + this.dim_card + '/' + this.dim[i].images.wallpaper[0])
                        }

                        if (this.dim[i].images.hatching) {
                            images.hatch_1 = this.newImage('../images/' + this.dim_card + '/' + this.dim[i].images.hatching[0])
                            images.hatch_2 = this.newImage('../images/' + this.dim_card + '/' + this.dim[i].images.hatching[1])
                            images.hatch_3 = this.newImage('../images/' + this.dim_card + '/' + this.dim[i].images.hatching[2])
                            images.hatch_4 = this.newImage('../images/' + this.dim_card + '/' + this.dim[i].images.hatching[3])
                            images.hatch_5 = this.newImage('../images/' + this.dim_card + '/' + this.dim[i].images.hatching[4])
                            images.hatch_6 = this.newImage('../images/' + this.dim_card + '/' + this.dim[i].images.hatching[5])
                            images.hatch_7 = this.newImage('../images/' + this.dim_card + '/' + this.dim[i].images.hatching[6])
                            images.hatch_8 = this.newImage('../images/' + this.dim_card + '/' + this.dim[i].images.hatching[7])
                        }

                        if (images) {
                            this.images[i] = images
                        }
                    }

                    // Load Digimon
                    this.loadDigimon()
                })
        },
        loadDigimon: function() {
            this.digimon = this.dim[this.current_digimon]
            if (this.digimon.images.wallpaper == undefined && this.current_digimon == 1) {
                this.frame = 'default'
            }
        },
        newImage: function (url) {
            const image = new Image()
            image.src = url
            return image;
        }
    },
    watch: {
        current_digimon: {
            handler: function (val) {
                if (val == 0) {
                    this.frame = 'egg'
                } else {
                    if (this.prev_digimon < val) {
                        console.log(222)
                        this.frame = 'wallpaper'
                    } else {
                        this.frame = 'default'
                    }
                }

                this.prev_digimon = val
                this.loadDigimon()
            },
            deep: true
        },
        dim_card: {
            handler: function (val) {
                this.digimon = {}
                this.current_digimon = 0
                this.loadDim()
            }
        },
        frame: {
            handler: function (val) {
                var vm = this

                if (val == 'hatch') {
                    vm.timer = setTimeout(function() {
                        vm.current_digimon++
                    }, 1500);
                } else if (val == 'happy') {
                    vm.timer = setTimeout(function() {
                        vm.frame = 'default'
                    }, 500);
                } else if (val == 'train') {
                    vm.timer = setTimeout(function() {
                        vm.frame = 'default'
                    }, 1000);
                } else if (val == 'attack') {
                    vm.timer = setTimeout(function() {
                        vm.frame = 'default'
                    }, 1000);
                } else if (val == 'wallpaper') {
                    vm.timer = setTimeout(function() {
                        vm.frame = 'default'
                    }, 1000);
                }
            },
            deep: true
        }
    }
}
</script>

<template>
    <div class="" v-if="digimon">

        <form>
            <div class="field">
                <div class="input-wrap select-wrap">
                    <select v-model="dim_card" class="input">
                        <option v-for="all_dim in all_dims" :value="all_dim.file">{{ all_dim.name }}</option>
                    </select>
                </div>
            </div>

            <div class="button-group" v-if="digimon.images">
                <button type="button" class="button" @click="frame = 'hatch'" v-if="digimon.images && digimon.images.hatching && (frame == 'hatch' || frame == 'egg')" :disabled="frame == 'hatch'">Hatch</button>
                <button type="button" class="button" @click="current_digimon--" v-if="frame != 'hatch' && frame != 'egg'" :disabled="frame == 'wallpaper'">Down</button>
                <button type="button" class="button right" @click="current_digimon++" v-if="current_digimon + 1 < dim.length && frame != 'hatch' && frame != 'egg'" :disabled="frame == 'wallpaper'">Up</button>
            </div>

            <hr>

        </form>

        <div class="image-wrap">
        <div class="background" v-if="digimon.images">
            <img :src="wallpaper" alt="" />

            <div class="digimon default" v-if="frame == 'default' && digimon.images.default">
                <img :src="images[current_digimon].default_1.src" alt="" class="frame1" />
                <img :src="images[current_digimon].default_2.src" alt="" class="frame2" v-if="digimon.images.default[1]" />
            </div>

            <div class="digimon walk" v-if="frame == 'walk' && digimon.images.walk">
                <img :src="images[current_digimon].walk_1.src" alt="" :class="{ 'frame1': digimon.images.walk[1] }" />
                <img :src="images[current_digimon].walk_2.src" alt="" class="frame2" v-if="digimon.images.walk[1]" />
            </div>
            <div class="digimon run" v-if="frame == 'run' && digimon.images.run">
                <img :src="images[current_digimon].run_1.src" alt="" :class="{ 'frame1': digimon.images.run[1] }" />
                <img :src="images[current_digimon].run_2.src" alt="" class="frame2" v-if="digimon.images.run[1]" />
            </div>
            <div class="digimon train" v-if="frame == 'train' && digimon.images.train">
                <img :src="images[current_digimon].train_1.src" alt="" class="frame1" />
                <img :src="images[current_digimon].train_2.src" alt="" class="frame2" v-if="digimon.images.train[1]" />
            </div>
            <div class="digimon happy" v-if="frame == 'happy' && digimon.images.happy">
                <img :src="images[current_digimon].happy.src" alt="" />
            </div>
            <div class="digimon sleep" v-if="frame == 'sleep' && digimon.images.sleep">
                <img :src="images[current_digimon].sleep.src" alt="" />
            </div>
            <div class="digimon attack" v-if="frame == 'attack' && digimon.images.attack">
                <img :src="images[current_digimon].attack_1.src" alt="" class="frame1" />
                <img :src="images[current_digimon].attack_2.src" alt="" class="frame2" v-if="digimon.images.attack[1]" />
            </div>

            <div class="digimon hatch" v-if="(frame == 'egg' || frame == 'hatch') && digimon.images.hatching" :class="{'start': frame == 'hatch'}">
                <img :src="images[current_digimon].hatch_1.src" alt="" class="frame1" />
                <img :src="images[current_digimon].hatch_2.src" alt="" class="frame2" />
                <img :src="images[current_digimon].hatch_3.src" alt="" class="frame3" />
                <img :src="images[current_digimon].hatch_4.src" alt="" class="frame4" />
                <img :src="images[current_digimon].hatch_5.src" alt="" class="frame5" />
                <img :src="images[current_digimon].hatch_6.src" alt="" class="frame6" />
                <img :src="images[current_digimon].hatch_7.src" alt="" class="frame7" />
                <img :src="images[current_digimon].hatch_8.src" alt="" class="frame8" />
            </div>

            <div class="wallpaper" v-if="frame == 'wallpaper' && digimon.images.wallpaper">
                <img :src="images[current_digimon].wallpaper.src" alt="" />
            </div>
            
        </div>
        </div>

        <div class="info">
            <div><b>{{ digimon.name }}</b></div>
            <div v-if="digimon.stage != 'Egg' && digimon.stage"><b>Stage</b>: {{ digimon.stage }}</div>
            <div v-if="digimon.attribute"><b>Attribute</b>: {{ digimon.attribute }}</div>
            <div v-if="digimon.activity_type"><b>Activity Type</b>: {{ digimon.activity_type }}</div>
            <div class="" v-if="digimon.stats && digimon.stats.hp != 0">
                <div><b>Stats</b></div>
                <div>HP: {{ digimon.stats.hp }}</div>
                <div>BP: {{ digimon.stats.bp }}</div>
                <div>AP: {{ digimon.stats.ap }}</div>
                <div><b>Critial Hit</b>: {{  digimon.stats.critial_hit }} turns</div>
            </div>
        </div>

        <form>

            <hr>

        <div class="button-group" v-if="digimon.images">
            <button type="button" class="button" @click="frame = 'default'" :class="{'active': frame == 'default'}" v-if="digimon.images.default" :disabled="frame == 'wallpaper'">Default</button>
            <button type="button" class="button" @click="frame = 'walk'" :class="{'active': frame == 'walk'}" v-if="digimon.images.walk" :disabled="frame == 'wallpaper'">Walk</button>
            <button type="button" class="button" @click="frame = 'run'" :class="{'active': frame == 'run'}" v-if="digimon.images.run" :disabled="frame == 'wallpaper'">Run</button>
            <button type="button" class="button" @click="frame = 'train'" :class="{'active': frame == 'train'}" v-if="digimon.images.train" :disabled="frame == 'wallpaper'">Train</button>
            <button type="button" class="button" @click="frame = 'happy'" :class="{'active': frame == 'happy'}" v-if="digimon.images.happy" :disabled="frame == 'wallpaper'">Happy</button>
            <button type="button" class="button" @click="frame = 'sleep'" :class="{'active': frame == 'sleep'}" v-if="digimon.images.sleep" :disabled="frame == 'wallpaper'">Sleep</button>
            <button type="button" class="button" @click="frame = 'attack'" :class="{'active': frame == 'attack'}" v-if="digimon.images.attack" :disabled="frame == 'wallpaper'">Attack</button>
        </div>



        </form>
    </div>
</template>

<style lang="less">

.image-wrap {
    height: 160px;
    width: 80px;
    margin: 0 0 16px;
    background-color: #EEE;
}
.background {
    position: relative;
    height: 100%;
}

.digimon {
    position: absolute;
    left: 8px;
    bottom: 12px;
    width: 64px;
    height: 56px;
    display: flex;
    align-items: flex-end;
    justify-content: center;
}

.digimon img {
    position: absolute;
}

.frame1 {
    animation: frame-animation-1 1s infinite;
}

.frame2 {
    animation: frame-animation-2 1s infinite;
}

.hatch {

    img {
        opacity: 0;
    }

    .frame1 img {
        opacity: 1;
    }
}

.hatch.start {

    .frame1 {
        animation: hatch-1 1s;
    }

    .frame2 {
        animation: hatch-1 1s;
        animation-delay: 0.125s;
    }

    .frame3 {
        animation: hatch-1 1s;
        animation-delay: 0.250s;
    }

    .frame4 {
        animation: hatch-1 1s;
        animation-delay: 0.375s;
    }

    .frame5 {
        animation: hatch-1 1s;
        animation-delay: 0.500s;
    }

    .frame6 {
        animation: hatch-1 1s;
        animation-delay: 0.625s;
    }

    .frame7 {
        animation: hatch-1 1s;
        animation-delay: 0.750s;
    }

    .frame8 {
        animation: hatch-end 1s infinite;
        animation-delay: 0.875s;
    }
}

.wallpaper {
    position: absolute;
    top: 0;
    left: 0;
}

.info {
    margin: 0 0 16px;
}

@keyframes hatch-1 {
	0% { opacity: 1;}
	12.49% { opacity: 1;}
	12.5% { opacity: 0;}
	100% { opacity: 0;}
}

@keyframes hatch-end {
	0% { opacity: 1;}
	12.49% { opacity: 1;}
	12.5% { opacity: 1;}
	100% { opacity: 1;}
}

@keyframes frame-animation-1 {
	0% { opacity: 0;}
	49.99% { opacity: 0;}
	50% { opacity: 1;}
	100% { opacity: 1;}
}

@keyframes frame-animation-2 {
	0% { opacity: 1;}
	49.99% { opacity: 1;}
	50% { opacity: 0;}
	100% { opacity: 0;}
}
</style>