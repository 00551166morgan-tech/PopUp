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
      color: white;
      font-family: Arial, sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
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

  <script>
    let popups = [];
    let Cpressed = false;

    function openPopup() {
      if (Cpressed) return;
      const p = window.open(
        "popup.html", // adjust if hosted elsewhere
        "_blank",
        "width=600,height=400"
      );
      if (p) popups.push(p);
    }

    // Click anywhere to open popup
    document.body.addEventListener("click", () => {
      openPopup();

      // Monitor popups quickly
      setInterval(() => {
        if (Cpressed) return;

        // Check for closed popups
        let closed = popups.filter(p => p.closed);
        if (closed.length > 0) {
          popups = popups.filter(p => !p.closed);
          // Open 3 new popups
          for (let i = 0; i < 3; i++) openPopup();
        }
      }, 100);
    });

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
