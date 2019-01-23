/*****************************************************************************
 * Open MCT, Copyright (c) 2014-2018, United States Government
 * as represented by the Administrator of the National Aeronautics and Space
 * Administration. All rights reserved.
 *
 * Open MCT is licensed under the Apache License, Version 2.0 (the
 * "License"); you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 * http://www.apache.org/licenses/LICENSE-2.0.
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
 * WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
 * License for the specific language governing permissions and limitations
 * under the License.
 *
 * Open MCT includes source code licensed under additional open source
 * licenses. See the Open Source Licenses file (LICENSES.md) included with
 * this source code distribution or the Licensing information page available
 * at runtime from the About dialog for additional information.
 *****************************************************************************/
 <template>
 <div class="l-preview-window">
    <div class="l-preview-window__object-name l-browse-bar__object-name--w" :class="type.cssClass">
        <span class="l-browse-bar__object-name">
            {{ domainObject.name }}
        </span>
        <context-menu-drop-down :object-path="objectPath"></context-menu-drop-down>
    </div>
    <div class="l-preview-window__object-view">
        <div ref="objectView">
    </div>
    </div>
</div>
 </template>
<style lang="scss">
    @import '~styles/sass-base';

    .l-preview-window {
        display: flex;
        flex-direction: column;
        position: absolute;
        top: 0; right: 0; bottom: 0; left: 0;

        > * + * {
            margin-top: $interiorMargin;
        }

        &__object-name {
            flex: 0 0 auto;
        }

        &__object-view {
            flex: 1 1 auto;

            > div:not([class]) {
                // Target an immediate child div without a class and make it display: contents
                display: contents;
            }
        }
    }
</style>

 <script>
    import ContextMenuDropDown from '../../ui/components/contextMenuDropDown.vue';

    export default {
        components: {
            ContextMenuDropDown
        },
        inject: [
            'openmct',
            'objectPath'
        ],
        data() {
            let domainObject = this.objectPath[0];
            let type = this.openmct.types.get(domainObject.type);

            return {
                domainObject: domainObject,
                type: type
            };
        },
        mounted() {
            let viewProvider = this.openmct.objectViews.get(this.domainObject)[0];
            this.view = viewProvider.view(this.domainObject);
            this.view.show(this.$refs.objectView);
        },
        destroy() {
            this.view.destroy();
        }
    }
 </script>