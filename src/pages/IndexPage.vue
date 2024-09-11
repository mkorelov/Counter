<template>

  <q-page class="flex flex-center">

    <div class="row full-width items-center">

      <div class="col-3 text-right">

        <q-btn

          size="35px"
          round
          color="red-14"
          icon="mic"
          id="startButton"

        />

      </div>

      <div class="col-6 text-center">

        <audio controls id="audioControls"></audio>

      </div>

      <div class="col-3 text-left">

       <q-btn

          size="35px"
          round
          color="red-14"
          icon="front_hand"
          id="stopButton"

        />

      </div>

    </div>

  </q-page>

</template>


<script>

  let audioIn = { audio: true };

    navigator.mediaDevices.getUserMedia(audioIn)

      .then(function (mediaStreamObj) {

        let start = document.getElementById('startButton');

        let stop = document.getElementById('stopButton');

        let playAudio = document.getElementById('audioControls');

        let mediaRecorder = new MediaRecorder(mediaStreamObj);

        start.addEventListener('click', function (ev) {
          mediaRecorder.start();
        })

        stop.addEventListener('click', function (ev) {
          mediaRecorder.stop();
        });

        mediaRecorder.ondataavailable = function (ev) {
          dataArray.push(ev.data);
        }

        let dataArray = [];

        mediaRecorder.onstop = function (ev) {

          let audioData = new Blob(dataArray,
                    { 'type': 'audio/mp3;' });

          dataArray = [];

          let audioSrc = window.URL
              .createObjectURL(audioData);

          playAudio.src = audioSrc;
        }
      })

      .catch(function (err) {
        console.log(err.name, err.message);
      });

</script>


<style scoped>

  .q-page {
    max-width: 700px;
    margin: 0 auto;
  }

</style>
