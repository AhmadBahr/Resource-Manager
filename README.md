# Resource Manager Electron App

## Table of Contents
1. [**General Info**](#general-info)
2. [**Technologies Used**](#technologies-used)
3. [**Setup**](#setup)
4. [**Features**](#features) 
5. [**Code & Snippets**](#codesnippets)

## General Info
The Resource Manager Electron App provides real-time system performance insights, displaying CPU, GPU, and RAM usage in a user-friendly interface. The app uses Electron's power to interact with system resources and React to render the UI dynamically.

# Technologies Used:

## Frontend Development:

React.js: Creates reusable UI components for a seamless experience.

Tailwind CSS: Adds responsive and modern design elements.

Chart.js: Renders performance metrics in visually appealing charts.

React Icons: Supplies icons for visual clarity.

 ## Backend:
 
Electron.js: Core framework to build cross-platform desktop applications.

Node.js: Provides runtime for fetching system resource data.

Systeminformation (NPM Package): Gathers real-time hardware and OS performance data.

# Setup

## Development Environment

Clone the repository:

git clone https://github.com/your-username/resource-manager.git

Navigate to the project directory:

cd resource-manager

Install dependencies (if applicable):

npm install

## Packaging

To build the app as an executable:

This will generate the distributable file in the dist directory.

## Features

Real-Time CPU Usage Monitoring:

Displays current CPU load percentage.
Graphical representation of usage trends.
RAM Usage Tracking:

Shows total, used, and free memory in a detailed bar chart.
GPU Information:

Tracks GPU load, temperature, and memory usage (if supported by the system).
Customizable UI:

Users can toggle between dark and light themes.
Options to adjust update intervals.

# Code & Snippets: 

## Example:  Main Process in Electron:

This snippet demonstrates The main process manages system-level functionality:

const { app, BrowserWindow, ipcMain } = require("electron");
const si = require("systeminformation");

let mainWindow;

app.on("ready", () => {
  mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      nodeIntegration: true,
      contextIsolation: false,
    },
  });

  mainWindow.loadURL("http://localhost:3000"); // Pointing to React's development server.
});

// Handle system info requests
ipcMain.on("get-system-info", async (event) => {
  const cpu = await si.currentLoad();
  const ram = await si.mem();
  const gpu = await si.graphics();

  event.reply("system-info", { cpu, ram, gpu });
});

## Example: Example: React Frontend (Display Data):

This snippet listens for system info updates and displays them.

import React, { useEffect, useState } from "react";
import { ipcRenderer } from "electron";
import { Line } from "react-chartjs-2";

const App = () => {
  const [cpuLoad, setCpuLoad] = useState([]);
  const [ramUsage, setRamUsage] = useState({});
  const [gpuInfo, setGpuInfo] = useState({});

  useEffect(() => {
    const fetchData = () => {
      ipcRenderer.send("get-system-info");
    };

    ipcRenderer.on("system-info", (event, data) => {
      const { cpu, ram, gpu } = data;
      setCpuLoad((prev) => [...prev, cpu.currentLoad].slice(-10));
      setRamUsage({
        total: (ram.total / 1e9).toFixed(2),
        used: (ram.active / 1e9).toFixed(2),
      });
      setGpuInfo(gpu.controllers[0]);
    });

    fetchData();
    const interval = setInterval(fetchData, 1000);

    return () => {
      clearInterval(interval);
      ipcRenderer.removeAllListeners("system-info");
    };
  }, []);

  return (
    <div className="p-4">
      <h1 className="text-xl font-bold">Resource Manager</h1>

      <div className="mt-4">
        <h2>CPU Usage</h2>
        <Line
          data={{
            labels: Array.from({ length: 10 }, (_, i) => `-${10 - i}s`),
            datasets: [
              {
                label: "CPU Load (%)",
                data: cpuLoad,
                borderColor: "#4caf50",
                fill: false,
              },
            ],
          }}
        />
      </div>

      <div className="mt-4">
        <h2>RAM Usage</h2>
        <p>
          Total: {ramUsage.total} GB, Used: {ramUsage.used} GB
        </p>
      </div>

      <div className="mt-4">
        <h2>GPU Information</h2>
        {gpuInfo && (
          <p>
            Model: {gpuInfo.model}, Load: {gpuInfo.load || 0}%, Temp:{" "}
            {gpuInfo.temperatureGpu || "N/A"}°C
          </p>
        )}
      </div>
    </div>
  );
};

export default App;
