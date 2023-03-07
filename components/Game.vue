<script>

import axios from 'axios'

export default {
    data() {
        return {
            frame: 'default',
            dim: [],
            dim_card: 'dinosaur-roar',
            digimon: {},
            current_digimon: 13,
            prev_digimon: ''
        }
    },
    computed: {

    },
    mounted() {
        this.loadDim()
    },
    updated() {
    },
    methods: {

        async loadDim() {
            axios
                .get('../json/' + this.dim_card + '.json')
                .then(response => {
                    this.dim = response.data

                    this.loadDigimon()
                })
        },
        loadDigimon: function() {
            this.digimon = this.dim[this.current_digimon]
            if (this.digimon.images.wallpaper == undefined && this.current_digimon == 1) {
                this.frame = 'default'
            }
        }
    },
    watch: {
        current_digimon: {
            handler: function (val) {
                if (val == 0) {
                    this.frame = 'egg'
                } else {
                    if (this.prev_digimon < val) {
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
                    setTimeout(function() {
                        vm.current_digimon++
                    }, 2000);
                } else if (val == 'happy') {
                    setTimeout(function() {
                        vm.frame = 'default'
                    }, 500);
                } else if (val == 'train') {
                    setTimeout(function() {
                        vm.frame = 'default'
                    }, 1000);
                } else if (val == 'attack') {
                    setTimeout(function() {
                        vm.frame = 'default'
                    }, 1000);
                } else if (val == 'wallpaper') {
                    setTimeout(function() {
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
        <div class="background" v-if="digimon.images">
            <img :src="'../images/' + dim_card + '/DIM_1_.PNG'" alt="" />

            <div class="digimon default" v-if="frame == 'default' && digimon.images.default">
                <img :src="'../images/' + dim_card + '/' + digimon.images.default[0]" alt="" class="frame1" />
                <img :src="'../images/' + dim_card + '/' + digimon.images.default[1]" alt="" class="frame2" v-if="digimon.images.default[1]" />
            </div>
            <div class="digimon walk" v-if="frame == 'walk' && digimon.images.walk">
                <img :src="'../images/' + dim_card + '/' + digimon.images.walk[0]" alt="" :class="{ 'frame1': digimon.images.walk[1] }" />
                <img :src="'../images/' + dim_card + '/' + digimon.images.walk[1]" alt="" class="frame2" v-if="digimon.images.walk[1]" />
            </div>
            <div class="digimon run" v-if="frame == 'run' && digimon.images.run">
                <img :src="'../images/' + dim_card + '/' + digimon.images.run[0]" alt="" :class="{ 'frame1': digimon.images.run[1] }" />
                <img :src="'../images/' + dim_card + '/' + digimon.images.run[1]" alt="" class="frame2" v-if="digimon.images.run[1]" />
            </div>
            <div class="digimon train" v-if="frame == 'train' && digimon.images.train">
                <img :src="'../images/' + dim_card + '/' + digimon.images.train[0]" alt="" class="frame1" />
                <img :src="'../images/' + dim_card + '/' + digimon.images.train[1]" alt="" class="frame2" v-if="digimon.images.train[1]" />
            </div>
            <div class="digimon happy" v-if="frame == 'happy' && digimon.images.happy">
                <img :src="'../images/' + dim_card + '/' + digimon.images.happy[0]" alt="" />
            </div>
            <div class="digimon sleep" v-if="frame == 'sleep' && digimon.images.sleep">
                <img :src="'../images/' + dim_card + '/' + digimon.images.sleep[0]" alt="" />
            </div>
            <div class="digimon attack" v-if="frame == 'attack' && digimon.images.attack">
                <img :src="'../images/' + dim_card + '/' + digimon.images.attack[0]" alt="" class="frame1" />
                <img :src="'../images/' + dim_card + '/' + digimon.images.attack[1]" alt="" class="frame2" v-if="digimon.images.attack[1]" />
            </div>

            <div class="digimon hatch" v-if="(frame == 'egg' || frame == 'hatch') && digimon.images.hatching" :class="{'start': frame == 'hatch'}">
                <img :src="'../images/' + dim_card + '/' + digimon.images.hatching[0]" alt="" class="frame1" />
                <img :src="'../images/' + dim_card + '/' + digimon.images.hatching[1]" alt="" class="frame2" />
                <img :src="'../images/' + dim_card + '/' + digimon.images.hatching[2]" alt="" class="frame3" />
                <img :src="'../images/' + dim_card + '/' + digimon.images.hatching[3]" alt="" class="frame4" />
                <img :src="'../images/' + dim_card + '/' + digimon.images.hatching[4]" alt="" class="frame5" />
                <img :src="'../images/' + dim_card + '/' + digimon.images.hatching[5]" alt="" class="frame6" />
                <img :src="'../images/' + dim_card + '/' + digimon.images.hatching[6]" alt="" class="frame7" />
                <img :src="'../images/' + dim_card + '/' + digimon.images.hatching[7]" alt="" class="frame8" />
            </div>

            <div class="wallpaper" v-if="frame == 'wallpaper' && digimon.images.wallpaper">
                <img :src="'../images/' + dim_card + '/' + digimon.images.wallpaper[0]" alt="" />
            </div>
        </div>

        <div class="info">
            <div><b>{{ digimon.name }}</b></div>
            <div>{{ dim_card.replace('-', ' ').replace(/(^\w|\s\w)/g, m => m.toUpperCase()) }}</div>
            <div v-if="digimon.stage != 'Egg'">Stage: {{ digimon.stage }}</div>
            <div v-if="digimon.attribute">Attribute: {{ digimon.attribute }}</div>
            <div v-if="digimon.activity_type">Activity Type: {{ digimon.activity_type }}</div>
            <div class="" v-if="digimon.stats">
                <div><b>Stats</b></div>
                <div>HP: {{ digimon.stats.hp }}</div>
                <div>BP: {{ digimon.stats.bp }}</div>
                <div>AP: {{ digimon.stats.ap }}</div>
                <div>Critial Hit: {{  digimon.stats.critial_hit }} turns</div>
            </div>
        </div>

        <form>

        <div class="button-group" v-if="digimon.images">
            <div class="button" @click="frame = 'default'" v-if="digimon.images.default">Default</div>
            <div class="button" @click="frame = 'walk'" v-if="digimon.images.walk">Walk</div>
            <div class="button" @click="frame = 'run'" v-if="digimon.images.run">Run</div>
            <div class="button" @click="frame = 'train'" v-if="digimon.images.train">Train</div>
            <div class="button" @click="frame = 'happy'" v-if="digimon.images.happy">Happy</div>
            <div class="button" @click="frame = 'sleep'" v-if="digimon.images.sleep">Sleep</div>
            <div class="button" @click="frame = 'attack'" v-if="digimon.images.attack">Attack</div>
            <div class="button" @click="frame = 'wallpaper'" v-if="digimon.images.wallpaper">Wallpaper</div>

            <div class="button" @click="frame = 'hatch'" v-if="digimon.images.hatching">Hatch</div>
        </div>

        <div class="button-group" v-if="frame != 'hatch' && frame != 'egg'">
            <div class="button" @click="current_digimon--" v-if="frame != 'hatch' && frame != 'egg'">Down</div>
            <div class="button" @click="current_digimon++" v-if="current_digimon + 1 < dim.length && frame != 'hatch' && frame != 'egg'">Up</div>
        </div>

        <div class="field">
            <div class="input-wrap select-wrap">
                <select v-model="dim_card" class="input">
                    <option value="dinosaur-roar">Dinosaur Roar</option>
                    <option value="guilmon-gp">Guilmon GP</option>
                    <option value="ancient-warriors">Ancient Warriors</option>
                </select>
            </div>
        </div>

        </form>
    </div>
</template>

<style lang="less">
.background {
    position: relative;
    margin: 0 0 16px;
}

.digimon {
    position: absolute;
    left: 8px;
    bottom: 8px;
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