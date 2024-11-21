# Resource Manager Electron App

## Table of Contents
1. [**General Info**](#general-info)
2. [**Technologies Used**](#technologies-used)
3. [**Setup**](#setup)
4. [**Features**](#features) 
5. [**Code & Snippets**](#codesnippets)

## General Info
The Resource Manager Electron App monitors system resources (CPU, RAM, GPU) in real-time using modern tools and frameworks. The app offers comprehensive support for development, testing, and deployment, ensuring a seamless experience for developers and end-users alike.

# Technologies Used:

## Core Frameworks:

Electron.js: Builds cross-platform desktop apps.

React.js + React DOM: Frontend framework for creating interactive UI components.

Vite: Ultra-fast build tool for React with first-class TypeScript support.

 ## Styling:
 
Tailwind CSS: Provides a modern and responsive design system.

## Data Visualization:

Recharts: Renders dynamic charts for visualizing system metrics.

## Utilities:

Systeminformation + os-utils: Collects detailed system information (CPU, GPU, RAM, etc.).

cross-env: Simplifies setting environment variables across platforms.

## Testing:

Cypress: E2E testing for app flows.

Playwright: Cross-browser automated testing.

Vitest: Unit testing framework optimized for Vite.

## Build and Packaging:

TypeScript: Adds static typing for robust development.

Electron Forge: Simplifies packaging and distribution of Electron apps.

## Development Tools:

npm-run-all: Orchestrates multiple NPM scripts.

Axios: Handles HTTP requests efficiently.

# Setup

## Development Environment

Clone the repository:

git clone https://github.com/your-username/resource-manager.git

Navigate to the project directory:

cd resource-manager

Install dependencies (if applicable):

npm install

## Run Development Mode

Transpile TypeScript and start the app:

npm run dev

Build the app for production:

npm run build

This will generate the distributable file in the dist directory.

## Features

Real-Time System Monitoring:

Displays CPU, RAM, and GPU usage dynamically.

Uses Recharts for interactive visualizations.

2. Testing and Automation:
   
E2E tests using Cypress and Playwright.

Unit tests with Vitest.

4. TypeScript Integration:
   
Provides static typing across the codebase for reliability.

6. Fast Development:
   
Vite for rapid builds and hot module replacement (HMR).

# Code & Snippets: 

## Example:React Frontend (TypeScript + Vite)

This React component fetches and visualizes resource data.

import React, { useState, useEffect } from "react";
import { ipcRenderer } from "electron";
import { LineChart, Line, CartesianGrid, XAxis, YAxis, Tooltip } from "recharts";

interface SystemInfo {
  cpuUsage: number;
  ram: {
    total: number;
    active: number;
  };
  gpu: {
    controllers: { model: string; temperatureGpu?: number }[];
  };
}

const App: React.FC = () => {
  const [data, setData] = useState<SystemInfo | null>(null);
  const [cpuHistory, setCpuHistory] = useState<number[]>([]);

  useEffect(() => {
    const fetchData = () => {
      ipcRenderer.send("get-system-info");
    };

    ipcRenderer.on("system-info", (_, systemData: SystemInfo) => {
      setData(systemData);
      setCpuHistory((prev) => [...prev, systemData.cpuUsage].slice(-10));
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

      <section className="mt-4">
        <h2>CPU Usage</h2>
        <LineChart width={600} height={300} data={cpuHistory.map((value, index) => ({ index, value }))}>
          <Line type="monotone" dataKey="value" stroke="#8884d8" />
          <CartesianGrid stroke="#ccc" />
          <XAxis dataKey="index" />
          <YAxis />
          <Tooltip />
        </LineChart>
      </section>

      {data && (
        <section className="mt-4">
          <h2>RAM Usage</h2>
          <p>Total: {(data.ram.total / 1e9).toFixed(2)} GB</p>
          <p>Active: {(data.ram.active / 1e9).toFixed(2)} GB</p>

          <h2>GPU Information</h2>
          <p>
            Model: {data.gpu.controllers[0]?.model || "N/A"}, Temp:{" "}
            {data.gpu.controllers[0]?.temperatureGpu || "N/A"}°C
          </p>
        </section>
      )}
    </div>
  );
};

export default App;

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

## Package.json Scripts

Add these scripts for running and testing:

"scripts": {
  "dev": "cross-env NODE_ENV=development vite",
  "build": "electron-forge package && vite build",
  "test": "vitest run",
  "test:playwright": "playwright test",
  "test:cypress": "cypress open",
  "start": "electron-forge start",
  "all": "npm-run-all --parallel dev test"
}

## Testing Configuration:

# Cypress:

Initialize with npx cypress open.

# Playwright:

Set up Playwright with npx playwright install.

# Vitest:

Add this configuration in vite.config.ts:

import { defineConfig } from "vite";

export default defineConfig({
  test: {
    globals: true,
    environment: "jsdom",
  },
});


