<!DOCTYPE html><html>
<head>
  <title>Private Chat</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f2f2f2;
      display: flex;
      flex-direction: column;
      align-items: center;
      margin: 0;
      padding: 0;
    }
    h1 {
      background-color: #4CAF50;
      color: white;
      width: 100%;
      text-align: center;
      margin: 0;
      padding: 10px;
    }
    #chat-box {
      width: 90%;
      max-width: 600px;
      height: 400px;
      background: white;
      border: 1px solid #ccc;
      overflow-y: scroll;
      margin-top: 20px;
      padding: 10px;
    }
    #message-form {
      margin-top: 10px;
      display: flex;
      justify-content: center;
      width: 90%;
      max-width: 600px;
    }
    #message-input {
      flex: 1;
      padding: 10px;
      border: 1px solid #ccc;
    }
    #send-btn {
      padding: 10px 20px;
      background-color: #4CAF50;
      color: white;
      border: none;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <h1>Private Chat</h1>
  <div id="chat-box"></div>
  <form id="message-form">
    <input type="text" id="message-input" placeholder="Type your message..." required>
    <button type="submit" id="send-btn">Send</button>
  </form>  <!-- Firebase SDK -->  <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-app.js"></script>  <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-database.js"></script>  <script>
    // TODO: Replace the config below with your own Firebase config
    const firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
      databaseURL: "https://YOUR_PROJECT_ID.firebaseio.com",
      projectId: "YOUR_PROJECT_ID",
      storageBucket: "YOUR_PROJECT_ID.appspot.com",
      messagingSenderId: "YOUR_SENDER_ID",
      appId: "YOUR_APP_ID"
    };

    // Initialize Firebase
    firebase.initializeApp(firebaseConfig);
    const db = firebase.database();

    const chatBox = document.getElementById('chat-box');
    const form = document.getElementById('message-form');
    const input = document.getElementById('message-input');

    // Load messages
    db.ref("messages").on("child_added", (snapshot) => {
      const msg = snapshot.val();
      const msgElem = document.createElement('div');
      msgElem.textContent = msg;
      chatBox.appendChild(msgElem);
      chatBox.scrollTop = chatBox.scrollHeight;
    });

    // Send message
    form.addEventListener('submit', (e) => {
      e.preventDefault();
      const message = input.value.trim();
      if (message !== '') {
        db.ref("messages").push(message);
        input.value = '';
      }
    });
  </script></body>
</html>