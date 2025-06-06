<template>
    <div>
        <div class="toggle-slider" :style="getStyleObject">
            <label class="switch">
                <input v-model="isActive" type="checkbox" @click="setNewToggleState">
                <span class="track">
                    <span class="handle"></span>
                </span>
            </label>
        </div>
    </div>
</template>

<script>
const PROP_KEYS = {
    DIAMETER: 'diameter',
    COLOR: 'color',
    BORDER_RADIOUS: 'borderRadius',
    BORDER_WIDTH: 'borderWidth',
    WIDTH: 'width',
    HEIGHT: 'height',
    ACTIVE_COLOR: 'activeColor'
};
export default {
    name: 'toggleButton',
    props: ["options"],
    data() {
        return {
            handle: {
                diameter: 23,
                distance: 30,
                color: '#fff',
                borderRadius: "50%",
            },
            track: {
                color: '#576171',
                width: 50,
                height: 20,
                activeColor: '#2196F3',
                borderWidth: 0,
                borderRadius: '34px',
            },
            isActive: false,
        }
    },
    computed: {
        getHandleDistance: function() {
            let handleDistance = 0;
            if (this.options && this.options.handle && this.options.track) {
                handleDistance = this.options.track.width - this.options.handle.diameter;
            } else {
                handleDistance = this.handle.distance;
            }
            return handleDistance;
        },
        getStyleObject: function(){
            let styleObj = {
                '--handle-diameter' : this.handle.diameter + 'px',
                '--handle-color': this.handle.color,
                '--handle-border-radius': this.handle.borderRadius,
                '--handle-distance': this.getHandleDistance + 'px',
                '--track-color': this.track.color,
                '--track-width': this.track.width + 'px',
                '--track-height': this.track.height + 'px',
                '--track-active-color': this.track.activeColor,
                '--track-border-width': this.track.borderWidth + 'px',
                '--track-border-radius': this.track.borderRadius
            };
            return styleObj;
        }
    },
    methods: {
        setConfigData() {
            if (!this.options) {
                return;
            }

            if (this.options.handle) {
                [
                    PROP_KEYS.COLOR,
                    PROP_KEYS.DIAMETER,
                    PROP_KEYS.BORDER_RADIOUS
                ].forEach(element => {
                    this.setBindedProp('handle', element);
                });
            }

            if (this.options.track) {
                // TODO - track border width, track border radius - 24.12.18
                [
                    PROP_KEYS.COLOR,
                    PROP_KEYS.WIDTH,
                    PROP_KEYS.HEIGHT,
                    PROP_KEYS.ACTIVE_COLOR,
                    PROP_KEYS.BORDER_WIDTH,
                    PROP_KEYS.BORDER_RADIOUS
                ].forEach(element => {
                    this.setBindedProp('track', element);
                });
            }

            if (this.options.isActive) {
                this.isActive = this.options.isActive;
            }
        },
        setBindedProp(key, propToBind) {
            if (this.option[key][propToBind]) {
                this[key][propToBind] = this.option[key][propToBind];
            }
        },
        setNewToggleState() {
            this.isActive = !this.isActive;
            this.$emit('setIsActive', this.isActive);
        }
    },
    created() {
        this.setConfigData();
    }
}
</script>

<style scoped lang="scss">
.switch {
    position: relative;
    display: inline-block;
    width: var(--track-width);
    height: var(--track-height);
    top: 3px;
    input {
        display: none;
    }
    .track {
        display: flex;
        align-items: center;
        position: absolute;
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: var(--track-color);
        cursor: pointer;
        border: var(--track-border-width) solid var(--track-color);
        border-radius: var(--track-border-radius);
        transition: .4s;
        .handle {
            display: flex;
            width: var(--handle-diameter);
            height: var(--handle-diameter);
            background-color: var(--handle-color);
            border-radius: var(--handle-border-radius);
            transition: .4s;
        }
    }
    input:checked+.track {
        background-color: var(--track-active-color);
        border: var(--track-border-width) solid var(--track-active-color);
    }
    input:focus+.track {
        box-shadow: 0 0 1px var(--track-active-color);
    }
    input:checked+.track>.handle {
        transform: translateX(var(--handle-distance));
    }
}
</style>