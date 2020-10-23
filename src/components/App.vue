<template>
    <Page>
        <ActionBar title="PepeCam"/>
        <GridLayout columns="*" rows="*">
            <Label class="message" :text="msg" col="0" row="0"/>
        </GridLayout>
        <StackLayout>
          <Button text="Tomar Foto" @tap="TomarCaptura" />
          <Button text="Escoge una Foto" @tap="selectPicture" />
          <WrapLayout>
            <Image v-for="img in images" v-bind:key="img.id" :src="img.src" width="150" height="150"/>
          </WrapLayout>
        </StackLayout>
    </Page>
</template>

<script >
//import * as camera from "nativescript-camera";
import * as imagepicker from "nativescript-imagepicker";

import { isAvailable, requestCameraPermissions, takePicture } from '@nativescript/camera';
import { Image } from "tns-core-modules/ui/image";

  export default {
    data() {
      return {
        images:[]
		  }
    },
    methods:{
      selectPicture(){
        let context = imagepicker.create({
          mode: 'multiple'
        });

        context.authorize()
        .then(function(){
          return context.present();
        })
        .then(selection => {
          selection.forEach(selected => {

            console.log(JSON.stringify(selected));

            let img = new Image();
            img.src = selected;
            this.images.push(img);
          });
        }).catch(function (e) {
          console.log('error in selectPicture', e);
        });

      },

      TomarCaptura() {
        requestCameraPermissions()
        .then(() => {
          takePicture({ width: 300, height: 300, keepAspectRatio: true, saveToGallery:true })
          .then(imageAsset => {
            let img = new Image();
            img.src = imageAsset;
            this.images.push(img);
            console.log('Ive got '+this.images.length+' images now.');
          })
          .catch(e => {
            console.log('error:', e);
          });
        })
        .catch(e => {
          console.log('Error requesting permission');
        });
      },

    }
  }
</script>

<style scoped>
    ActionBar {
        background-color: #53ba82;
        color: #ffffff;
    }

    .message {
        vertical-align: center;
        text-align: center;
        font-size: 20;
        color: #333333;
    }
</style>
