<!DOCTYPE html>
<html>
<head>
    <title>Massachusetts Weather Alerts</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 10px;
        }

        .alert {
            border-left: 5px solid red;
            background: #fff3f3;
            padding: 10px;
            margin-bottom: 10px;
        }

        .event {
            font-weight: bold;
            font-size: 18px;
        }

        .headline {
            color: #444;
        }
    </style>
</head>
<body>

<h2>Active Weather Alerts</h2>
<div id="alerts">Loading alerts...</div>

<script>
fetch("https://api.weather.gov/alerts/active?area=MA")
    .then(response => response.json())
    .then(data => {
        const container = document.getElementById("alerts");

        if (data.features.length === 0) {
            container.innerHTML =
                "<p>No active watches, warnings, or advisories in Massachusetts.</p>";
            return;
        }

        container.innerHTML = "";

        data.features.forEach(alert => {
            const props = alert.properties;

            const div = document.createElement("div");
            div.className = "alert";

            div.innerHTML = `
                <div class="event">${props.event}</div>
                <div class="headline">${props.headline}</div>
            `;

            container.appendChild(div);
        });
    })
    .catch(error => {
        document.getElementById("alerts").innerHTML =
            "Unable to load alerts.";
    });
</script>

</body>
</html>
