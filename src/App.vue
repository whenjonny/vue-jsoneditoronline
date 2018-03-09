<template>
  <div id="app">
    <v-app>
      <v-content class="page">
        <v-container fluid
                     fill-height
                     grid-list-lg>
          <v-layout row
                    wrap>
            <v-flex xs12
                    lg6
                    class="editor-container">
              <div class="editor-wrap">
                <v-jsoneditor class="editor editor-code"
                              :value="codeValue"
                              :options="optionsCode"
                              @input="codeChanged">
                </v-jsoneditor>
                <v-btn fab
                       small
                       bottom
                       right
                       absolute
                       color="primary"
                       class="editor-transform"
                       @click="transformCodeToTree">
                  <v-icon>subdirectory_arrow_right</v-icon>
                </v-btn>
              </div>
            </v-flex>
            <v-flex xs12
                    lg6
                    class="editor-container">
              <div class="editor-wrap">
                <v-jsoneditor class="editor editor-tree"
                              :value="treeValue"
                              :options="optionsTree"
                              @input="treeChanged">
                </v-jsoneditor>
                <v-btn fab
                       small
                       bottom
                       left
                       absolute
                       color="primary"
                       class="editor-transform"
                       @click="transformTreeToCode">
                  <v-icon>subdirectory_arrow_left</v-icon>
                </v-btn>
              </div>
            </v-flex>
          </v-layout>
        </v-container>
      </v-content>
    </v-app>
  </div>
</template>

<script>
import { clone } from 'lodash';

export default {
  name: 'App',
  data() {
    return {
      codeValue: {},
      codeJSON: null,
      treeValue: {},
      treeJSON: null,
      optionsCode: {
        mode: 'code',
        modes: null,
      },
      optionsTree: {
        mode: 'tree',
        modes: null,
      },
    };
  },
  methods: {
    codeChanged(value) {
      this.codeJSON = value;
    },
    treeChanged(value) {
      this.treeJSON = value;
    },
    transformCodeToTree() {
      this.treeValue = clone(this.codeJSON);
    },
    transformTreeToCode() {
      this.codeValue = clone(this.treeJSON);
    },
  },
};
</script>

<style>
.editor-container {
  position: relative;
}

.editor-wrap {
  position: relative;
  height: 100%;
}

.editor-transform {
  margin-bottom: 0.5em;
}

.editor {
  position: absolute;
  height: 100%;
  width: 100%;
  top: 0;
  left: 0;
}
</style>
