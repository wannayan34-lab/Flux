User Action → Action → Dispatcher → Store → Component → UI update
<!DOCTYPE html>
<html>
<head>
  <title>Flux Example</title>
</head>
<body>
  <h1 id="counter">0</h1>
  <button id="addBtn">Add 1</button>

  <script>
    // 1. Store
    const Store = {
      value: 0,
      listeners: [],
      subscribe: function(listener) { this.listeners.push(listener); },
      update: function(newValue) {
        this.value = newValue;
        this.listeners.forEach(l => l()); // notify components
      }
    };

    // 2. Component (UI)
    function updateUI() {
      document.getElementById('counter').innerText = Store.value;
    }

    Store.subscribe(updateUI);

    // 3. Action
    function increment() {
      const newValue = Store.value + 1;
      Dispatcher.dispatch({ type: "INCREMENT", payload: newValue });
    }

    // 4. Dispatcher
    const Dispatcher = {
      dispatch: function(action) {
        if(action.type === "INCREMENT") {
          Store.update(action.payload);
        }
      }
    };

    // 5. Button click
    document.getElementById('addBtn').addEventListener('click', increment);
  </script>
</body>
</html>
