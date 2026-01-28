<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>CLICK ME</title>
  <style>
    html, body {
      width: 100%;
      height: 100%;
      margin: 0;
      background: #222;
      display: flex;
      justify-content: center;
      align-items: center;
      font-family: Arial, sans-serif;
      color: white;
      cursor: pointer;
      user-select: none;
    }
    h1 {
      font-size: 5vw;
    }
  </style>
</head>
<body>
  <h1>CLICK ME</h1>

  <button id="launch" style="position:absolute;width:100vw;height:100vh;opacity:0;border:none;cursor:pointer;"></button>

  <script>
    let popups = [];
    let Cpressed = false;

    function openPopup() {
      if (Cpressed) return; // stop reopening after C
      const p = window.open(
        "popup.html", // change this if using hosted GitHub Pages full link
        "_blank",
        "width=600,height=400"
      );
      if (p) popups.push(p);
    }

    document.getElementById("launch").onclick = () => {
      openPopup();

      // Monitor popups quickly (every 100ms)
      setInterval(() => {
        if (Cpressed) return;
        // Check if any popup was closed
        let closed = popups.filter(p => p.closed);
        if (closed.length > 0) {
          popups = popups.filter(p => !p.closed);
          // Open 3 new popups
          for (let i = 0; i < 3; i++) openPopup();
        }
      }, 100);
    };

    // Listen for messages from popups (C key)
    window.addEventListener("message", e => {
      if (e.data === "close-all") {
        Cpressed = true;
        popups.forEach(p => { if (!p.closed) p.close(); });
        popups = [];
      }
    });
  </script>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>⚠️ SYSTEM NOTICE ⚠️</title>
  <style>
    html, body {
      margin: 0;
      width: 100%;
      height: 100%;
      background: red;
      color: white;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      font-family: Arial, sans-serif;
      text-align: center;
      user-select: none;
    }
    h1 {
      font-size: 4vw;
    }
    p {
      font-size: 2vw;
    }
  </style>
</head>
<body>
  <h1>⚠️ SYSTEM NOTICE ⚠️</h1>
  <p>Please wait.</p>
  <p>Do not close this window.</p>

  <script>
    // Flash title
    setInterval(() => {
      document.title =
        document.title === "⚠️ SYSTEM NOTICE ⚠️"
          ? "ALERT!"
          : "⚠️ SYSTEM NOTICE ⚠️";
    }, 500);

    // Disable right-click
    document.addEventListener("contextmenu", e => e.preventDefault());

    // Press C to close all popups
    document.addEventListener("keydown", e => {
      if (e.key.toLowerCase() === "c") {
        window.opener?.postMessage("close-all", "*");
      }
    });
  </script>
</body>
</html>
