<template>
  <div id="gjs" ref="gjs">
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import 'grapesjs/dist/css/grapes.min.css';
import grapesjs from 'grapesjs';
import gjsBlocksBasic from 'grapesjs-blocks-basic';
import grapesjsPluginForms from 'grapesjs-plugin-forms';
import grapesjsPluginExport from 'grapesjs-plugin-export';
import grapesjsPresetWebpage from 'grapesjs-preset-webpage';
import grapesjsTemplates from 'grapesjs-templates';
import grapesjsIndexeddb from 'grapesjs-indexeddb';
import fa from 'grapesjs/locale/fa';
import grapesjsRulers from 'grapesjs-rulers'
import 'grapesjs-rulers/dist/grapesjs-rulers.min.css'
import axios from 'axios';

const editor = ref(null);
const gjs = ref();
const projectEndpoint = "http://172.16.1.63:3500/template";
const currentId = ref('0');


onMounted(() => {


  editor.value = grapesjs.init({
    height: '100%',
    container: gjs.value,
    showOffsets: true,
    fromElement: true,
    noticeOnUnload: false,
    dragMode: 'absolute',
    pageManager: true,
    deviceManager: {
      devices: [{
        name: 'A3',
        width: '1122px',
        height: '793px',
      }, {
        name: 'A4',
        width: '793px',
        height: '1122px',
      },
      {
        name: 'A5',
        width: '561px',
        height: '793px',
      },
      ]
    },
    commands: {
      defaults: [
        {
          id: 'portrait-visibility',
          run() {
            let x = document.querySelector('.gjs-frame-wrapper')
            let w = x.style.width
            let h = x.style.height
            x.style.width = h
            x.style.height = w
          },
        },
        {
          id: 'save-data',
          run() {
            console.log('x')
          }
        }
      ],
    },
    i18n: {
      locale: 'fa',
      localeFallback: 'fa',
      messages: { fa, fa },
    },
    selectorManager: {
      componentFirst: true,
    },
    // storageManager: {
    // type : "local",
    // autosave: true,
    // autoload: true,
    // options: {
    //   local: {
    //     key: 'alireza',
    //   },
    // }
    // },


    storageManager: {
      type: 'remote',
      autosave: true,
      contentTypeJson: true,
      stepsBeforeSave: 3,
      storeComponents: true,
      storeStyles: true,
      storeHtml: true,
      storeCss: true,
    },


    plugins: [gjsBlocksBasic, grapesjsPluginForms, grapesjsPluginExport, grapesjsPresetWebpage, grapesjsIndexeddb, grapesjsTemplates, grapesjsRulers],
    pluginsOpts: {
      gjsBlocksBasic: { flexGrid: true, blocks: ['text', 'link', 'image', 'video', 'map'] },
      grapesjsIndexeddb: {
        options: { key: 'gjsProject', dbName: 'gjs', objectStoreName: 'projects' }
      },
      grapesjsPresetWebpage: {
        modalImportTitle: 'Import Template',
        modalImportLabel: '<div style="margin-bottom: 10px; font-size: 13px;">Paste here your HTML/CSS and click Import</div>',
        modalImportContent: function (editor) {
          return editor.getHtml() + '<style>' + editor.getCss() + '</style>'
        },
        grapesjsTemplates: {
          // templates: 'http://172.16.1.63:3500/template',
          // projects: 'http://172.16.1.63:3500/template',
          onStore: 'http://172.16.1.63:3500/template'
        },
      },
    },
    blockManager: {
      blocks: [
        {
          id: 'footer',
          label: 'footer',
          category: 'constant',
          media: `<svg fill="#000000" version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 512 512" xml:space="preserve"><g><g><g><path d="M490.667,0H21.333C9.536,0,0,9.557,0,21.333V128h512V21.333C512,9.557,502.464,0,490.667,0z M85.333,106.667C73.557,106.667,64,97.109,64,85.333S73.557,64,85.333,64s21.333,9.557,21.333,21.333S97.109,106.667,85.333,106.667zM149.333,106.667c-11.776,0-21.333-9.557-21.333-21.333S137.557,64,149.333,64s21.333,9.557,21.333,21.333S161.109,106.667,149.333,106.667z M213.333,106.667c-11.776,0-21.333-9.557-21.333-21.333S201.557,64,213.333,64s21.333,9.557,21.333,21.333S225.109,106.667,213.333,106.667z"/><rect y="170.667" width="512" height="192"/><path d="M0,490.667C0,502.443,9.536,512,21.333,512h469.333c11.797,0,21.333-9.557,21.333-21.333v-85.333H0V490.667z"/></g></g></g></svg>`,
          content: {
            type: 'div',
            attributes: { class: 'footer' },
            components: [{
              type: 'div',
              content: 'Hello world!!!'
            }, {
              type: 'div',
              content: 'Hello world!!!'
            }],
            styles: `.footer { width:100%; color:red; }`,
          },
          activate: true,
        }
      ],
    },
  });





  const pn = editor.value.Panels;
  const panelViews = pn.addPanel({
    id: 'options'
  });


  panelViews.get('buttons').add([{
    attributes: {
      title: 'Toggle Rulers'
    },
    context: 'toggle-rulers',
    label: `<svg width="18" viewBox="0 0 16 16"><path d="M0 8a.5.5 0 0 1 .5-.5h15a.5.5 0 0 1 0 1H.5A.5.5 0 0 1 0 8z"/><path d="M4 3h8a1 1 0 0 1 1 1v2.5h1V4a2 2 0 0 0-2-2H4a2 2 0 0 0-2 2v2.5h1V4a1 1 0 0 1 1-1zM3 9.5H2V12a2 2 0 0 0 2 2h8a2 2 0 0 0 2-2V9.5h-1V12a1 1 0 0 1-1 1H4a1 1 0 0 1-1-1V9.5z"/></svg>`,
    command: 'ruler-visibility',
    id: 'ruler-visibility'
  }]);


  panelViews.get('buttons').add([{
    attributes: {
      title: 'Toggle Portrait'
    },
    context: 'toggle-portrait',
    label: `<svg fill="#000000" width="18" height="18" viewBox="0 0 36 36" version="1.1"  preserveAspectRatio="xMidYMid meet" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"><title>landscape-line</title><path d="M32,6H4A2,2,0,0,0,2,8V28a2,2,0,0,0,2,2H32a2,2,0,0,0,2-2V8A2,2,0,0,0,32,6Zm0,22H4V8H32Z" class="clr-i-outline clr-i-outline-path-1"></path><path d="M9,22a.82.82,0,0,0,.55-.21.8.8,0,0,0,0-1.13L7.83,18.8H28.17l-1.72,1.86a.8.8,0,0,0,0,1.13A.82.82,0,0,0,27,22a.78.78,0,0,0,.58-.26L31.09,18l-3.47-3.74a.79.79,0,0,0-1.13,0,.8.8,0,0,0,0,1.13l1.72,1.86H7.83l1.72-1.86a.8.8,0,0,0,0-1.13.79.79,0,0,0-1.13,0L4.91,18l3.47,3.74A.78.78,0,0,0,9,22Z" class="clr-i-outline clr-i-outline-path-2"></path><rect x="0" y="0" width="36" height="36" fill-opacity="0"/></svg>`,
    command: 'portrait-visibility',
    id: 'portrait-visibility'
  }]);


  panelViews.get('buttons').add([{
    attributes: {
      title: 'save data'
    },
    context: 'save-data',
    label: `<svg width="18" height="18" viewBox="0 -0.5 21 21" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
    
    <title>save_item [#1411]</title>
    <desc>Created with Sketch.</desc>
    <defs>

</defs>
    <g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
        <g id="Dribbble-Light-Preview" transform="translate(-419.000000, -640.000000)" fill="#000000">
            <g id="icons" transform="translate(56.000000, 160.000000)">
                <path d="M370.21875,484 C370.21875,483.448 370.68915,483 371.26875,483 C371.84835,483 372.31875,483.448 372.31875,484 C372.31875,484.552 371.84835,485 371.26875,485 C370.68915,485 370.21875,484.552 370.21875,484 L370.21875,484 Z M381.9,497 C381.9,497.552 381.4296,498 380.85,498 L379.8,498 L379.8,494 C379.8,492.895 378.86025,492 377.7,492 L369.3,492 C368.13975,492 367.2,492.895 367.2,494 L367.2,498 L366.15,498 C365.5704,498 365.1,497.552 365.1,497 L365.1,487.044 C365.1,486.911 365.15565,486.784 365.2533,486.691 L367.2,484.837 L367.2,486 C367.2,487.105 368.13975,488 369.3,488 L377.7,488 C378.86025,488 379.8,487.105 379.8,486 L379.8,482 L380.85,482 C381.4296,482 381.9,482.448 381.9,483 L381.9,497 Z M377.7,498 L369.3,498 L369.3,495 C369.3,494.448 369.7704,494 370.35,494 L376.65,494 C377.2296,494 377.7,494.448 377.7,495 L377.7,498 Z M369.3,482.837 L370.17885,482 L377.7,482 L377.7,485 C377.7,485.552 377.2296,486 376.65,486 L370.35,486 C369.7704,486 369.3,485.552 369.3,485 L369.3,482.837 Z M381.9,480 L369.7347,480 C369.45645,480 369.18975,480.105 368.99235,480.293 L363.30765,485.707 C363.11025,485.895 363,486.149 363,486.414 L363,498 C363,499.105 363.93975,500 365.1,500 L381.9,500 C383.06025,500 384,499.105 384,498 L384,482 C384,480.895 383.06025,480 381.9,480 L381.9,480 Z" id="save_item-[#1411]"></path>
            </g>
        </g>
    </g>
</svg>`,
    command: 'save-data',
    id: 'sava-data'
  }]);


  editor.value.Storage.add('remote', {
    // async load() {
    //   return await axios.get(projectEndpoint);
    // },

    async store(data) {
      return await axios.post(projectEndpoint, { id: currentId.value, templateData: data }).then(
        (response) => {
          currentId.value = response.data.id
        }
      );
    },
  });




  editor.value.Commands.add('set-device-desktop', {
    run: editor => editor.setDevice('A3')
  });
  editor.value.Commands.add('set-device-tablet', {
    run: editor => editor.setDevice('A4')
  });
  editor.value.Commands.add('set-device-mobile', {
    run: editor => editor.setDevice('A5')
  });
})




// pn.addButton('options', [{ id: 'save-database', className: 'fa fa-floppy-o', command: 'save-database', attributes: { title: 'Save to database' } }])




</script>

<style>
#gjs {
  height: 100%;
}

@media print {
  .gjs-pn-panels {
    display: none;
  }
}
</style>