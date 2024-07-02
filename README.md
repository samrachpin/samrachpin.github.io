
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CCU BANK QUEUE SAMPLE</title>
<style>
  body { font-family: Arial, sans-serif; }
  .queue-list { list-style-type: none; padding: 0; }
  .queue-item { background-color: #f2f2f2; margin: 5px 0; padding: 10px; }
  .instruction { font-weight: bold; margin-bottom: 20px; }
  .red { color: red; } /* Class to make text red */
  h2 {
    font-family: Arial, sans-serif; /* Set the font to Arial */
    color: blue; /* Set the text color to blue */
</style>
</head>
<body>
<h2>Welcome to CCU BANK</h2>
<div id="instruction" class="instruction"></div>
<ul id="queueList" class="queue-list">
  <!-- Queue numbers will be listed here -->
</ul>

<script>
  // Queue numbers and their corresponding counters
  let queueNumbers = ['A123', 'A124', 'B567', 'B568', 'B569', 'C788', 'C789'];
  const counters = [3, 5, 1, 7, 4, 2, 6];
  let servedNumbers = [];
  let servedCounters = [];

  function updateQueueList() {
    const queueListElement = document.getElementById('queueList');
    queueListElement.innerHTML = ''; // Clear the current list
    queueNumbers.forEach((number, index) => {
      const listItem = document.createElement('li');
      listItem.className = 'queue-item';
      listItem.textContent = `${index + 1}. ${number}`;
      queueListElement.appendChild(listItem);
    });
  }

  function updateDisplay() {
    const instructionElement = document.getElementById('instruction');
    if (queueNumbers.length > 0) {
      const currentNumber = queueNumbers.shift(); // Remove the first number from the queue
      const currentCounter = counters[0]; // Get the first counter from the list
      instructionElement.innerHTML = `Customer <span class="red">${currentNumber}</span> --> Counter <span class="red">${currentCounter}</span>.`;
      servedNumbers.push(currentNumber); // Add the served number to the servedNumbers list
      servedCounters.push(counters.shift()); // Remove the first counter and add to servedCounters list
      updateQueueList(); // Update the queue list display
      setTimeout(() => {
        queueNumbers.push(servedNumbers.shift()); // Re-add the served number to the queue
        counters.push(servedCounters.shift()); // Re-add the served counter to the counters list
        updateQueueList(); // Update the queue list display
      }, 31000); // Re-add the number to the queue after 31 seconds
    } else {
      // Reset the queue numbers and counters when all have been directed
      queueNumbers = [...servedNumbers]; // Re-add served numbers to the queue
      servedNumbers = []; // Clear the served numbers list
      updateQueueList();
      instructionElement.textContent = '';
    }
  }

  // Initialize the queue list display
  updateQueueList();

  // Update the display every 30 seconds
  setInterval(updateDisplay, 30000);
</script>
</body>
</html>
