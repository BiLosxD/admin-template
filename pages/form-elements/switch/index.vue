<template lang="html">
    <div id="dashboard" v-if="loaded">

        <div class="box mb">
            <div class="top_box">
                <h2>Default</h2>
            </div>
            <div class="bottom_box">
                <div class="box_group_inline">
                    <div id="switch" :class="[ 'mr ten', (form.default) ? 'toggled' : '' ]">
                        <label for="switch" @click="toggle('default')"></label>
                        <input
                            type="checkbox"
                            name="switch"
                            id="switch"
                            :checked="form.default"
                        />
                        <span>Active</span>
                    </div>
                </div>
            </div>
        </div>

        <div class="box mb">
            <div class="top_box">
                <h2>Disabled</h2>
                <div class="description">
                    <p>Add <b>:class</b>: <i>disabled</i> together with switch id</p>
                </div>
            </div>
            <div class="bottom_box">
                <div class="box_group_inline">
                    <div id="switch" class="disabled">
                        <label for="switch"></label>
                        <input
                            type="checkbox"
                            name="switch"
                            id="switch"
                        />
                        <span>Disabled</span>
                    </div>
                </div>
            </div>
        </div>

        <div class="box mb">
            <div class="top_box">
                <h2>Types</h2>
                <div class="description">
                    <p>These switches are only for demo purposes. You can change switch color by adding this classes.</p>
                    <p><b>class</b>: <i>primary, secondary, success, error, warning, info, dark</i></p>
                </div>
            </div>
            <div class="bottom_box">
                <div class="box_group_inline">
                    <div id="switch" :class="[ 'toggled mr ten', data ]" v-for="(data, key) in types" :key="key">
                        <label :for="`switch_${key}`"></label>
                        <input
                            type="checkbox"
                            :name="`switch_${key}`"
                            :id="`switch_${key}`"
                            checked="true"
                        />
                        <span>{{ data }}</span>
                    </div>
                </div>
            </div>
        </div>

    </div>
</template>

<script>
    export default {
        data () {
            return {
                loaded: false,
                types: ['primary', 'secondary', 'success', 'cancel', 'warning', 'info', 'dark'],
                form: {
                    default: false
                }
            }
        },
        methods: {
            toggle (type) {
                const me = this
                switch (type) {
                    case 'default':
                        me.form.default ^= true
                        break
                }
            },
            initialization (event) {
                const me = this
                if (document.readyState != 'interactive') {
                    setTimeout( () => {
                        me.$store.commit('global/loader/checkLoader', { status: false })
                        me.loaded = true
                        document.body.classList.remove('no_scroll', 'no_click')
                    }, 1000)
                }
            }
        },
        mounted () {
            const me = this
            me.initialization()
        },
        asyncData ({ store }) {
            store.commit('global/settings/populateTitle', { title: 'Switch' })
            store.commit('global/loader/checkLoader', { status: true })
        },
        beforeMount () {
            window.addEventListener('load', this.initialization)
        },
        beforeDestroy () {
            window.removeEventListener('load', this.initialization)
        }
    }
</script>
