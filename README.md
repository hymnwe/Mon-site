<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Caméra + GPS + Écran</title>
  <style>
    body {
      background-color: #000;
      color: #fff;
      text-align: center;
      font-family: Arial, sans-serif;
    }
    video {
      width: 80%;
      max-width: 600px;
      margin: 10px 0;
      border: 2px solid #fff;
    }
    button {
      padding: 10px 20px;
      margin: 10px;
      font-size: 18px;
      background: #444;
      color: #fff;
      border: none;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <h1>Caméra + GPS + Partage écran</h1>
  <video id="cameraVideo" autoplay></video>
  <p id="location">Localisation : en attente…</p>
  <button onclick="shareScreen()">Partager l’écran</button>
  <video id="screenVideo" autoplay></video>

  <script>
    // Caméra
    navigator.mediaDevices.getUserMedia({ video: true, audio: false })
    .then(stream => {
      document.getElementById('cameraVideo').srcObject = stream;
    })
    .catch(err => console.error("Erreur caméra : " + err));

    // Localisation
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(pos => {
        document.getElementById('location').innerText =
          `Latitude: ${pos.coords.latitude}, Longitude: ${pos.coords.longitude}`;
      }, err => {
        document.getElementById('location').innerText =
          "Erreur localisation : " + err.message;
      });
    } else {
      document.getElementById('location').innerText = "Géolocalisation non supportée.";
    }

    // Partage d'écran
    function shareScreen() {
      navigator.mediaDevices.getDisplayMedia({ video: true })
      .then(stream => {
        document.getElementById('screenVideo').srcObject = stream;
      })
      .catch(err => console.error("Erreur partage écran : " + err));
    }
  </script>
</body>
</html>