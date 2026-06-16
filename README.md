<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SevaHub PRO</title>

<style>
body{
  margin:0;
  font-family:Arial;
  background:#f4f6f9;
}

.navbar{
  display:flex;
  justify-content:space-between;
  padding:15px 25px;
  background:#222;
  color:white;
}

.hero{
  text-align:center;
  padding:30px;
  background:white;
}

input, select{
  padding:10px;
  margin:5px;
  width:220px;
}

.btn{
  background:#0d6efd;
  color:white;
  padding:10px 15px;
  border:none;
  border-radius:8px;
  cursor:pointer;
  margin-top:10px;
}

.cards{
  display:flex;
  flex-wrap:wrap;
  justify-content:center;
  gap:15px;
  padding:20px;
}

.card{
  background:white;
  padding:15px;
  width:250px;
  border-radius:10px;
  box-shadow:0 2px 8px rgba(0,0,0,0.1);
}

#requests{
  margin-top:20px;
}
</style>
</head>

<body>

<!-- Firebase -->
<script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-firestore-compat.js"></script>

<script>
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_AUTH_DOMAIN",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_STORAGE_BUCKET",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};

if (!firebase.apps.length) {
  firebase.initializeApp(firebaseConfig);
}

const db = firebase.firestore();

/* BOOKING */
function book(){

  let name = document.getElementById("name").value;
  let phone = document.getElementById("phone").value;
  let service = document.getElementById("service").value;
  let area = document.getElementById("area").value;

  if(!name || !phone || !service || !area){
    alert("Fill all fields");
    return;
  }

  db.collection("bookings").add({
    name,
    phone,
    service,
    area,
    time: new Date()
  }).then(()=>{

    alert("Booking Saved 🚀");

    // WhatsApp auto open
    window.open(
      `https://wa.me/919060512088?text=Booking%20Received%0AName:%20${name}%0APhone:%20${phone}%0AService:%20${service}%0AArea:%20${area}`
    );

  });

}

/* LIVE BOOKINGS */
function loadBookings(){

  const box = document.getElementById("requests");

  db.collection("bookings")
  .orderBy("time","desc")
  .onSnapshot(snapshot=>{

    box.innerHTML="";

    snapshot.forEach(doc=>{
      let r = doc.data();

      box.innerHTML += `
        <div class="card">
          <b>${r.name}</b><br>
          📞 ${r.phone}<br>
          🧹 ${r.service}<br>
          📍 ${r.area}
        </div>
      `;
    });

  });

}

window.onload = loadBookings;
</script>

<!-- NAVBAR -->
<div class="navbar">
  <div><b>SevaHub PRO</b></div>
</div>

<!-- HERO -->
<div class="hero">

<h2>Book Trusted Local Services</h2>

<input id="name" placeholder="Name"><br>
<input id="phone" placeholder="Phone"><br>

<select id="service">
  <option>Head Massage</option>
  <option>Bathroom Cleaning</option>
  <option>Teacher</option>
</select><br>

<input id="area" placeholder="Area"><br>

<button class="btn" onclick="book()">Book Now</button>

</div>

<!-- LIVE BOOKINGS -->
<div class="cards" id="requests"></div>

<!-- CONTACT -->
<div class="hero">
  <h3>Contact Support</h3>

  <a class="btn"
  href="https://wa.me/919060512088?text=Help%20needed%20for%20SevaHub">
  WhatsApp
  </a>

  <a class="btn"
  href="mailto:adiraj200987@gmail.com">
  Email
  </a>
</div>

</body>
</html>
