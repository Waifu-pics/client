<template>
  <v-container class="compact" style="margin-top: 50px;">
    <v-row class="mb-6 compact" justify="flex-start" no-gutters>
      <div>
        <div>
          <h1 class="font-weight-light">API Docs</h1>
          <p>The open and supported part of the waifu.pics api is incredibly easy to use. You can find more information about how to utilize this in your application below.</p>
        </div>
        <v-alert type="info" outlined>
          If you have any issues or questions with the API, please create a Github issue at <a href="https://github.com/Waifu-pics/waifu-api">https://github.com/Waifu-pics/waifu-api</a>
        </v-alert>
        <v-alert type="warning" outlined>
          Currently we have switched the API url to https://api.waifu.pics from https://waifu.pics/api. While that route does still work, you should switch any existing application to use the newer URL.
        </v-alert>
        <v-divider></v-divider>
      </div>
      
      <div>
        <h2 class="font-weight-light tab">Image Categories</h2>
        <v-tabs v-model="tab" dark class="compact">
          <v-tabs-slider></v-tabs-slider>
          <v-tab>sfw</v-tab>
          <v-tab>nsfw</v-tab>
          <v-tab-item>
            <v-list class="compact">
              <v-list-item v-for="end in this.$store.getters.endpoints.sfw" :key="end">
                <v-list-item-content>
                  <v-list-item-title>{{end}}</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list>
          </v-tab-item>
          <v-tab-item>
            <v-list class="compact">
              <v-list-item v-for="end in this.$store.getters.endpoints.nsfw" :key="end">
                <v-list-item-content>
                  <v-list-item-title>{{end}}</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list>
          </v-tab-item>
        </v-tabs>
        <v-divider style="margin-top:20px;"></v-divider>
      </div>

      <div class="compact">
        <div class="tab">
          <h2 class="font-weight-light">Get image</h2>
          <p>Recieve one image url from your endpoint of choice.</p>
        </div>
        <Apitable type="GET">
          {{api}}<span class="vararg">type</span>/<span class="vararg">category</span>
        </Apitable>
        <div class="tab">
          <Comment>The type field is the type classification. Valid types are <b>sfw</b> and <b>nsfw</b>.</Comment>
        </div>
        <div class="tab">
          <h3 class="font-weight-light">Response</h3>
          <p>Here is an example response for this endpoint</p>
          <vue-json-pretty :data="examples.one" />
        </div>
        <v-divider></v-divider>
      </div>

      <div class="compact">
        <div class="tab">
          <h2 class="font-weight-light">Get many images</h2>
          <p>Recieve 30 unique images from a specific endpoint or category</p>
        </div>
        <Apitable class="tab" type="POST">
          {{api}}many/<span class="vararg">type</span>/<span class="vararg">category</span>
        </Apitable>
        <div class="tab">
          <Comment>The type field is the type classification. Valid types are <b>sfw</b> and <b>nsfw</b>.</Comment>
        </div>
        <div class="tab">
          <h3 class="font-weight-light">Fields</h3>
          <p>Fields that can be sent to this endpoint</p>
          <Reqtable :field="'exclude'" :type="'String Array'">
            A list of URL's to not recieve from this endpoint.
          </Reqtable>
        </div>
        <div class="tab">
          <h3 class="font-weight-light">Response</h3>
          <p>Here is an example response for this endpoint</p>
          <vue-json-pretty :data="examples.many" />
        </div>
        <v-divider></v-divider>
      </div>
    </v-row>
  </v-container>
</template>

<script>
import Apitable from '@/components/Docs/Apitable.vue'
import Reqtable from '@/components/Docs/Reqtable.vue'
import Comment from '@/components/Docs/Minicomment.vue'
import VueJsonPretty from 'vue-json-pretty'
import 'vue-json-pretty/lib/styles.css'

export default {
  name: 'More',
  components: {
    Apitable,
    Comment,
    VueJsonPretty,
    Reqtable,
  },
  data () {
    return {
      api: process.env.VUE_APP_API,
      examples: {
        one: JSON.parse(`{"url": "${process.env.VUE_APP_CDN}Tj6Wzwo.png"}`),
        many: JSON.parse(`{"files": ["${process.env.VUE_APP_CDN}qUY7BBo.jpg"]}`),
      },
    }
  },
}
</script>

<style lang="scss" scoped>
.compact {
  min-width: 600px;
  width: 600px;

  // ( ͡° ͜ʖ ͡°) responsive
  @media only screen and (max-width: 600px) {
    min-width: 100%;
    width: 100%;
  }
}
.tab {
  margin: 10px;
}
.centered {
  text-align: center;
}
.v-list{
  height: 300px;
  width: 350px;
  overflow-y: auto;
  text-align: start;
}
.bottom {
  margin-bottom: 30px;
}
.vararg {
  color: #f76868;
}
</style>
