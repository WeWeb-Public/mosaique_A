<template>
    <div class="mosaique_A">
        <!-- wwManager:start -->
        <wwSectionEditMenu v-bind:sectionCtrl="sectionCtrl"></wwSectionEditMenu>
        <!-- wwManager:end -->
        <wwObject class="background" v-bind:ww-object="section.data.background" ww-category="background"></wwObject>
        <div class="container section-padding">
            <div class="row">
                <div class="title-container">
                    <h1 class="section-title">
                        <wwObject v-bind:ww-object="section.data.title"></wwObject>
                    </h1>
                    <div class="section-subtitle">
                        <wwObject v-bind:ww-object="section.data.subtitle"></wwObject>
                    </div>
                </div>
            </div>
            <div class="row padding0-20">
                <div class="block" :class="{'left': !(index % 4)}" v-for="(block, index) in section.data.blocks" :key="block.uniqueId">
                    <div class="block-img-container">
                        <wwObject v-bind:ww-object="block.img"></wwObject>
                    </div>
                    <!-- wwManager:start -->
                    <div v-show="editMode" class="edit-button-top-left" @click="remove(section.data.blocks, { index })">
                        <i class="wwi wwi-delete" aria-hidden="true"></i>
                    </div>
                    <!-- wwManager:end -->
                </div>
                <!-- wwManager:start -->
                <div v-show="editMode" class="block add-block-container" :class="{'left': !(section.data.blocks.length % 4)}">
                    <wwLayoutPlus @click="add(section.data.blocks, getNewBlock())"></wwLayoutPlus>
                </div>
                <!-- wwManager:end -->
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "__COMPONENT_NAME__",
    props: {
        sectionCtrl: Object
    },
    data() {
        return {}
    },
    computed: {
        section() {
            return this.sectionCtrl.get();
        },
        // wwManager:start
        editMode() {
            return this.sectionCtrl.getEditMode() == 'CONTENT'
        }
        // wwManager:end
    },
    created() {
        //Initialize section data
        this.section.data = this.section.data || {};
        this.section.data.blocks = this.section.data.blocks || [this.getNewBlock()];

        if (!this.section.data.background) {
            this.section.data.background = wwLib.wwObject.getDefault({ type: 'ww-color' });
        }
        if (!this.section.data.title) {
            this.section.data.title = wwLib.wwObject.getDefault({ type: 'ww-text' });
        }
        if (!this.section.data.subtitle) {
            this.section.data.subtitle = wwLib.wwObject.getDefault({ type: 'ww-text' });
        }
        this.sectionCtrl.update(this.section);
    },
    methods: {
        getNewBlock() {
            return {
                img: wwLib.wwObject.getDefault({ type: 'ww-image', ratio: 100 })
            }
        },
        // wwManager:start
        add(array, elem) {
            array.push(elem);
            this.sectionCtrl.update(this.section);
        },
        remove(array, options) {
            array.splice(options.index, 1);
            this.sectionCtrl.update(this.section);
        }
        // wwManager:end
    }
};
</script>

<style lang="scss" scoped>
.mosaique_A {
    position: relative;

    .container {
        width: 100%;
        position: relative;
    }

    .background {
        position: absolute;
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
    }

    .padding0-20 {
        padding: 0px 20px;
    }

    .section-padding {
        padding: 30px 15px
    }

    .row {
        display: -ms-flexbox;
        display: flex;
        -ms-flex-wrap: wrap;
        flex-wrap: wrap;
        margin-right: -15px;
        margin-left: -15px;
    }

    .block {
        position: relative;
        width: 100%;
        min-height: 1px;
        -ms-flex: 0 0 50%;
        flex: 0 0 50%;
        max-width: 50%;
        padding: 0px 5px;
        margin-top: 8px;
        margin-bottom: 5px;
    }

    .title-container {
        position: relative;
        width: 100%;
        min-height: 1px;
        padding-right: 15px;
        padding-left: 15px;
        -ms-flex: 0 0 100%;
        flex: 0 0 100%;
        max-width: 100%;
    }
    
    /* wwManager:start */
    .edit-button-top-left {
        position: absolute;
        left: -5px;
        top: -13px;
        width: 26px;
        height: 26px;
        display: flex;
        align-items: center;
        justify-content: center;
        border-radius: 100%;
        text-align: center;
        font-size: 18px;
        line-height: 36px;
        cursor: pointer;
        pointer-events: all;
        z-index: 3;
        color: white;
        background-color: #E73055;
        background: linear-gradient(to right, #E73055 0%, rgb(175, 33, 61) 100%);
    }

    .add-block-container {
        display: flex;
        justify-content: center;
        align-items: center;
    }
    /* wwManager:end */
}

@media (min-width: 768px) {
    .mosaique_A {
        .section-padding {
            padding: 50px 30px
        }
    }
}

@media (min-width: 992px) {
    .mosaique_A {
        .section-padding {
            padding: 75px 50px
        }
        .block {
            -ms-flex: 0 0 16.666666%;
            flex: 0 0 16.666666%;
            max-width: 16.666666%;
            &.left {
                margin-left: 16.666666%;
            }
        }
        .title-container {
            -ms-flex: 0 0 66.666666%;
            flex: 0 0 66.666666%;
            max-width: 66.666666%;
            margin-left: 16.666666%;
        }
    }
}

@media (min-width: 1200px) {}
</style>
