<template>
  <div class="content_1">
    <div class="content_2">
      <h1>Create New Item</h1>
      <p><span class="ant-form-item-required"></span> Required fields</p>
      <!----------------------------------------- Init Form -------------------------------------->
      <a-form-model
        :model="form"
        ref="ruleForm"
        :form="form"
        :rules="rules"
        @submit="handleSubmit"
      >
        <!----------------------------------------- Upload File -------------------------------------->
        <a-form-model-item prop="file" label="Image, Video, Audio, or 3D Model">
          <p>
            File types supported: JPG, PNG, GIF, SVG, MP4, WEBM, MP3, WAV, OGG,
            GLB, GLTF. Max size: 100 MB
          </p>
          <a-upload
            class="avatar-uploader"
            :show-upload-list="false"
            list-type="picture-card"
            @change="handleChangeUpload"
          >
            <img v-if="imageUrl" :src="imageUrl" alt="avatar" />
            <div v-else>
              <a-icon type="file-image" />
              <div class="ant-upload-text">Upload</div>
            </div>
          </a-upload>
        </a-form-model-item>
        <!----------------------------------------- Name -------------------------------------->
        <a-form-model-item ref="name" prop="name" label="Item Name">
          <a-input
            @blur="
              () => {
                $refs.name.onFieldBlur();
              }
            "
            v-model="form.name"
            placeholder="Item Name"
          />
        </a-form-model-item>
        <!----------------------------------------- External Link -------------------------------------->
        <a-form-model-item label="External Link">
          <p>You are welcome to link to your own webpage with more details.</p>
          <a-input
            v-model="form.ext_link"
            placeholder="https://yoursite.io/item/123"
          />
        </a-form-model-item>
        <!----------------------------------------- Description -------------------------------------->
        <a-form-model-item label="Description">
          <p>
            The description will be included on the item's detail page
            underneath its image.
          </p>
          <a-textarea
            class="description"
            v-model="form.description"
            placeholder="Provide a detailed description of your item..."
            :rows="4"
          />
        </a-form-model-item>
        <!----------------------------------------- Properties -------------------------------------->
        <a-form-model-item>
          <a-icon class="position-absolute icon" type="unordered-list" />
          <p class="position-absolute right-p">Properties</p>
          <p class="position-absolute right-p-1">
            Textual traits that show up as rectangles
          </p>
          <a-button
            class="position-absolute right-b button-plus"
            type="primary"
            icon="plus"
            @click="showModal('properties')"
          />
          <a-modal
            v-model="visibleP"
            title="Add Properties"
            @cancel="handleCancel('properties')"
          >
            <template slot="footer">
              <a-button
                class="btn-modal"
                type="primary"
                @click="handleSave('properties')"
              >
                Save
              </a-button>
            </template>
            <p class="p-modal">
              Properties show up underneath your item, are clickable, and can be
              filtered in your collection's sidebar.
            </p>
            <br />
            <h2 class="h2-name">Name</h2>
            <h2 class="h2-value">Value</h2>
            <br /><br />
            <a-form-model-item
              v-for="(item, index) in form.properties"
              :key="index"
            >
              <div>
                <a-input
                  class="add-left"
                  v-model="form.properties[index]"
                  placeholder="Character"
                  style="width: 60%; margin-right: 8px"
                />
                <a-input
                  class="add-rigth"
                  v-model="form.value[index]"
                  placeholder="Male"
                  style="width: 60%; margin-right: 8px"
                />
                <a-icon
                  id="delete-input"
                  v-if="form.properties.length > 1"
                  class="dynamic-delete-button"
                  type="minus-square"
                  :disabled="form.properties.length === 1"
                  @click="() => remove(index, 'properties')"
                />
              </div>
              <br />
            </a-form-model-item>
            <br /><br />
            <a-form-model-item>
              <a-button
                class="btn-add"
                type="dashed"
                style="width: 60%"
                @click="add('properties')"
              >
                <a-icon type="plus" /> Add More
              </a-button>
            </a-form-model-item>
          </a-modal>
        </a-form-model-item>
        <a-divider />
        <br /><br />
        <!----------------------------------------- Levels -------------------------------------->
        <a-form-model-item>
          <a-icon theme="filled" class="position-absolute icon" type="star" />
          <p class="position-absolute right-p">Levels</p>
          <p class="position-absolute right-p-1">
            Numerical traits that show as a progress bar
          </p>
          <a-button
            class="position-absolute right-b button-plus"
            type="primary"
            icon="plus"
            @click="showModal('levels')"
          />
          <a-modal
            v-model="visibleL"
            title="Add Levels"
            @cancel="handleCancel('levels')"
          >
            <template slot="footer">
              <a-button
                class="btn-modal"
                type="primary"
                @click="handleSave('levels')"
              >
                Save
              </a-button>
            </template>
            <p class="p-modal">
              Properties show up underneath your item, are clickable, and can be
              filtered in your collection's sidebar.
            </p>
            <br />
            <h2 class="h2-name">Name</h2>
            <h2 class="h2-value">Value</h2>
            <br /><br />
            <a-form-model-item
              v-for="(item, index) in form.levels"
              :key="index"
            >
              <div>
                <a-input
                  class="add-left"
                  v-model="form.levels[index]"
                  placeholder="Speed"
                />
                <a-input-number
                  class="div-input-number-1"
                  id="input-number-1"
                  v-model="form.value_l_1[index]"
                  :min="1"
                  :max="10"
                />
                <div class="middle">Of</div>
                <a-input-number
                  class="div-input-number-2"
                  id="input-number-2"
                  v-model="form.value_l_2[index]"
                  :min="1"
                  :max="10"
                />
                <a-icon
                  id="delete-input"
                  v-if="form.levels.length > 1"
                  class="dynamic-delete-button"
                  type="minus-square"
                  :disabled="form.levels.length === 1"
                  @click="() => remove(index, 'levels')"
                />
              </div>
              <br />
            </a-form-model-item>
            <br /><br />
            <a-form-model-item>
              <a-button
                class="btn-add"
                type="dashed"
                style="width: 60%"
                @click="add('levels')"
              >
                <a-icon type="plus" /> Add More
              </a-button>
            </a-form-model-item>
          </a-modal>
        </a-form-model-item>
        <a-divider />
        <br /><br />
        <!----------------------------------------- Stats -------------------------------------->
        <a-form-model-item>
          <a-icon class="position-absolute icon" type="bar-chart" />
          <p class="position-absolute right-p">Stats</p>
          <p class="position-absolute right-p-1">
            Numerical traits that just show as numbers
          </p>
          <a-button
            class="position-absolute right-b button-plus"
            type="primary"
            icon="plus"
            @click="showModal('stats')"
          />
          <a-modal
            v-model="visibleS"
            title="Add Stats"
            @cancel="handleCancel('stats')"
          >
            <template slot="footer">
              <a-button
                class="btn-modal"
                type="primary"
                @click="handleSave('stats')"
              >
                Save
              </a-button>
            </template>
            <p class="p-modal">
              Stats show up underneath your item, are clickable, and can be
              filtered in your collection's sidebar.
            </p>
            <br />
            <h2 class="h2-name">Name</h2>
            <h2 class="h2-value">Value</h2>
            <br /><br />
            <a-form-model-item v-for="(item, index) in form.stats" :key="index">
              <div>
                <a-input
                  class="add-left"
                  v-model="form.stats[index]"
                  placeholder="Speed"
                />
                <a-input-number
                  class="div-input-number-1"
                  id="input-number-1"
                  v-model="form.value_s_1[index]"
                  :min="1"
                  :max="10"
                />
                <div class="middle">Of</div>
                <a-input-number
                  class="div-input-number-2"
                  id="input-number-2"
                  v-model="form.value_s_2[index]"
                  :min="1"
                  :max="10"
                />
                <a-icon
                  id="delete-input"
                  v-if="form.stats.length > 1"
                  class="dynamic-delete-button"
                  type="minus-square"
                  :disabled="form.stats.length === 1"
                  @click="() => remove(index, 'stats')"
                />
              </div>
              <br />
            </a-form-model-item>
            <br /><br />
            <a-form-model-item>
              <a-button
                class="btn-add"
                type="dashed"
                style="width: 60%"
                @click="add('stats')"
              >
                <a-icon type="plus" /> Add More
              </a-button>
            </a-form-model-item>
          </a-modal>
        </a-form-model-item>
        <a-divider />
        <br />
        <!----------------------------------------- Supply -------------------------------------->
        <a-form-model-item label="Supply">
          <p>The number of items that can be minted. No gas cost to you!</p>
          <a-input disabled v-model="form.supply" placeholder="Supply" />
        </a-form-model-item>
        <!----------------------------------------- Blokchain -------------------------------------->
        <a-form-model-item label="Blokchain">
          <a-select v-model="form.blokchain">
            <a-select-option value="BSC" disabled>
              Binance Smart Chain
            </a-select-option>
            <a-select-option value="Solana" disabled> Solana </a-select-option>
            <a-select-option value="Rinkeby"> Rinkeby </a-select-option>
          </a-select>
        </a-form-model-item>
        <a-divider class="end" />
        <!----------------------------------------- Submit Form -------------------------------------->
        <a-form-model-item>
          <a-button
            :disabled="form.name != '' && form.file != '' ? false : true"
            type="primary"
            html-type="submit"
          >
            <a-spin v-if="loading" />
            <span v-else>Submit</span>
          </a-button>
        </a-form-model-item>
      </a-form-model>
    </div>
  </div>
</template>

<script>
import Web3 from "web3";
import Moralis from "moralis";
/** Connect to Moralis server */
const serverUrl = "https://wauq8qcezrrg.usemoralis.com:2053/server";
const appId = "NgsjXA07FBe6wcu3R0ycjeUDiP1RGjfmb5FuZHkP";
Moralis.start({ serverUrl, appId });
let user = Moralis.User.current();
const nft_contract_address = "0x0Fb6EF3505b9c52Ed39595433a21aF9B5FCc4431";
const web3 = new Web3(window.ethereum);

/** Add from here down */
async function login() {
  if (!user) {
    try {
      user = await Moralis.authenticate({ signingMessage: "Hello World!" });
    } catch (error) {
      console.log(error);
    }
  } else {
    await Moralis.enableWeb3();
  }
}

login();

function getBase64(img, callback) {
  const reader = new FileReader();
  reader.addEventListener("load", () => callback(reader.result));
  reader.readAsDataURL(img);
}

export default {
  name: "HelloWorld",
  data() {
    return {
      loading: false,
      imageUrl: "",
      visibleP: false,
      visibleL: false,
      visibleS: false,
      form: {
        file: "",
        name: "",
        description: "",
        ext_link: "",
        unlock_content: "",
        sensitive_content: "",
        supply: 1,
        blokchain: "Rinkeby",
        properties: [""],
        properties_copy: [],
        value: [""],
        value_copy: [],
        levels: [""],
        levels_copy: [],
        value_l_1: [""],
        value_l_1_copy: [],
        value_l_2: [""],
        value_l_2_copy: [],
        stats: [""],
        stats_copy: [],
        value_s_1: [""],
        value_s_1_copy: [],
        value_s_2: [""],
        value_s_2_copy: [],
      },
      rules: {
        name: [
          {
            required: true,
            message: "Please input Item name",
            trigger: "blur",
          },
        ],
        file: [
          {
            required: true,
            message: "Please input some File",
            trigger: "blur",
          },
        ],
      },
    };
  },
  methods: {
    async submit(metadata) {
      const imageFile = new Moralis.File(metadata.image.name, metadata.image);
      await imageFile.saveIPFS();
      const imageUri = imageFile.ipfs();
      metadata.image = imageUri;

      const jsonFile = new Moralis.File("metadata.json", {
        base64: btoa(JSON.stringify(metadata)),
      });
      await jsonFile.saveIPFS();

      const metadataURI = jsonFile.ipfs();
      console.log(metadataURI);
      const encodedFunction = web3.eth.abi.encodeFunctionCall(
        {
          name: "mintToken",
          type: "function",
          inputs: [
            {
              type: "string",
              name: "tokenURI",
            },
          ],
        },
        [metadataURI]
      );

      const transactionParameters = {
        to: nft_contract_address,
        from: window.ethereum.selectedAddress,
        data: encodedFunction,
      };
      try {
        const txid = await window.ethereum.request({
          method: "eth_sendTransaction",
          params: [transactionParameters],
        });
        this.$notification.success({
          message: "Congratulations!!",
          description: function (h) {
            return h("div", [
              "You have created an NFT, for more details of the transaction see: ",
              h("br"),
              h(
                "a",
                {
                  attrs: {
                    href: `https://rinkeby.etherscan.io/tx/${txid}`,
                  },
                },
                [txid]
              ),
            ]);
          },
          duration: 5,
        });
        this.loading = false;
      } catch (e) {
        console.log(`erroooooooooooooooooooorrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrr ${e}`);
        this.$notification.error({
          message: "Unexpected error!!",
          description: `An error has occurred with the transaction, more details: ${e.message}`,
          duration: 5,
        });
        this.loading = false;
      }
    },
    handleChangeUpload(info) {
      getBase64(info.file.originFileObj, (imageUrl) => {
        this.imageUrl = imageUrl;
      });
      this.form.file = info.file.originFileObj;
    },

    remove(index, type) {
      if (type == "properties") {
        if (this.form.properties.length === 1) {
          return;
        }
        this.form.properties.splice(index, 1);
        this.form.value.splice(index, 1);
      } else if (type == "levels") {
        if (this.form.levels.length === 1) {
          return;
        }
        this.form.levels.splice(index, 1);
        this.form.value_l_1.splice(index, 1);
        this.form.value_l_2.splice(index, 1);
      } else {
        if (this.form.stats.length === 1) {
          return;
        }
        this.form.stats.splice(index, 1);
        this.form.value_s_1.splice(index, 1);
        this.form.value_s_2.splice(index, 1);
      }
    },

    add(type) {
      if (type == "properties") {
        this.form.properties.push("");
        this.form.value.push("");
      } else if (type == "levels") {
        this.form.levels.push("");
        this.form.value_l_1.push("");
        this.form.value_l_2.push("");
      } else {
        this.form.stats.push("");
        this.form.value_s_1.push("");
        this.form.value_s_2.push("");
      }
    },

    showModal(modal) {
      if (modal == "properties") {
        if (this.form.properties_copy.length != 0) {
          this.form.properties = [];
        } else this.form.properties = [""];
        if (this.form.value_copy.length != 0) {
          this.form.value = [];
        } else this.form.value = [""];

        this.form.properties_copy.forEach((element) => {
          this.form.properties.push(element);
        });
        this.form.value_copy.forEach((element) => {
          this.form.value.push(element);
        });
        this.visibleP = true;
      } else if (modal == "levels") {
        if (this.form.levels_copy.length != 0) {
          this.form.levels = [];
        } else this.form.levels = [""];
        if (this.form.value_l_1_copy.length != 0) {
          this.form.value_l_1 = [];
        } else this.form.value_l_1 = [""];
        if (this.form.value_l_2_copy.length != 0) {
          this.form.value_l_2 = [];
        } else this.form.value_l_2 = [""];
        this.form.levels_copy.forEach((element) => {
          this.form.levels.push(element);
        });
        this.form.value_l_1_copy.forEach((element) => {
          this.form.value_l_1.push(element);
        });
        this.form.value_l_2_copy.forEach((element) => {
          this.form.value_l_2.push(element);
        });
        this.visibleL = true;
      } else {
        if (this.form.stats_copy.length != 0) {
          this.form.stats = [];
        } else this.form.stats = [""];
        if (this.form.value_s_1_copy.length != 0) {
          this.form.value_s_1 = [];
        } else this.form.value_s_1 = [""];
        if (this.form.value_s_2_copy.length != 0) {
          this.form.value_s_2 = [];
        } else this.form.value_s_2 = [""];

        this.form.stats_copy.forEach((element) => {
          this.form.stats.push(element);
        });
        this.form.value_s_1_copy.forEach((element) => {
          this.form.value_s_1.push(element);
        });
        this.form.value_s_2_copy.forEach((element) => {
          this.form.value_s_2.push(element);
        });
        this.visibleS = true;
      }
    },
    handleCancel(type) {
      if (type == "properties") {
        this.form.properties = [""];
        this.form.value = [""];
      } else if (type == "levels") {
        this.form.levels = [""];
        this.form.value_l_1 = [""];
        this.form.value_l_2 = [""];
      } else {
        this.form.stats = [""];
        this.form.value_s_1 = [""];
        this.form.value_s_2 = [""];
      }
    },
    handleSave(type) {
      if (type == "properties") {
        this.visibleP = false;
        this.form.properties_copy = [];
        this.form.properties.forEach((element) => {
          this.form.properties_copy.push(element);
        });
        this.form.value_copy = [];
        this.form.value.forEach((element) => {
          this.form.value_copy.push(element);
        });
      } else if (type == "levels") {
        this.visibleL = false;
        this.form.levels_copy = [];
        this.form.levels.forEach((element) => {
          this.form.levels_copy.push(element);
        });
        this.form.value_l_1_copy = [];
        this.form.value_l_1.forEach((element) => {
          this.form.value_l_1_copy.push(element);
        });
        this.form.value_l_2_copy = [];
        this.form.value_l_2.forEach((element) => {
          this.form.value_l_2_copy.push(element);
        });
      } else {
        this.visibleS = false;
        this.form.stats_copy = [];
        this.form.stats.forEach((element) => {
          this.form.stats_copy.push(element);
        });
        this.form.value_s_1_copy = [];
        this.form.value_s_1.forEach((element) => {
          this.form.value_s_1_copy.push(element);
        });
        this.form.value_s_2_copy = [];
        this.form.value_s_2.forEach((element) => {
          this.form.value_s_2_copy.push(element);
        });
      }
    },
    handleSubmit(e) {
      e.preventDefault();
      this.$refs.ruleForm.validate((valid) => {
        if (valid) {
          this.loading = true;
          const metadata_attributtes = [];
          let i;
          for (i = 0; i < this.form.properties_copy.length; i++) {
            metadata_attributtes.push({
              trait_type: this.form.properties_copy[i],
              value: this.form.value_copy[i],
            });
          }
          for (i = 0; i < this.form.levels_copy.length; i++) {
            metadata_attributtes.push({
              trait_type: this.form.levels_copy[i],
              value: this.form.value_l_2_copy[i],
              max_value: this.form.value_l_1_copy[i],
            });
          }
          for (i = 0; i < this.form.stats_copy.length; i++) {
            metadata_attributtes.push({
              display_type: "number",
              trait_type: this.form.stats_copy[i],
              value: this.form.value_s_2_copy[i],
              max_value: this.form.value_s_1_copy[i],
            });
          }
          let metadata = {
            name: this.form.name,
            description: this.form.description,
            image: this.form.file,
            external_url: this.form.ext_link,
            blokchain: this.form.blokchain,
            attributes: metadata_attributtes,
          };
          this.submit(metadata);
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.ant-form-item-label > label {
  color: rgb(255, 255, 255) !important;
}
.content_2 {
  width: 700px;
  margin: 0 auto;
}
.content_1 {
  width: 100%;
  height: 100%;
  position: absolute;
}
.ant-input {
  height: 52px !important;
  border-radius: 10px !important;
  color: rgb(255, 255, 255) !important;
  background-color: rgb(53, 56, 64) !important;
  border: 2px solid rgb(21, 27, 34) !important;
  font-size: 17px !important;
}
.ant-form label {
  font-weight: 700 !important;
  font-size: 16px !important;
}
.description {
  height: auto !important;
}
.ant-select-selection {
  height: 52px !important;
  border-radius: 10px !important;
  color: rgb(255, 255, 255) !important;
  background-color: rgb(53, 56, 64) !important;
  font-size: 17px !important;
  border: 2px solid rgb(21, 27, 34) !important;
}
.ant-select-selection:hover {
  background-color: rgba(240, 248, 255, 0.23) !important;
}
.ant-col-sm-offset-4 {
  margin-left: 0% !important;
}
.ant-btn-primary-disabled,
.ant-btn-primary.disabled,
.ant-btn-primary[disabled],
.ant-btn-primary-disabled:hover,
.ant-btn-primary.disabled:hover,
.ant-btn-primary[disabled]:hover,
.ant-btn-primary-disabled:focus,
.ant-btn-primary.disabled:focus,
.ant-btn-primary[disabled]:focus,
.ant-btn-primary-disabled:active,
.ant-btn-primary.disabled:active,
.ant-btn-primary[disabled]:active,
.ant-btn-primary-disabled.active,
.ant-btn-primary.disabled.active,
.ant-btn-primary[disabled].active {
  color: rgba(255, 255, 255) !important;
  opacity: 0.2 !important;
}
.ant-btn-primary {
  margin-bottom: 30px !important;
  color: rgba(255, 255, 255) !important;
  background-color: rgb(0 54 107) !important;
  border-color: rgb(0 54 107) !important;
  font-weight: 700 !important;
  font-size: 17px !important;
  border-radius: 10px !important;
  float: right !important;
  height: 52px !important;
  width: 100px !important;
}
.ant-select-dropdown-menu-item {
  color: rgba(255, 255, 255) !important;
  background-color: rgb(53, 56, 64) !important;
  height: 52px !important;
  font-size: 17px !important;
  border-radius: 10px !important;
}
.ant-select-focused .ant-select-selection,
.ant-select-selection:focus,
.ant-select-selection:active {
  border-color: rgb(240, 248, 255) !important;
  box-shadow: 0 0 0 2px #e8e8e861 !important;
}
.ant-select-dropdown-menu-item:hover {
  background-color: rgba(240, 248, 255, 0.23) !important;
}
.ant-select-dropdown-menu-item-disabled {
  opacity: 0.1 !important;
}
.ant-select-dropdown {
  background-color: rgb(53, 56, 64) !important;
}
.ant-input:focus {
  border-color: rgb(240, 248, 255) !important;
  box-shadow: 0 0 0 2px #e8e8e861 !important;
}
.ant-input:hover {
  background-color: rgba(240, 248, 255, 0.23) !important;
}
.ant-select-arrow {
  color: rgba(255, 255, 255) !important;
}
.avatar-uploader > .ant-upload {
  width: 300px !important;
  height: 300px !important;
}
.ant-upload.ant-upload-select-picture-card {
  background-color: #fafafa00 !important;
  border-radius: 10px !important;
  border: 3px dashed #d9d9d9 !important;
}
.ant-upload.ant-upload-select-picture-card > .ant-upload {
  color: rgba(240, 248, 255, 0.23) !important;
  font-size: 40px !important;
}
.ant-upload .anticon {
  font-size: 100px !important;
}
img {
  border-radius: 10px !important;
  width: 270px !important;
  height: 270px !important;
}
.ant-upload:hover {
  background-color: rgba(240, 248, 255, 0.02) !important;
}
h1 {
  margin-top: 0 !important;
  color: rgb(255 255 255) !important;
  font-weight: 650 !important;
  font-size: 50px !important;
}
p {
  color: rgb(80 84 87) !important;
  font-size: 13px !important;
  font-weight: 700 !important;
  margin-bottom: 0 !important;
  margin-top: -20px !important;
}
.position-absolute {
  position: absolute !important;
}
.right-b {
  left: 650px !important;
}
.right-p {
  left: 40px !important;
  color: rgb(255, 255, 255) !important;
  font-weight: 700 !important;
  font-size: 16px !important;
}
.right-p-1 {
  left: 40px !important;
  bottom: -45px !important;
  width: 600px !important;
  color: rgba(255, 255, 255, 0.63) !important;
  font-weight: 400 !important;
  font-size: 14px !important;
}
.icon {
  bottom: -27px !important;
  font-size: 25px !important;
  color: #d9d9d9 !important;
}
.ant-divider,
.ant-divider-vertical {
  top: 2em !important;
}
.ant-divider {
  background: #353840 !important;
}
.button-plus {
  background-color: #353840 !important;
  border-color: #353840 !important;
  width: 50px !important;
  top: -10px !important;
}
.button-plus:hover {
  background-color: rgba(240, 248, 255, 0.23) !important;
}
.ant-btn .anticon {
  font-size: 20px !important;
}
.ant-modal-header {
  color: rgb(255, 255, 255) !important;
  background: rgb(48 51 57) !important;
  border-radius: 10px 10px 0 0 !important;
  border-bottom: 1px solid #201f1f !important;
}
.ant-modal-content {
  color: rgb(255, 255, 255) !important;
  background: rgb(48 51 57) !important;
  border-radius: 10px !important;
}
.ant-modal-title {
  margin-left: 160px !important;
  color: rgb(255, 255, 255) !important;
  font-weight: 700 !important;
  font-size: 20px !important;
}
.ant-modal-footer {
  border-top: 1px solid #201f1f !important;
  border-radius: 0 0 10px 10px !important;
}
.ant-modal-close-x {
  color: rgb(132 146 155) !important;
  font-size: 20px !important;
}
.p-modal {
  font-size: 14px !important;
  font-weight: 600 !important;
  color: rgb(127 140 149) !important;
}
.btn-add {
  color: rgba(255, 255, 255) !important;
  background-color: rgb(53 56 64) !important;
  border-color: #201f1f !important;
  font-weight: 700 !important;
  font-size: 17px !important;
  border-radius: 10px !important;
  height: 52px !important;
  width: 145px !important;
}
.btn-add:hover {
  background-color: rgba(240, 248, 255, 0.23) !important;
}
.add-left {
  border: 1px solid rgb(21, 27, 34) !important;
  position: absolute !important;
  width: 200px !important;
}

.add-rigth {
  border: 1px solid rgb(21, 27, 34) !important;
  position: absolute !important;
  width: 200px !important;
  left: 230px !important;
}
#delete-input {
  position: absolute !important;
  font-size: 50px !important;
  bottom: -23px !important;
  color: rgb(127 140 149) !important;
  left: 440px !important;
}
h2 {
  font-size: 18px !important;
  position: absolute !important;
  color: rgb(255 255 255) !important;
}
.h2-name {
  left: 95px !important;
}
.h2-value {
  left: 325px !important;
}
#input-number-1 {
  color: rgb(255, 255, 255) !important;
  border-color: #353840 !important;
  border-radius: 0px 10px 10px 0px !important;
  height: 50px !important;
  background-color: #ff000000 !important;
  left: inherit !important;
}
.div-input-number-1 {
  position: absolute !important;
  left: 352px !important;
  width: 80px !important;
  background-color: #353840 !important;
  border-color: rgb(21, 27, 34) !important;
  border-width: 1px 1px 1px 0px !important;
  border-style: solid !important;
  height: 52px !important;
  border-radius: 0px 10px 10px 0px !important;
  color: rgb(255, 255, 255) !important;
  font-size: 17px !important;
}
#input-number-1:focus {
  border-color: rgb(240, 248, 255) !important;
  box-shadow: 0 0 0 2px #e8e8e861 !important;
}
.div-input-number-1:hover {
  background-color: rgba(240, 248, 255, 0.23) !important;
}
#input-number-2 {
  color: rgb(255, 255, 255) !important;
  border-color: #353840 !important;
  border-radius: 10px 0px 0px 10px !important;
  height: 50px !important;
  background-color: #201f1f00 !important;
  left: inherit !important;
}
.div-input-number-2 {
  background-color: rgba(240, 248, 255, 0.23) !important;
  position: absolute !important;
  left: 219px !important;
  width: 80px !important;
  background-color: #353840 !important;
  border-color: rgb(21, 27, 34) !important;
  border-width: 1px 0px 1px 1px !important;
  border-style: solid !important;
  height: 52px !important;
  border-radius: 10px 0px 0px 10px !important;
  color: rgb(255, 255, 255) !important;
  font-size: 17px !important;
}
#input-number-2:focus {
  border-color: rgb(240, 248, 255) !important;
  box-shadow: 0 0 0 2px #e8e8e861 !important;
}
.div-input-number-2:hover {
  background-color: rgba(240, 248, 255, 0.23) !important;
}
.div-input-number-2:focus {
  border-color: rgb(240, 248, 255) !important;
  box-shadow: 0 0 0 2px #e8e8e861 !important;
}
.middle {
  background-color: rgb(123 135 144 / 56%) !important;
  position: absolute !important;
  left: 300px !important;
  width: 50px !important;
  height: 52px !important;
  border: 1px solid rgb(21, 27, 34) !important;
  text-align: center !important;
  font-size: 17px !important;
  color: rgb(255, 255, 255) !important;
  line-height: 51px !important;
}
.end {
  background: #141414 !important;
  margin-top: -20px !important;
  top: 15px !important;
}
.ant-notification-notice {
  background: rgb(53 56 64) !important;
  border-radius: 10px !important;
  box-shadow: 0 0px 10px #ffffff !important;
  color: rgb(92 96 107) !important;
  width: 560px !important;
}
.ant-notification-notice-message {
  color: rgb(255, 255, 255) !important;
}
.ant-notification-close-icon {
  color: rgb(255, 255, 255) !important;
}
.ant-notification {
  margin-right: 190px !important;
}
.btn-modal {
  float: none !important;
  margin-bottom: 0px !important;
}
</style>
