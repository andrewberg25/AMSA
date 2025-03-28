# AMSA
Test for AMSA Fun
function createInteractiveMultiplicationTable(imageSrc) {
  if (typeof window === 'undefined') {
    return "This function is designed to run in a browser environment.";
  }

  const table = document.createElement("table");
  table.style.borderCollapse = "collapse";
  table.style.margin = "20px";

  const headerRow = document.createElement("tr");
  const emptyHeader = document.createElement("th");
  headerRow.appendChild(emptyHeader);

  for (let i = 1; i <= 10; i++) {
    const header = document.createElement("th");
    header.textContent = i;
    header.style.border = "1px solid black";
    header.style.padding = "8px";
    header.style.textAlign = "center";
    header.style.width = "40px";
    header.style.height = "40px";
    header.style.backgroundColor = "#f0f0f0";
    headerRow.appendChild(header);
  }
  table.appendChild(headerRow);

  for (let i = 1; i <= 10; i++) {
    const row = document.createElement("tr");
    const rowHeader = document.createElement("th");
    rowHeader.textContent = i;
    rowHeader.style.border = "1px solid black";
    rowHeader.style.padding = "8px";
    rowHeader.style.textAlign = "center";
    rowHeader.style.width = "40px";
    rowHeader.style.height = "40px";
    rowHeader.style.backgroundColor = "#f0f0f0";
    row.appendChild(rowHeader);

    for (let j = 1; j <= 10; j++) {
      const cell = document.createElement("td");
      cell.textContent = i * j;
      cell.style.border = "1px solid black";
      cell.style.padding = "8px";
      cell.style.textAlign = "center";
      cell.style.width = "40px";
      cell.style.height = "40px";
      cell.dataset.row = i;
      cell.dataset.col = j;
      cell.addEventListener("click", function() {
        selectedCells.push(this);
        highlightSelection();
      });
      row.appendChild(cell);
    }
    table.appendChild(row);
  }

  // Selection Logic
  let selectedRow = null;
  let selectedCol = null;
  let selectedCells = [];

  function highlightSelection() {
    const cells = table.querySelectorAll("td");
    cells.forEach(cell => {
      cell.style.backgroundColor = "";
      cell.innerHTML = cell.dataset.row * cell.dataset.col;

      if (selectedRow && cell.dataset.row == selectedRow) {
        cell.style.backgroundColor = "lightblue";
      }
      if (selectedCol && cell.dataset.col == selectedCol) {
        cell.style.backgroundColor = "lightblue";
      }
      if (selectedRow && selectedCol && cell.dataset.row == selectedRow && cell.dataset.col == selectedCol) {
        cell.innerHTML = `<img src="${imageSrc}" style="width: 40px; height: 40px;">`;
      }
    });

    selectedCells.forEach(cell => {
      cell.style.backgroundColor = "yellow";
    });
  }

  const headerCells = table.querySelectorAll("th");
  headerCells.forEach(header => {
    header.addEventListener("click", function() {
      const value = parseInt(this.textContent);
      if (this.parentNode.rowIndex === 0) {
        selectedCol = value;
      } else {
        selectedRow = value;
      }
      highlightSelection();
    });
  });

  // Clear Button
  const clearButton = document.createElement("button");
  clearButton.textContent = "Clear";
  clearButton.addEventListener("click", function() {
    selectedRow = null;
    selectedCol = null;
    selectedCells = [];
    highlightSelection();
  });

  const container = document.createElement("div");
  container.appendChild(table);
  container.appendChild(clearButton);

  return container;
}

// Example usage (in a browser environment):
if (typeof window !== 'undefined') {
  const imageToUse = "https://deckard.openprocessing.org/user515774/visual2595433/hab3b7741b607c2cd79f0135a1a23ccf6/Screenshot%202025-03-28%20at%201.59.35%E2%80%AFPM.png";
  const myTable = createInteractiveMultiplicationTable(imageToUse);
  document.body.appendChild(myTable);
}
