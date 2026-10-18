# Live-Simulator-
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Orbital Velocity Simulation Sandbox</title>
    <style>
        :root {
            --bg-color: #0b0f19;
            --card-bg: #131a26;
            --accent-blue: #00d2ff;
            --accent-geo: #ff9f43;
            --text-main: #f0f4f8;
            --text-muted: #8da2bb;
            --border-color: #222f44;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        .container {
            width: 100%;
            max-width: 1000px;
            background: var(--card-bg);
            border: 1px solid var(--border-color);
            border-radius: 12px;
            padding: 24px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.5);
        }

        header {
            margin-bottom: 20px;
            border-bottom: 1px solid var(--border-color);
            padding-bottom: 15px;
        }

        h1 {
            font-size: 1.5rem;
            color: #ffffff;
            margin-bottom: 5px;
        }

        p.subtitle {
            color: var(--text-muted);
            font-size: 0.9rem;
        }

        .app-layout {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 24px;
        }

        @media (max-width: 768px) {
            .app-layout {
                grid-template-columns: 1fr;
            }
        }

        .controls-panel {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .control-group {
            background: rgba(255, 255, 255, 0.02);
            padding: 16px;
            border-radius: 8px;
            border: 1px solid var(--border-color);
        }

        label {
            display: block;
            font-size: 0.9rem;
            color: var(--text-main);
            margin-bottom: 10px;
            font-weight: 500;
        }

        .slider-wrapper {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        input[type="range"] {
            flex: 1;
            accent-color: var(--accent-blue);
            cursor: pointer;
        }

        .value-display {
            font-family: monospace;
            font-size: 1rem;
            font-weight: bold;
            color: var(--accent-blue);
            min-width: 90px;
            text-align: right;
        }

        .preset-buttons {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-top: 10px;
        }

        button.preset-btn {
            background: #1b263b;
            border: 1px solid var(--border-color);
            color: var(--text-main);
            padding: 10px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 0.85rem;
            transition: all 0.2s ease;
        }

        button.preset-btn:hover {
            background: #2c3e50;
            border-color: var(--accent-blue);
        }

        .telemetry-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 12px;
        }

        .telemetry-card {
            background: rgba(0, 0, 0, 0.2);
            border: 1px solid var(--border-color);
            padding: 12px;
            border-radius: 6px;
            text-align: center;
        }

        .telemetry-label {
            font-size: 0.75rem;
            color: var(--text-muted);
            text-transform: uppercase;
            letter-spacing: 0.5px;
            margin-bottom: 4px;
        }

        .telemetry-value {
            font-size: 1.2rem;
            font-weight: bold;
            font-family: monospace;
        }

        .v-leo { color: var(--accent-blue); }
        .v-geo { color: var(--accent-geo); }

        .formula-card {
            font-family: "Times New Roman", Times, serif;
            font-style: italic;
            text-align: center;
            font-size: 1.2rem;
            background: rgba(0, 210, 255, 0.05);
            border: 1px dashed var(--accent-blue);
            padding: 10px;
            border-radius: 6px;
        }

        .canvas-container {
            display: flex;
            justify-content: center;
            align-items: center;
            background: #05070c;
            border-radius: 8px;
            border: 1px solid var(--border-color);
            position: relative;
            aspect-ratio: 1;
        }

        canvas {
            width: 100%;
            height: 100%;
            border-radius: 8px;
        }

        .legend {
            position: absolute;
            bottom: 10px;
            left: 10px;
            font-size: 0.75rem;
            background: rgba(11, 15, 25, 0.8);
            padding: 8px;
            border-radius: 4px;
            border: 1px solid var(--border-color);
            display: flex;
            flex-direction: column;
            gap: 4px;
        }

        .legend-item {
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
        }
    </style>
</head>
<body>

<div class="container">
    <header>
        <h1>Orbital Velocity Physics Simulator</h1>
        <p class="subtitle">Live interactive calculations for Katie Carlson's Orbital Mechanics Lecture</p>
    </header>

    <div class="app-layout">
        <!-- Controls Side -->
        <div class="controls-panel">
            <div class="formula-card">
                v = √(GM / r)
            </div>

            <div class="control-group">
                <label for="altitudeRange">Orbital Altitude (Above Earth Surface)</label>
                <div class="slider-wrapper">
                    <input type="range" id="altitudeRange" min="200" max="36000" step="50" value="400">
                    <div class="value-display" id="altitudeValue">400 km</div>
                </div>
                
                <div class="preset-buttons">
                    <button class="preset-btn" onclick="setPreset(400)">Low Earth Orbit (ISS)</button>
                    <button class="preset-btn" onclick="setPreset(35786)">Geostationary (GEO)</button>
                </div>
            </div>

            <div class="telemetry-grid">
                <div class="telemetry-card">
                    <div class="telemetry-label">Orbital Velocity</div>
                    <div class="telemetry-value v-leo" id="velocityMetric">7.67 km/s</div>
                </div>
                <div class="telemetry-card">
                    <div class="telemetry-label">Velocity (Imperial)</div>
                    <div class="telemetry-value v-leo" id="velocityImperial">17,158 mph</div>
                </div>
                <div class="telemetry-card">
                    <div class="telemetry-label">Total Radius (r)</div>
                    <div class="telemetry-value" id="totalRadius">6,771 km</div>
                </div>
                <div class="telemetry-card">
                    <div class="telemetry-label">Orbital Period</div>
                    <div class="telemetry-value" id="orbitalPeriod">92.6 min</div>
                </div>
            </div>
        </div>

        <!-- Simulation Canvas Side -->
        <div class="canvas-container">
            <canvas id="orbitCanvas" width="500" height="500"></canvas>
            <div class="legend">
                <div class="legend-item"><span class="dot" style="background: #3a86ff;"></span> Earth (R = 6,371 km)</div>
                <div class="legend-item"><span class="dot" style="background: var(--accent-blue);"></span> Current Spacecraft Path</div>
                <div class="legend-item"><span class="dot" style="background: var(--accent-geo);"></span> Geostationary Track Boundary</div>
            </div>
        </div>
    </div>
</div>

<script>
    const canvas = document.getElementById('orbitCanvas');
    const ctx = canvas.getContext('2d');
    const altitudeSlider = document.getElementById('altitudeRange');
    const altitudeValue = document.getElementById('altitudeValue');
    
    // Physics constants
    const GM = 3.986004418e5; // Earth's Standard Gravitational Parameter (km^3/s^2)
    const R_EARTH = 6371;      // Earth radius in km
    
    let currentAltitude = parseInt(altitudeSlider.value);
    let angle = 0;

    function calculateOrbit(alt) {
        const r = R_EARTH + alt; // Total radius from center
        const v = Math.sqrt(GM / r); // Orbital velocity in km/s
        const vMph = v * 2236.94; // Convert to mph
        const periodSec = 2 * Math.PI * r / v;
        const periodMin = periodSec / 60;

        return {
            radius: r,
            velocity: v,
            velocityMph: vMph,
            period: periodMin
        };
    }

    function updateTelemetry() {
        const data = calculateOrbit(currentAltitude);
        
        altitudeValue.textContent = `${currentAltitude.toLocaleString()} km`;
        document.getElementById('velocityMetric').textContent = `${data.velocity.toFixed(2)} km/s`;
        document.getElementById('velocityImperial').textContent = `${Math.round(data.velocityMph).toLocaleString()} mph`;
        document.getElementById('totalRadius').textContent = `${Math.round(data.radius).toLocaleString()} km`;
        
        if (data.period > 1440) {
