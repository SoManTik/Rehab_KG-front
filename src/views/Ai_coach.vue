<script setup>
import { PoseLandmarker, FilesetResolver, DrawingUtils } from '@mediapipe/tasks-vision'
import { onMounted, ref } from 'vue';
import { useRouter } from 'vue-router'
import Camera from "simple-vue-camera";


const router = useRouter()


const image = ref(null)
// const poseLandmarkerResult = ref({})
const webcam_stream = ref(null)
const output_canvas = ref(null)
const lastVideoTime = ref(0)
let poseLandmarker = null;
var videoHeight = "";
var videoWidth = "";
var is_pause = ref(false)

function renderLoop() {
  var divElement = document.getElementById("video");
  videoWidth = divElement.offsetWidth + 'px';
  videoHeight = divElement.offsetHeight + 'px';

  const video = webcam_stream.value.video

  const canvasElement = document.getElementById("output_canvas");
  const canvasCtx = canvasElement.getContext("2d");
  const drawingUtils = new DrawingUtils(canvasCtx);

  let startTimeMs = performance.now();

  canvasElement.style.height = videoHeight;
  // video.style.height = videoHeight;
  canvasElement.style.width = videoWidth;
  // video.style.width = videoWidth;
  if(is_pause.value==false){
  if (video.currentTime !== lastVideoTime.value) {
    const poseLandmarkerResult = poseLandmarker.detectForVideo(video, startTimeMs, (result) => {
      // console.log(result)
      canvasCtx.save();
      canvasCtx.clearRect(0, 0, canvasElement.width, canvasElement.height);
      for (const landmark of result.landmarks) {
        drawingUtils.drawLandmarks(landmark, {
          radius: (data) => DrawingUtils.lerp(data.from.z, -0.15, 0.1, 5, 1)

        });
        drawingUtils.drawConnectors(landmark, PoseLandmarker.POSE_CONNECTIONS);
      }
      canvasCtx.restore();
    });

    // processResults(detections);
    lastVideoTime.value = video.currentTime;
  }
}


  requestAnimationFrame(() => {
    renderLoop();
  });
}

onMounted(async () => {
  let runningMode = "IMAGE";
  // let poseLandmarker;
  const videoHeight = "360px";
  const videoWidth = "480px";
  const vision = await FilesetResolver.forVisionTasks(
    "https://cdn.jsdelivr.net/npm/@mediapipe/tasks-vision@0.10.0/wasm"
  );
  poseLandmarker = await PoseLandmarker.createFromOptions(vision, {
    baseOptions: {
      modelAssetPath: `https://storage.googleapis.com/mediapipe-models/pose_landmarker/pose_landmarker_lite/float16/1/pose_landmarker_lite.task`,
      delegate: "GPU"
    },
    runningMode: runningMode,
    numPoses: 1
  });
  await poseLandmarker.setOptions({ runningMode: "VIDEO" });



})

var is_start_teaching_video = ref(true);
function start_teaching_video() {
  is_start_teaching_video.value = true
  is_pause.value = false

}

function start_exercise() {
  is_start_teaching_video.value = false
  is_pause.value = false


}
function to_Pause() {
  is_pause.value = true

}



function to_Restart() {
  console.log('dmckd')
  router.go('/Ai_coach')
}
</script>

<template>
  <main>


    <div class="container">

      <div class="row">

        <div class="col-11" style="margin-top: 1%;" v-if="is_start_teaching_video == false">
          <camera crossOrigin="anonymous" playsinline ref="webcam_stream" id="video" class="" autoplay
            @started="renderLoop">
          </camera>
          <canvas class="output_canvas" ref="output_canvas" id="output_canvas" width="1280" height="720"
            style="position: absolute; left: left; top: 0px;">
          </canvas>
        </div>

        <iframe style="margin-top: 1%;" v-if="is_start_teaching_video == true" class="col-12" width="1280" height="720"
          src="https://www.youtube.com/embed/KNfqxl7jkiU"></iframe>


      </div>
      <div style="margin-top: 1%;" class="row">
        <div class="d-grid gap-2 d-md-block">
          <a href="/ExerciseTask"><button class="btn btn-primary" type="button">
            < Back</button></a>

              <div class="btn-group" role="group" aria-label="Basic radio toggle button group" style="margin-left: 1%;">
                <input @click="start_teaching_video()" type="radio" class="btn-check" name="btnradio" id="btnradio1"
                  autocomplete="off" checked>
                <label class="btn btn-outline-primary" for="btnradio1">Teaching Video</label>

                <input type="radio" class="btn-check" name="btnradio" id="btnradio2" autocomplete="off">
                <label class="btn btn-outline-primary" for="btnradio2" @click="start_exercise()">Start</label>

                <input type="radio" class="btn-check" name="btnradio" id="btnradio3" autocomplete="off" data-bs-toggle="modal" data-bs-target="#staticBackdrop">
                <label class="btn btn-outline-primary" for="btnradio3" @click="to_Pause()">Pause</label>

                <input type="radio" class="btn-check" name="btnradio" id="btnradio4" autocomplete="off"  >
                <label class="btn btn-outline-primary" for="btnradio4" @click="to_Restart()">Restart</label>
              </div>
        </div>

      </div>


<!-- Modal -->
<div class="modal fade" id="staticBackdrop" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
  <div class="modal-dialog modal-dialog-centered">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="staticBackdropLabel">Message</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        You paused the exercise.
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary" data-bs-dismiss="modal" @click="start_exercise()">Continue</button>
      </div>
    </div>
  </div>
</div>


    </div>


  </main>
</template>
