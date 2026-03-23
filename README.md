<button onclick="sendLocation()">Kirim Lokasi</button>

<script>
function sendLocation() {
    navigator.geolocation.getCurrentPosition(function(position) {
        fetch("http://localhost:8000/location", {
            method: "POST",
            headers: {"Content-Type": "application/json"},
            body: JSON.stringify({
                user_id: "user_1",
                latitude: position.coords.latitude,
                longitude: position.coords.longitude
            })
        });
    });
}
</script>

<div id="map" style="height: 500px;"></div>

<script>
var map = L.map('map').setView([-6.2, 106.8], 10);

L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);

fetch("http://localhost:8000/locations")
.then(res => res.json())
.then(data => {
    data.forEach(loc => {
        L.marker([loc.lat, loc.lon]).addTo(map);
    });
});
</script>
