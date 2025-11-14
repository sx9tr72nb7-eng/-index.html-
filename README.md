<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>تجربة الواقع المعزز - المرشد الجامعي الذكي</title>
  <script src="https://aframe.io/releases/1.4.2/aframe.min.js"></script>
  <script src="https://cdn.rawgit.com/AR-js-org/AR.js/master/aframe/build/aframe-ar.js"></script>
  <style>
    body { font-family: Arial, sans-serif; margin: 0; overflow: hidden; }
    button {
      position: fixed;
      top: 20px;
      right: 20px;
      padding: 10px 18px;
      background: #6a4bc3;
      color: white;
      border: none;
      border-radius: 10px;
      font-size: 16px;
      z-index: 12;
      cursor: pointer;
    }
    #info {
      position: fixed;
      left: 10px;
      bottom: 20px;
      background: rgba(255, 255, 255, 0.9);
      padding: 10px 15px;
      border-radius: 8px;
      color: #333;
      font-size: 14px;
      z-index: 12;
    }
  </style>
</head>
<body>
  <h1 style="text-align:center;">📷 تجربة الواقع المعزز - المرشد الجامعي الذكي</h1>

  <a-scene embedded arjs>
    <!-- مثال على ماركر "hiro" -->
    <a-marker preset="hiro" id="marker1">
      <a-box position="0 0.5 0" material="color: #6a4bc3;" rotation="0 45 0"></a-box>
    </a-marker>

    <a-entity camera></a-entity>
  </a-scene>

  <div id="info">وجّه الكاميرا على العلامة للتجربة</div>
  <button id="takePhoto">📸 التقاط صورة</button>

  <script>
    const infoDiv = document.getElementById('info');
    const marker = document.getElementById('marker1');

    // عند ظهور الماركر، عرض إحداثياته
    marker.addEventListener('markerFound', () => {
      const position = marker.object3D.position;
      const rotation = marker.object3D.rotation;
      infoDiv.textContent = `تم العثور على الماركر! الإحداثيات: x=${position.x.toFixed(2)}, y=${position.y.toFixed(2)}, z=${position.z.toFixed(2)} | الدوران: x=${rotation.x.toFixed(1)}, y=${rotation.y.toFixed(1)}, z=${rotation.z.toFixed(1)}`;
    });

    // عند فقدان الماركر
    marker.addEventListener('markerLost', () => {
      infoDiv.textContent = 'وجّه الكاميرا على العلامة للتجربة';
    });

    // زر التقاط الصورة
    document.getElementById('takePhoto').addEventListener('click', () => {
      const scene = document.querySelector('a-scene');
      const canvas = scene.querySelector('canvas');
      if (!canvas) { alert('الكاميرا لم تجهز بعد'); return; }

      const dataURL = canvas.toDataURL('image/png');
      const link = document.createElement('a');
      link.href = dataURL;
      link.download = 'ar_snapshot.png';
      document.body.appendChild(link);
      link.click();
      link.remove();

      infoDiv.textContent = '✅ تم التقاط الصورة!';
      setTimeout(() => {
        infoDiv.textContent = 'وجّه الكاميرا على العلامة للتجربة';
      }, 2000);
    });
  </script>
</body>
</html>
