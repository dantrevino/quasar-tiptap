<template>
  <editor-menu-bar :editor="editor" v-slot="editorContext">
    <section class="row col-12 justify-between items-center bg-light tiptap-menubar">

      <!-- Toolbar: left -->
      <section class="row q-px-xs menubar is-hidden" :class="{ 'is-focused': editorContext.focused }">
        <!-- Table -->
        <template v-if="editorContext.isActive.table && editorContext.isActive.table()">
          <template v-for="(item, index) of tableToolbar">
            <q-separator vertical inset :key="index" v-if="item==='separator'" />
            <component :key="index"
                       :name="item"
                       :is="getName(item)"
                       :editor="editor"
                       v-bind="editorContext"
                       v-else-if="typeof item === 'string'" />
            <component :key="index"
                       :name="item.name"
                       :is="getName(item.name)"
                       :opt="item.options"
                       :editor="editor"
                       v-bind="editorContext"
                       v-else-if="typeof item === 'object' && item.type === 'menu'" />
            <component :key="index"
                       :is="item"
                       :editor="editor"
                       v-bind="editorContext"
                       v-else />
            <o-table-group v-bind="editorContext" :key="`table-${index}`" v-if="item==='table'" />
          </template>
        </template>

        <!-- Normal -->
        <template v-else>
      <q-btn-group size="sm" outline>
        <q-btn
          class="editor-btn"
          size="sm"
          flat
          color="primary"
          @click="commands.bold"
          ><q-icon color="primary">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              viewBox="0 0 24 24"
            >
              <path
                d="M17.061,11.22C17.641,10.462,18,9.526,18,8.5C18,6.019,15.981,4,13.5,4H6v15h8c2.481,0,4.5-2.019,4.5-4.5	C18.5,13.203,17.94,12.042,17.061,11.22z M13.5,7C14.327,7,15,7.673,15,8.5S14.327,10,13.5,10H9V7H13.5z M14,16H9v-3h5	c0.827,0,1.5,0.673,1.5,1.5S14.827,16,14,16z"
              />
            </svg> </q-icon
        ></q-btn>
        <q-btn
          class="editor-btn"
          size="sm"
          flat
          color="primary"
          @click="commands.italic"
          ><q-icon color="primary">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              viewBox="0 0 24 24"
            >
              <path
                d="M19 7L19 4 9 4 9 7 11.868 7 9.012 17 5 17 5 20 15 20 15 17 12.132 17 14.988 7z"
              />
            </svg> </q-icon
        ></q-btn>
        <q-btn
          class="editor-btn"
          size="sm"
          flat
          color="primary"
          @click="commands.strike"
          ><q-icon color="primary">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              viewBox="0 0 24 24"
            >
              <path
                d="M20 11h-8c-4 0-4-1.816-4-2.5C8 7.882 8 6 12 6c2.8 0 2.99 1.678 3 2.014L16 8h1c0-1.384-1.045-4-5-4C6.584 4 6 7.147 6 8.5c0 .728.148 1.667.736 2.5H4v2h8 5.227H20V11zM12 18c-3.793 0-3.99-1.815-4-2H6c0 .04.069 4 6 4 5.221 0 6-2.819 6-4.5 0-.146-.009-.317-.028-.5h-2.006C15.998 15.2 16 15.376 16 15.5 16 16.184 16 18 12 18z"
              />
            </svg> </q-icon
        ></q-btn>
        <q-btn
          class="editor-btn"
          size="sm"
          flat
          color="primary"
          @click="commands.underline"
          ><q-icon color="primary">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              viewBox="0 0 24 24"
            >
              <path
                d="M5 18H19V20H5zM6 4v6c0 3.309 2.691 6 6 6s6-2.691 6-6V4h-2v6c0 2.206-1.794 4-4 4s-4-1.794-4-4V4H6z"
              />
            </svg> </q-icon
        ></q-btn>
        <q-btn
          class="editor-btn"
          size="sm"
          flat
          color="primary"
          @click="commands.code"
          ><q-icon color="primary">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              viewBox="0 0 24 24"
            >
              <path
                d="M8.293 6.293L2.586 12 8.293 17.707 9.707 16.293 5.414 12 9.707 7.707zM15.707 17.707L21.414 12 15.707 6.293 14.293 7.707 18.586 12 14.293 16.293z"
              />
            </svg> </q-icon
        ></q-btn>
        <q-btn class="editor-btn" size="sm" flat @click="commands.paragraph">
          <q-icon color="primary">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              viewBox="0 0 24 24"
            >
              <path
                d="M9,16h2v4h2V6h2v14h2V6h3V4h-7h-2H9c-3.309,0-6,2.691-6,6S5.691,16,9,16z M9,6h2v8H9c-2.206,0-4-1.794-4-4S6.794,6,9,6z"
              />
            </svg>
          </q-icon>
        </q-btn>
        <q-btn-dropdown size="sm" label="H1" color="primary" flat>
          <q-list dense>
            <q-item v-close-popup>
              <q-item-section>
                <q-btn
                  size="sm"
                  flat
                  color="primary"
                  @click="commands.heading({ level: 1 })"
                  >H1
                </q-btn>
              </q-item-section>
            </q-item>
            <q-item v-close-popup>
              <q-item-section>
                <q-btn
                  size="sm"
                  flat
                  color="primary"
                  @click="commands.heading({ level: 2 })"
                  >H2</q-btn
                >
              </q-item-section>
            </q-item>
            <q-item v-close-popup @click="commands.heading({ level: 3 })">
              <q-item-section>
                <q-btn
                  size="sm"
                  flat
                  color="primary"
                  @click="commands.heading({ level: 3 })"
                  >H3</q-btn
                >
              </q-item-section>
            </q-item>
            <q-item
              clickable
              v-close-popup
              @click="commands.heading({ level: 4 })"
            >
              <q-item-section>
                <q-btn
                  size="sm"
                  flat
                  color="primary"
                  @click="commands.heading({ level: 4 })"
                  >H4</q-btn
                >
              </q-item-section>
            </q-item>
            <q-item
              clickable
              v-close-popup
              @click="commands.heading({ level: 5 })"
            >
              <q-item-section>
                <q-btn
                  size="sm"
                  flat
                  color="primary"
                  @click="commands.heading({ level: 5 })"
                  >H5</q-btn
                >
              </q-item-section>
            </q-item>
            <q-item
              clickable
              v-close-popup
              @click="commands.heading({ level: 6 })"
            >
              <q-item-section>
                <q-btn
                  size="sm"
                  flat
                  color="primary"
                  @click="commands.heading({ level: 6 })"
                  >H6</q-btn
                >
              </q-item-section>
            </q-item>
          </q-list>
        </q-btn-dropdown>

        <q-btn class="editor-btn" size="sm" flat color="primary"
          ><q-icon color="primary">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              viewBox="0 0 24 24"
            >
              <path
                d="M4 6H6V8H4zM4 11H6V13H4zM4 16H6V18H4zM20 8L20 6 18.8 6 9.2 6 8.023 6 8.023 8 9.2 8 18.8 8zM8 11H20V13H8zM8 16H20V18H8z"
              />
            </svg> </q-icon
        ></q-btn>
        <q-btn
          class="editor-btn"
          size="sm"
          flat
          color="primary"
          @click="commands.ordered_list"
          ><q-icon color="primary">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              viewBox="0 0 24 24"
            >
              <path
                d="M5.282 12.064c-.428.328-.72.609-.875.851-.155.24-.249.498-.279.768h2.679v-.748H5.413c.081-.081.152-.151.212-.201.062-.05.182-.142.361-.27.303-.218.511-.42.626-.604.116-.186.173-.375.173-.578 0-.189-.05-.363-.151-.512-.1-.153-.237-.268-.412-.341-.174-.076-.419-.111-.733-.111-.3 0-.537.038-.706.114-.172.078-.302.19-.396.338-.094.143-.159.346-.194.604l.894.076c.025-.188.074-.317.147-.394.072-.073.166-.108.279-.108.11 0 .2.035.272.108.073.071.108.156.108.258 0 .091-.037.19-.108.297C5.711 11.713 5.544 11.865 5.282 12.064zM5.337 18.45c-.114 0-.208-.036-.282-.105-.074-.07-.128-.195-.162-.378L4 18.085c.059.204.142.372.251.506.109.133.248.235.417.306C4.836 18.966 5.067 19 5.36 19c.3 0 .541-.047.725-.14.185-.096.325-.228.424-.403.098-.175.146-.354.146-.544 0-.152-.029-.282-.088-.393-.06-.107-.142-.195-.249-.261-.066-.042-.161-.078-.286-.11.155-.085.268-.183.345-.299.076-.115.113-.24.113-.383 0-.239-.093-.437-.281-.596-.187-.159-.49-.238-.909-.238-.365 0-.648.072-.847.219-.2.143-.334.353-.404.626l.844.151c.023-.162.067-.274.133-.338s.151-.098.257-.098c.103 0 .183.032.241.089.059.06.087.139.087.238 0 .104-.038.193-.117.27s-.177.112-.293.112c-.028 0-.066-.004-.116-.011L5.04 17.54c.123-.036.22-.056.289-.056.132 0 .237.041.313.126.077.082.115.199.115.352 0 .146-.04.266-.119.354C5.558 18.403 5.458 18.45 5.337 18.45zM6.285 8.367V5H5.546C5.451 5.208 5.319 5.383 5.152 5.523 4.984 5.665 4.748 5.785 4.444 5.888v.754C4.65 6.577 4.821 6.509 4.958 6.438c.137-.069.277-.162.423-.276v2.206H6.285zM9 6H20V8H9zM9 11H20V13H9zM9 16H20V18H9z"
              />
            </svg> </q-icon
        ></q-btn>
        <q-btn
          class="editor-btn"
          size="sm"
          flat
          color="primary"
          @click="commands.blockquote"
        >
          <q-icon color="primary">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              viewBox="0 0 24 24"
            >
              <path
                d="M20.309 17.708C22.196 15.66 22.006 13.03 22 13v-3V5c0-.552-.447-1-1-1h-6c-1.103 0-2 .897-2 2v7c0 .552.447 1 1 1h1 2.078c-.021.402-.123.912-.429 1.396-.508.801-1.465 1.348-2.846 1.624L13 17.18V20h1C16.783 20 18.906 19.229 20.309 17.708zM9.302 17.708c1.888-2.048 1.697-4.678 1.691-4.708v-3V5c0-.552-.447-1-1-1h-6c-1.103 0-2 .897-2 2v7c0 .552.447 1 1 1h1 2.078c-.021.402-.123.912-.429 1.396-.508.801-1.465 1.348-2.846 1.624L1.993 17.18V20h1C5.776 20 7.899 19.229 9.302 17.708z"
              />
            </svg>
          </q-icon>
        </q-btn>
        <q-btn
          class="editor-btn"
          size="sm"
          flat
          color="primary"
          @click="commands.code_block"
          ><q-icon color="primary">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              viewBox="0 0 24 24"
            >
              <path
                d="M20,3H4C2.897,3,2,3.897,2,5v14c0,1.103,0.897,2,2,2h16c1.103,0,2-0.897,2-2V5C22,3.897,21.103,3,20,3z M4,19V7h16 l0.002,12H4z"
              />
              <path
                d="M9.293 9.293L5.586 13 9.293 16.707 10.707 15.293 8.414 13 10.707 10.707zM14.707 9.293L13.293 10.707 15.586 13 13.293 15.293 14.707 16.707 18.414 13z"
              />
            </svg> </q-icon
        ></q-btn>
      </q-btn-group>        </template>

        <!-- Extra -->
        <slot name="left" />
      </section>

      <!-- Toolbar: right -->
      <section class="row q-px-xs">
        <slot name="right" />
      </section>
    </section>
  </editor-menu-bar>
</template>

<script>
import { EditorMenuBar } from 'tiptap'
import { CommandComponents, TableToolbar } from 'src/data/editor'

import OForeColorDropdown from 'src/components/buttons/OForeColorDropdown'
import OBackColorDropdown from 'src/components/buttons/OBackColorDropdown'
import OFontFamilyDropdown from 'src/components/buttons/OFontFamilyDropdown'
import OAlignDropdown from 'src/components/buttons/OAlignDropdown'
import OAlignGroup from 'src/components/buttons/OAlignGroup'
import OLineHeightDropdown from 'src/components/buttons/OLineHeightDropdown'
import OHeadingDropdown from 'src/components/buttons/OHeadingDropdown'
import OHeadingList from 'src/components/buttons/OHeadingList'
import OListDropdown from 'src/components/buttons/OListDropdown'
import OIndentDropdown from 'src/components/buttons/OIndentDropdown'
import OTextFormatDropdown from 'src/components/buttons/OTextFormatDropdown'

import OAddMoreBtn from 'src/components/buttons/OAddMoreBtn'
import OPhotoBtn from 'src/components/buttons/OPhotoBtn'
import OLinkBtn from 'src/components/buttons/OLinkBtn'
import OTableBtn from 'src/components/buttons/OTableBtn'
import OTableGroup from 'src/components/buttons/OTableGroup'

import OMenubarBtn from 'src/components/buttons/OMenubarBtn'
import OSimpleCommandBtn from 'src/components/buttons/OSimpleCommandBtn'
import OMetaInput from 'src/components/common/OMetaInput'

export default {
  name: 'o-editor-menu-bar',
  data () {
    return {
      tableToolbar: TableToolbar,
      pageView: 'page',
      isSlideShow: false
    }
  },
  props: {
    editor: {
      type: Object
    },
    toolbar: {
      type: Array,
      default: function () {
        return []
      }
    }
  },
  components: {
    EditorMenuBar,
    OMenubarBtn,
    OSimpleCommandBtn,
    OMetaInput,
    OForeColorDropdown,
    OBackColorDropdown,
    OFontFamilyDropdown,
    OAlignDropdown,
    OAlignGroup,
    OLineHeightDropdown,
    OHeadingDropdown,
    OHeadingList,
    OListDropdown,
    OIndentDropdown,
    OTextFormatDropdown,
    OAddMoreBtn,
    OPhotoBtn,
    OLinkBtn,
    OTableBtn,
    OTableGroup
  },
  methods: {
    getName (item) {
      return CommandComponents[item] || 'o-simple-command-btn'
    },
    showSidePanel () {},
    onSlideShow () {}
  },
  mounted () {
  },
  deactivated () {
  },
  beforeDestroy () {
  }
}
</script>

<style lang="stylus">
</style>
