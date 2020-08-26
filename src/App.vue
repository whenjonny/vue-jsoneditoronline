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
                <v-btn fab
                       small
                       bottom
                       right
                       absolute
                       color="primary"
                       class="editor-transform editor-ae"
                       @click="transformCodeToAEAPI">
                  AE API
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
        mode: 'code',
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
    transformCodeToAEAPI() {
      let publishRequestDTO;
      let productDTO;
      if (this.codeJSON instanceof Array) {
        publishRequestDTO = this.codeJSON[0];
        publishRequestDTO.class = 'com.alibaba.global.gpf.common.domain.request.PublishRequestDTO';
        productDTO = publishRequestDTO.productDTO;
        productDTO.class = 'com.alibaba.global.gpf.common.ae.domain.dto.AEProductDTO';
        const skuList = productDTO.skuList;
        if (skuList instanceof Array) {
          for (const skuIndex in skuList) {
            const sku = skuList[skuIndex];

            sku.class = 'com.alibaba.global.gpf.common.ae.domain.dto.AESkuDTO';
            const salePropertyPairList = sku.salePropertyPairList;
            if (salePropertyPairList !== undefined && salePropertyPairList.length > 0) {
              for (const salePropertyPairIndex in salePropertyPairList) {
                const salePropertyPair = salePropertyPairList[salePropertyPairIndex];

                salePropertyPair.class = 'com.alibaba.global.gpf.common.ae.domain.dto.AEPropPairDTO';
                const propValueList = salePropertyPair.propValueList;
                if (propValueList !== undefined && propValueList.length > 0) {
                  for (const propValueIndex in propValueList) {
                    let propValue = propValueList[propValueIndex];

                    propValue.class = 'com.alibaba.global.gpf.common.ae.domain.dto.AEPropValueDTO';
                  }
                }
              }
            }
            debugger
            const nationalPrices = sku.nationalPrices;
            debugger
            if (nationalPrices != undefined && nationalPrices.length > 0) {
              debugger
              for (const nationalPriceIndex in nationalPrices) {
                let nationalPrice = nationalPrices[nationalPriceIndex];
                nationalPrice.class = 'com.alibaba.global.gpf.common.ae.domain.dto.NationalPriceDTO';
              }
            }
            debugger

          }
        }
        const extDTO=productDTO.extDTO;
        if (extDTO != undefined ) {

          extDTO.class = 'com.alibaba.global.gpf.common.ae.domain.dto.AEExtDTO';
        }
        const mediaDTO = productDTO.mediaDTO;
        if (mediaDTO != undefined) {

          mediaDTO.class = 'com.alibaba.global.gpf.common.ae.domain.dto.AEMediaDTO';
        }
        const packageDTO = productDTO.packageDTO;
        if (packageDTO != undefined) {

          packageDTO.class = 'com.alibaba.global.gpf.common.ae.domain.dto.AEPackageDTO';
        }
        const productInfoDTO = productDTO.productInfoDTO;
        if (productInfoDTO != undefined) {

          productInfoDTO.class = 'com.alibaba.global.gpf.common.ae.domain.dto.AEProductInfoDTO';
        }
        const descriptionDTO = productDTO.descriptionDTO;
        if (descriptionDTO != undefined) {

          descriptionDTO.class = 'com.alibaba.global.gpf.common.ae.domain.dto.AEDescriptionDTO';
          let descriptionMap = descriptionDTO.descriptionMap;
          for (const i in descriptionMap) {
            let desVal = descriptionMap[i];

            desVal.class = 'com.alibaba.global.gpf.common.ae.domain.dto.AEDescription';
          }

        }
        const freightDTO = productDTO.freightDTO;
        if (freightDTO != undefined) {
          freightDTO.class = 'com.alibaba.global.gpf.common.ae.domain.dto.FreightDTO';
        }
        const userDTO = productDTO.userDTO;
        if (userDTO != undefined) {
          userDTO.class = 'com.alibaba.global.gpf.common.domain.request.BaseUserDTO';
        }
        let propertyPairList = productDTO.propertyPairList;
        if (propertyPairList != undefined && propertyPairList.length > 0) {
          for (const propertyPairIndex in propertyPairList) {
            let propertyPair = propertyPairList[propertyPairIndex];
            propertyPair.class = 'com.alibaba.global.gpf.common.ae.domain.dto.AEPropPairDTO';
            let propValueList = propertyPair.propValueList;
            if (propValueList != undefined && propValueList.length > 0) {
              for (const propValueIndex in propValueList) {
                const propValue = propValueList[propValueIndex];
                propValue.class = 'com.alibaba.global.gpf.common.ae.domain.dto.AEPropValueDTO';
              }
            }
          }
        }

        debugger;
      } else {
        productDTO = this.codeJSON.productDTO;
      }
      this.transformCodeToTree();
      return productDTO;
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

.editor-ae {
  margin-right: 5em;
}

.editor {
  position: absolute;
  height: 100%;
  width: 100%;
  top: 0;
  left: 0;
}
</style>
