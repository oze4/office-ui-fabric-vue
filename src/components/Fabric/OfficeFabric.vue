<template>
    <div :class="classNames.root" ref="root"></div>
</template>

<script lang="ts">
    import {getStyles} from "@/components/Fabric/OfficeFabric.styles";
    import {IOfficeFabricProps, IOfficeFabricStyleProps, IOfficeFabricStyles} from "@/components/Fabric/OfficeFabric.types";
    import {loadTheme} from "@/styling";
    import {getWindow} from "@/utility/dom";
    import {isDirectionalKeyCode} from "@/utility/keyboard";
    import {IStyleFunctionOrObject, mergeStyleSets} from "@uifabric/merge-styles";
    import {Component, Prop, Vue} from "vue-property-decorator";

    @Component
    export default class OfficeFabric extends Vue implements IOfficeFabricProps {
        @Prop({type: Boolean}) public styles?: IStyleFunctionOrObject<IOfficeFabricStyleProps, IOfficeFabricStyles>;

        private isFocusVisible!: boolean;

        private mounted() {
            const win = getWindow(this.$refs.root as HTMLElement);
            if(win) {
                win.addEventListener("mousedown", this.onMouseDown, true);
                win.addEventListener("keydown", this.onKeyDown, true);
            }
        }

        private beforeDestroy() {
            const win = getWindow(this.$refs.root as HTMLElement);
            if(win) {
                win.removeEventListener("mousedown", this.onMouseDown);
                win.removeEventListener("keydown", this.onKeyDown);
            }
        }

        private get classNames() {
            return mergeStyleSets(getStyles({
                theme: loadTheme({}),
                isFocusVisible: this.isFocusVisible,
                styles: this.styles
            }));
        }

        private onMouseDown() {
            this.isFocusVisible = true;
        }

        private onKeyDown(ev: KeyboardEvent) {
            if(isDirectionalKeyCode(ev.which)) {
                this.isFocusVisible = true;
            }
        }
    }
</script>
