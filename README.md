# JavaScript-III-Day-2
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>DOM Performance Test</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 20px;
      background: #f4f6f8;
    }

    button {
      margin: 5px;
      padding: 10px 15px;
      cursor: pointer;
      border: none;
      background: #007bff;
      color: white;
      border-radius: 5px;
    }

    #cardContainer {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
      gap: 10px;
      margin-top: 20px;
    }

    .card {
      background: white;
      padding: 15px;
      border-radius: 5px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
      text-align: center;
    }

    #resultBox {
      margin-top: 20px;
      padding: 15px;
      background: #222;
      color: #00ff99;
      border-radius: 5px;
      font-family: monospace;
    }
  </style>
</head>
<body>

  <h2>DOM Performance Comparison</h2>

  <button onclick="generateInnerHTML()">innerHTML</button>
  <button onclick="generateCreateElement()">createElement</button>
  <button onclick="generateFragment()">DocumentFragment</button>
  <button onclick="generateInsertAdjacent()">insertAdjacentHTML</button>
  <button onclick="generateTemplate()">Template</button>

  <div id="resultBox">Performance results will appear here...</div>

  <div id="cardContainer"></div>

  <!-- Template -->
  <template id="cardTemplate">
    <div class="card">Product</div>
  </template>

  <script>
    const container = document.getElementById("cardContainer");
    const resultBox = document.getElementById("resultBox");
    const CARD_COUNT = 100;

    function clearContainer() {
      container.innerHTML = "";
    }

    function showResult(method, time) {
      resultBox.innerHTML = `${method} completed in <b>${time.toFixed(2)} ms</b>`;
    }

    function generateInnerHTML() {
      clearContainer();
      const start = performance.now();

      let html = "";
      for (let i = 1; i <= CARD_COUNT; i++) {
        html += `<div class="card">Product ${i}</div>`;
      }
      container.innerHTML = html;

      const end = performance.now();
      showResult("innerHTML", end - start);
    }

    function generateCreateElement() {
      clearContainer();
      const start = performance.now();

      for (let i = 1; i <= CARD_COUNT; i++) {
        const card = document.createElement("div");
        card.className = "card";
        card.textContent = `Product ${i}`;
        container.appendChild(card);
      }

      const end = performance.now();
      showResult("createElement + appendChild", end - start);
    }

    function generateFragment() {
      clearContainer();
      const start = performance.now();

      const fragment = document.createDocumentFragment();

      for (let i = 1; i <= CARD_COUNT; i++) {
        const card = document.createElement("div");
        card.className = "card";
        card.textContent = `Product ${i}`;
        fragment.appendChild(card);
      }

      container.appendChild(fragment);

      const end = performance.now();
      showResult("DocumentFragment", end - start);
    }

    function generateInsertAdjacent() {
      clearContainer();
      const start = performance.now();

      for (let i = 1; i <= CARD_COUNT; i++) {
        container.insertAdjacentHTML(
          "beforeend",
          `<div class="card">Product ${i}</div>`
        );
      }

      const end = performance.now();
      showResult("insertAdjacentHTML", end - start);
    }

    function generateTemplate() {
      clearContainer();
      const template = document.getElementById("cardTemplate");
      const start = performance.now();

      const fragment = document.createDocumentFragment();

      for (let i = 1; i <= CARD_COUNT; i++) {
        const clone = template.content.cloneNode(true);
        clone.querySelector(".card").textContent = `Product ${i}`;
        fragment.appendChild(clone);
      }

      container.appendChild(fragment);

      const end = performance.now();
      showResult("Template Cloning", end - start);
    }
  </script>

</body>
</html>
