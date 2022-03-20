# recordVideo

```
var abc = document.querySelector('video');

record(abc, abc.duration * 1000).then(res => {
  const a = document.createElement('a');
  a.href = res;
  a.innnerHTML = 'download';
  a.download = +new Date() + '.webm';
  document.body.appendChild(a);
  a.click();
});

abc.currentTime = 0;

abc.play();

function record(canvas, time) {
  var recordedChunks = [];
  return new Promise(function (res, rej) {
    var stream = canvas.captureStream(30 /*fps*/);
    mediaRecorder = new MediaRecorder(stream, { mimeType: "video/webm; codecs=vp9" });
    mediaRecorder.start(time || 4000);
    mediaRecorder.ondataavailable = function (e) {
      recordedChunks.push(event.data);
      if (mediaRecorder.state === 'recording') {
        mediaRecorder.stop();
      }
    }

    mediaRecorder.onstop = function (event) {
      var blob = new Blob(recordedChunks, { type: "video/webm" });
      var url = URL.createObjectURL(blob);
      res(url);
    }
  })
}
```
