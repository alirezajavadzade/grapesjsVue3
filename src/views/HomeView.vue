<template>
  <div class="main">

    <!-- main template -->
    <div id="gjs" ref="gjs">
    </div>


    <!-- <div class="sidebar">
      <div class="s-card" v-for="(item, i) in listItem" :key="i">
        <img v-if="item.thumbnail == ''" src="/src/assets/placeholder.png" alt="" width="100%">
        <img v-else :src="`${item.prefix}${item.thumbnail}`" alt="" width="100%">
        <span>{{ item.name }}</span>
      </div>
    </div> -->

    <!-- save modal -->
    <ModalConfirm v-model="saveTemplateModal" title="انتخاب فرمت">
      <div class="save-box">
        <label>نام فرمت خود را وارد کنید : </label>
        <input v-model="inputName" type="text" id="save-input">
      </div>
      <div class="save-box">
        <label>عکس قالب را وارد کنید : </label>
        <input type="file" id="thumbnail-input" accept="image/jpg" @change="templateImage">
      </div>
      <div class="action-box">
        <button class="save-btn" @click="saveTemplate">ذخیره</button>
      </div>
    </ModalConfirm>

    <!-- list modal  -->
    <ModalConfirm v-model="loadTemplateModal" title="فرمت ها">
      <div class="cardBox" v-if="listItem.length !== 0">
        <div class="card" v-for="(item, i) in listItem" :key="i" @click="getItem(item.id)">
          <deleteIcon class="delete-icon" @click="deleteTemplate(item.id)" />
          <img v-if="item.thumbnail == ''" src="/src/assets/placeholder.png" alt="" width="100%">
          <img v-else :src="`${item.prefix}${item.thumbnail}`" alt="" width="100%">
          <h3>{{ item.name }}</h3>
        </div>
      </div>
      <div class="cardBox" v-else>
        <p>دیتای ذخیره شده ای ندارید!</p>
      </div>
    </ModalConfirm>

    <!-- update modal -->
    <ModalConfirm v-model="updateTemplateModal" title="به روز رسانی فرمت">
      <div>
        <h4>آیا قالب قبلا ذخیره شده است</h4>
        <p>آیا میخواهید این قالب به روز رسانی شود؟</p>
      </div>
      <div class="action-box">
        <button class="save-btn" @click="updateTemplate">ذخیره</button>
        <button class="save-btn" @click="updateTemplateModal = false">انصراف</button>
      </div>
    </ModalConfirm>

    <!-- delete modal -->
    <ModalConfirm v-model="deleteTemplateModal" title="حذف فرمت">
      <h3 class="modal-text">آیا از حذف این فرمت مطمئن هستید؟</h3>
      <div class="action-box">
        <button class="save-btn submit" @click="submitDelete">حذف</button>
        <button class="save-btn" @click="deleteTemplateModal = false">انصراف</button>
      </div>
    </ModalConfirm>

  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import 'grapesjs/dist/css/grapes.min.css';
import grapesjs from 'grapesjs';
import fa from 'grapesjs/locale/fa';
import gjsBlocksBasic from 'grapesjs-blocks-basic';
import grapesjsPluginForms from 'grapesjs-plugin-forms';
import grapesjsPluginExport from 'grapesjs-plugin-export';
import grapesjsPresetWebpage from 'grapesjs-preset-webpage';
import grapesjsRulers from 'grapesjs-rulers';
import 'grapesjs-rulers/dist/grapesjs-rulers.min.css';
import axios from 'axios';
import ModalConfirm from '@/components/ModalConfirm.vue';
import deleteIcon from '@/assets/delete.svg';




let editor = "";
const gjs = ref();
const projectEndpoint = "http://172.16.1.63:3500/template";
const beforebase64Url = ref();
const thumbnailInput = ref('');
const obj = ref({});
const updateObj = ref({});
const listItem = ref([]);
const saveTemplateModal = ref(false);
const loadTemplateModal = ref(false);
const updateTemplateModal = ref(false);
const deleteTemplateModal = ref(false)
const inputName = ref('');
const templateId = ref('');
const currentItem = ref('');
const currentId = ref('');
const screenH = ref('');
const screenW = ref('');




const templateImage = (ev) => {
  // convert to base64
  let file = ev.target.files[0]
  let reader = new FileReader();
  reader.onloadend = function () {
    let base64Url = reader.result.split(';base64,');
    beforebase64Url.value = base64Url[0] + ';base64,';
    thumbnailInput.value = base64Url[1];
  }
  reader.readAsDataURL(file);
}

const saveTemplate = async () => {
  const deviceManager = editor.Devices;
  const selected = deviceManager.getSelected();
  const storedProjectData = await editor.store();
  let x = document.querySelector('.gjs-frame-wrapper');
  obj.value.id = storedProjectData.pages[0].id
  obj.value.data = JSON.stringify(storedProjectData);
  obj.value.name = inputName.value;
  obj.value.thumbnail = thumbnailInput.value;
  obj.value.prefix = beforebase64Url.value;
  obj.value.size = selected.id;
  axios.post(projectEndpoint, { templateData: obj.value },).then(
    (response) => {
      saveTemplateModal.value = false;
    })
}

const getList = () => {
  axios.get(projectEndpoint).then(
    (response) => {
      listItem.value = response.data
    })
}

const getItem = (e) => {
  axios.get(`${projectEndpoint}/${e}`).then(
    (response) => {
      loadTemplateModal.value = false;
      currentItem.value = response.data[0]
      const currentData = JSON.parse(currentItem.value.data)
      editor.loadProjectData(currentData);
      currentId.value = currentData.pages[0].id;
      editor.setDevice(response.data[0].size);
    })
}

const updateTemplate = async () => {
  const storedProjectData = await editor.store();
  updateObj.value.data = JSON.stringify(storedProjectData);
  updateObj.value.name = currentItem.value.name;
  updateObj.value.thumbnail = currentItem.value.thumbnail;
  updateObj.value.prefix = currentItem.value.prefix
  axios.put(`${projectEndpoint}/${currentId.value}`, { templateData: updateObj.value }).then(
    (response) => {
      updateTemplateModal.value = false;
    }
  )
}

const deleteTemplate = (e) => {
  templateId.value = e;
  deleteTemplateModal.value = true;
}

const submitDelete = () => {
  axios.delete(`${projectEndpoint}/${templateId.value}`).then(
    (response) => {
      deleteTemplateModal.value = false;
      loadTemplateModal.value = false;
      getList()
    })
}

const printMethod = () => {
  let wrapper = document.querySelector('.gjs-frame')
  console.log(wrapper)
  window.print()
}



onMounted(() => {
  getList()
  editor = grapesjs.init({
    height: '100%',
    container: gjs.value,
    showOffsets: true,
    fromElement: true,
    noticeOnUnload: false,
    dragMode: 'absolute',
    pageManager: false,
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
        width: '510px',
        height: '760px',
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

            // console.log(h)
            if (h > w) {
              console.log('landscape')
            } else {
              console.log('portrait')
            }
          },
        },
        {
          id: 'save-data',
          run() {
            if (currentId.value !== '') {
              updateTemplateModal.value = true;
            } else {
              saveTemplateModal.value = true;
            }
          }
        },
        {
          id: 'load-data',
          async run() {
            loadTemplateModal.value = true
            await getList()
          }
        },
        {
          id: 'print',
          run() {
            printMethod()
          }
        },
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
    storageManager: false,
    plugins: [gjsBlocksBasic, grapesjsPluginForms, grapesjsPluginExport, grapesjsPresetWebpage, grapesjsRulers],
    pluginsOpts: {
      gjsBlocksBasic: { flexGrid: true, blocks: ['text', 'link', 'image', 'video', 'map'] },
      grapesjsPresetWebpage: {
        modalImportTitle: 'Import Template',
        modalImportLabel: '<div style="margin-bottom: 10px; font-size: 13px;">Paste here your HTML/CSS and click Import</div>',
        modalImportContent: function (editor) {
          return editor.getHtml() + '<style>' + editor.getCss() + '</style>'
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
            type: 'default',
            attributes: { class: 'footer' },
            components: [{
              type: 'default',
              content: 'Hello world!!!'
            }, {
              type: 'default',
              content: 'Hello world!!!'
            }],
            styles: `.footer { width:100%; color:red; }`,
          },
          activate: true,
        }
      ],
    },
  });


  const pn = editor.Panels;
  const panelViews = pn.addPanel({
    id: 'options'
  });

  const dv = editor.Panels;
  const panelDevices = dv.addPanel({
    id: 'devices-c'
  })



  // right side buttons
  panelViews.get('buttons').add([{
    attributes: {
      title: 'Toggle Rulers'
    },
    context: 'toggle-rulers',
    label: `<svg width="17" viewBox="0 0 16 16"><path d="M0 8a.5.5 0 0 1 .5-.5h15a.5.5 0 0 1 0 1H.5A.5.5 0 0 1 0 8z"/><path d="M4 3h8a1 1 0 0 1 1 1v2.5h1V4a2 2 0 0 0-2-2H4a2 2 0 0 0-2 2v2.5h1V4a1 1 0 0 1 1-1zM3 9.5H2V12a2 2 0 0 0 2 2h8a2 2 0 0 0 2-2V9.5h-1V12a1 1 0 0 1-1 1H4a1 1 0 0 1-1-1V9.5z"/></svg>`,
    command: 'ruler-visibility',
    id: 'ruler-visibility'
  }]);


  panelViews.get('buttons').add([{
    attributes: {
      title: 'save data'
    },
    context: 'save-data',
    label: `<svg fill="#b9a5a6" width="17px" height="17px" viewBox="0 0 32 32" style="fill-rule:evenodd;clip-rule:evenodd;stroke-linejoin:round;stroke-miterlimit:2;" version="1.1" xml:space="preserve" xmlns="http://www.w3.org/2000/svg" xmlns:serif="http://www.serif.com/" xmlns:xlink="http://www.w3.org/1999/xlink" stroke="#b9a5a6">

<g id="SVGRepo_bgCarrier" stroke-width="0"/>

<g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"/>

<g id="SVGRepo_iconCarrier">

<path d="M9,29l0,-8.25c0,-1.518 1.232,-2.75 2.75,-2.75l8.5,0c1.518,0 2.75,1.232 2.75,2.75l0,8.25l-14,-0Zm-2,-0.101c-0.953,-0.195 -1.837,-0.665 -2.536,-1.363c-0.937,-0.938 -1.464,-2.21 -1.464,-3.536c-0,-4.439 -0,-11.561 0,-16c-0,-1.326 0.527,-2.598 1.464,-3.536c0.938,-0.937 2.21,-1.464 3.536,-1.464l2,-0l0,5.083c0,2.201 1.613,3.917 3.5,3.917l5,0c1.887,0 3.5,-1.716 3.5,-3.917l0,-5.083l0.221,0c0.24,0 0.472,0.087 0.654,0.244l5.779,5c0.22,0.19 0.346,0.466 0.346,0.756c0,0 0,9.426 -0,15c0,1.326 -0.527,2.598 -1.464,3.536c-0.699,0.698 -1.583,1.168 -2.536,1.363l0,-8.149c0,-2.622 -2.128,-4.75 -4.75,-4.75c0,0 -8.5,0 -8.5,0c-2.622,0 -4.75,2.128 -4.75,4.75l0,8.149Zm13,-25.899l0,5.083c0,1.02 -0.626,1.917 -1.5,1.917c0,0 -5,0 -5,0c-0.874,0 -1.5,-0.897 -1.5,-1.917l0,-5.083l8,0Z"/>

<g id="Icon"/>

</g>

</svg>`,
    command: 'save-data',
    id: 'sava-data'
  }]);


  panelViews.get('buttons').add([{
    attributes: {
      title: 'load data'
    },
    context: 'load-data',
    label: `<svg width="17px" height="17px" viewBox="0 0 21 21" fill="none" xmlns="http://www.w3.org/2000/svg" stroke="#b9a5a6">

<g id="SVGRepo_bgCarrier" stroke-width="0"/>

<g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"/>

<g id="SVGRepo_iconCarrier"> <path d="M3 4H21" stroke="#b9a5a6" stroke-width="2" stroke-linecap="round"/> <path fill-rule="evenodd" clip-rule="evenodd" d="M19 4H5V15.8C5 16.9201 5 17.4802 5.21799 17.908C5.40973 18.2843 5.71569 18.5903 6.09202 18.782C6.51984 19 7.0799 19 8.2 19H15.8C16.9201 19 17.4802 19 17.908 18.782C18.2843 18.5903 18.5903 18.2843 18.782 17.908C19 17.4802 19 16.9201 19 15.8V4ZM11 15V11.4142L9.70711 12.7071C9.31658 13.0976 8.68342 13.0976 8.29289 12.7071C7.90237 12.3166 7.90237 11.6834 8.29289 11.2929L11.1868 8.39896C11.2053 8.38051 11.2242 8.36282 11.2436 8.34589C11.4269 8.13403 11.6978 8 12 8C12.3022 8 12.5731 8.13403 12.7564 8.34589C12.7758 8.36282 12.7947 8.38051 12.8132 8.39896L15.7071 11.2929C16.0976 11.6834 16.0976 12.3166 15.7071 12.7071C15.3166 13.0976 14.6834 13.0976 14.2929 12.7071L13 11.4142V15C13 15.5523 12.5523 16 12 16C11.4477 16 11 15.5523 11 15Z" fill="#b9a5a6"/> </g>

</svg>`,
    command: 'load-data',
    id: 'load-data'
  }]);


  panelViews.get('buttons').add([{
    attributes: {
      title: 'print'
    },
    context: 'print',
    label: `<svg width="17px" height="17px" viewBox="0 0 21 21" fill="none" xmlns="http://www.w3.org/2000/svg" stroke="#b9a5a6">

<g id="SVGRepo_bgCarrier" stroke-width="0"/>

<g id="SVGRepo_tracerCarrier" stroke-linecap="round" stroke-linejoin="round"/>

<g id="SVGRepo_iconCarrier"> <path d="M3 4H21" stroke="#b9a5a6" stroke-width="2" stroke-linecap="round"/> <path fill-rule="evenodd" clip-rule="evenodd" d="M19 4H5V15.8C5 16.9201 5 17.4802 5.21799 17.908C5.40973 18.2843 5.71569 18.5903 6.09202 18.782C6.51984 19 7.0799 19 8.2 19H15.8C16.9201 19 17.4802 19 17.908 18.782C18.2843 18.5903 18.5903 18.2843 18.782 17.908C19 17.4802 19 16.9201 19 15.8V4ZM11 15V11.4142L9.70711 12.7071C9.31658 13.0976 8.68342 13.0976 8.29289 12.7071C7.90237 12.3166 7.90237 11.6834 8.29289 11.2929L11.1868 8.39896C11.2053 8.38051 11.2242 8.36282 11.2436 8.34589C11.4269 8.13403 11.6978 8 12 8C12.3022 8 12.5731 8.13403 12.7564 8.34589C12.7758 8.36282 12.7947 8.38051 12.8132 8.39896L15.7071 11.2929C16.0976 11.6834 16.0976 12.3166 15.7071 12.7071C15.3166 13.0976 14.6834 13.0976 14.2929 12.7071L13 11.4142V15C13 15.5523 12.5523 16 12 16C11.4477 16 11 15.5523 11 15Z" fill="#b9a5a6"/> </g>

</svg>`,
    command: 'print',
    id: 'print'
  }]);





  // left side buttons
  // panelDevices.get('buttons').add([{
  //   attributes: {
  //     title: 'A3'
  //   },
  //   context: 'A3',
  //   label: `A3`,
  //   command: 'A3',
  //   id: 'A3'
  // }]);

  // panelDevices.get('buttons').add([{
  //   attributes: {
  //     title: 'A4'
  //   },
  //   context: 'A4',
  //   label: `A4`,
  //   command: 'A4',
  //   id: 'A4'
  // }]);

  // panelDevices.get('buttons').add([{
  //   attributes: {
  //     title: 'A5'
  //   },
  //   context: 'A5',
  //   label: `A5`,
  //   command: 'A5',
  //   id: 'A5'
  // }]);


  panelDevices.get('buttons').add([{
    attributes: {
      title: 'Toggle Portrait'
    },
    context: 'toggle-portrait',
    label: `<svg fill="#000000" width="17" height="17" viewBox="0 0 36 36" version="1.1"  preserveAspectRatio="xMidYMid meet" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"><title>landscape-line</title><path d="M32,6H4A2,2,0,0,0,2,8V28a2,2,0,0,0,2,2H32a2,2,0,0,0,2-2V8A2,2,0,0,0,32,6Zm0,22H4V8H32Z" class="clr-i-outline clr-i-outline-path-1"></path><path d="M9,22a.82.82,0,0,0,.55-.21.8.8,0,0,0,0-1.13L7.83,18.8H28.17l-1.72,1.86a.8.8,0,0,0,0,1.13A.82.82,0,0,0,27,22a.78.78,0,0,0,.58-.26L31.09,18l-3.47-3.74a.79.79,0,0,0-1.13,0,.8.8,0,0,0,0,1.13l1.72,1.86H7.83l1.72-1.86a.8.8,0,0,0,0-1.13.79.79,0,0,0-1.13,0L4.91,18l3.47,3.74A.78.78,0,0,0,9,22Z" class="clr-i-outline clr-i-outline-path-2"></path><rect x="0" y="0" width="36" height="36" fill-opacity="0"/></svg>`,
    command: 'portrait-visibility',
    id: 'portrait-visibility'
  }]);



  editor.Commands.add('set-device-desktop', {
    run: editor => editor.setDevice('A3')
  });
  editor.Commands.add('set-device-tablet', {
    run: editor => editor.setDevice('A4')
  });
  editor.Commands.add('set-device-mobile', {
    run: editor => editor.setDevice('A5')
  });

})

const langs = {
  'Map': 'نقشه',
  '1 Column': 'یک ستون',
  '2 Columns': 'دو ستون',
  '3 Columns': 'سه ستون',
  '2 Columns 3/7': 'دو ستون 3/7',
  'Text': 'متن',
  'Link': 'لینک',
  'Image': 'عکس',
  'Video': 'ویدئو',
  'Map': 'نقشه',
  'Link Block': 'جعبه لینک',
  'Quote': 'نقل قول',
  'Text section': 'جعبه متن',
  'Form': 'فرم',
  'Input': 'متن ورودی',
  'Textarea': 'فضای متن',
  'Select': 'باکس انتخابی',
  'Button': 'دکمه',
  'Label': 'برچسب',
  'Checkbox': 'گزینش',
  'Radio': 'دکمه رادیو',
  'footer': 'پاورقی',
}


setTimeout(() => {
  let items = document.querySelectorAll('.gjs-block.gjs-one-bg')
  for (const item of items) {
    if (item.getAttribute('title') == undefined) {
      continue
    }
    let key = item.getAttribute('title')
    item.children[1].innerHTML = langs[key]
  }


  // DELETE BLOCKS
  let Column1 = document.querySelector('[title="1 Column"]')
  Column1.remove();


  // DELETE DEVICE MANAGER
  let deviceBtns = document.querySelector('.gjs-pn-devices-c');
  deviceBtns.querySelectorAll('svg[style*="display: block; max-width:22px"]').forEach((item, i) => {
    item.parentNode.setAttribute('id', `id${i}`)
  })

  document.getElementById('id0').innerHTML = 'A3';
  document.getElementById('id1').innerHTML = 'A4';
  document.getElementById('id2').innerHTML = 'A5';




  // change name of title
  let nameList = ['ثابت', 'متغیر', 'اضافی']
  let titleName = document.querySelectorAll('.gjs-title')
  titleName.forEach((item, i) => {
    item.innerHTML = nameList[i]
  })

}, 100)

</script>




<style>
.main {
  width: 100%;
  height: 100%;
  position: relative;
}

#gjs {
  height: 100%;
}

@media print {
  .gjs-pn-panels {
    display: none;
  }
}

.save-box {
  display: flex;
  justify-content: space-between;
  align-items: center;
  direction: rtl;
  padding: 2rem 0.5rem;
}

.save-box input {
  margin: 0.5rem;
  padding: 0.5rem;
  border-radius: 8px;
  border: 1px solid #000;
}

.save-box input:focus-visible {
  outline: none;
}

.my-small-modal .gjs-mdl-dialog {
  max-width: 500px;
}

.save-btn {
  border: 0;
  margin: 0.5rem;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  background: #fff;
  cursor: pointer;
}

.save-btn.submit {
  background: #858585 !important;
}

.card-box {
  display: flex;
  flex-wrap: wrap;
}

.card {
  width: 100%;
  max-width: 10rem;
  padding: 0.5rem;
  border-radius: 4px;
  box-shadow: 0px 0px 8px 0px rgba(117, 117, 117, 0.75);
  margin: 1rem;
  position: relative;
}

.card img {
  aspect-ratio: 16 / 9;
  border-radius: 8px;
}

.card h3 {
  text-align: end;
  margin: 0.8rem 0;
}

.sidebar {
  width: 8rem;
  height: 100%;
  max-height: 95vh;
  background: #e9e9e9 !important;
  position: absolute;
  top: 2.6rem;
  left: 0;
  z-index: 1;
  overflow-y: auto;
  padding: 0.5rem;
}

.sidebar .s-card {
  padding: 0.2rem;
  margin: 0.3rem 0;
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border-radius: 8px;
  box-shadow: 0px 0px 8px 0px rgba(117, 117, 117, 0.75);
}

.sidebar .s-card img {
  border-radius: 4px;
  max-width: 3rem;
  max-height: 2rem;
  aspect-ratio: 16 / 9;
}

/* modal */
.save-modal {
  display: flex;
  position: fixed;
  width: 30rem;
  height: 30rem;
  border-radius: 16px;
  position: fixed;
  left: 35%;
  top: 20%;
  background: #3c3133;
  z-index: 100;
}

.cardBox {
  display: flex;
  flex-wrap: wrap;
}

.delete-icon {
  position: absolute;
  top: -15px;
  left: -15px;
  background-color: #fff;
  margin: 0.3rem;
  border-radius: 100%;
  cursor: pointer;
}

.modal-text {
  text-align: center;
}

.gjs-pn-btn {
  display: flex;
  justify-content: center;
  align-items: center;
}

.gjs-one-bg {
  background: #fff !important;
}

.gjs-two-color {
  color: #009dd6 !important;
}

.gjs-four-color-h {
  color: #009dd6 !important;
}

.gjs-four-color {
  color: #009dd6 !important;
}

.gjs-block-label {
  line-height: unset;
}

.gjs-title {
  text-align: right !important;
  font-size: 14px;
  font-weight: bold;
}
</style>
