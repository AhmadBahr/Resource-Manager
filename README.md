# Resource Manager - System Monitor

A modern desktop application for real-time system resource monitoring built with Electron and React.

![Electron](https://img.shields.io/badge/Electron-191970?style=for-the-badge&logo=electron&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)
![Recharts](https://img.shields.io/badge/Recharts-FF6B6B?style=for-the-badge&logo=recharts&logoColor=white)
![Playwright](https://img.shields.io/badge/Playwright-2EAD96?style=for-the-badge&logo=playwright&logoColor=white)

## ✨ Features

- 📊 **Real-Time Monitoring** - Live CPU, RAM, and storage usage tracking
- 📈 **Interactive Charts** - Beautiful visualizations with Recharts
- 🖥️ **System Tray** - Minimize to system tray for background monitoring
- 🎨 **Modern UI** - Clean, responsive interface with Tailwind CSS
- 🔧 **Cross-Platform** - Works on Windows, macOS, and Linux
- 🧪 **Comprehensive Testing** - Unit tests with Vitest and E2E with Playwright
- ⚡ **Fast Development** - Hot reload with Vite and TypeScript

## 🚀 Quick Start

### Prerequisites
- Node.js (v18+)
- npm or yarn

### Installation
```bash
# Clone the repository
git clone <repository-url>
cd Resource-Manager

# Install dependencies
npm install

# Start development mode
npm run dev
```

The app will open automatically in development mode.

### Building for Production
```bash
# Build for your platform
npm run dist:win    # Windows
npm run dist:mac    # macOS
npm run dist:linux  # Linux
```

## 🛠️ Tech Stack

**Frontend:**
- React 18 + TypeScript
- Vite for fast development
- Tailwind CSS for styling
- Recharts for data visualization

**Desktop:**
- Electron 32
- Node.js system APIs
- os-utils for resource monitoring

**Testing:**
- Vitest for unit testing
- Playwright for E2E testing

**Build Tools:**
- TypeScript for type safety
- Electron Builder for packaging
- npm-run-all for script orchestration

## 📁 Project Structure

```
Resource-Manager/
├── src/
│   ├── electron/           # Electron main process
│   │   ├── main.ts        # App entry point
│   │   ├── resourceManager.ts  # System monitoring
│   │   ├── tray.ts        # System tray functionality
│   │   └── menu.ts        # Application menu
│   └── ui/                # React frontend
│       ├── App.tsx        # Main React component
│       ├── Chart.tsx      # Chart components
│       └── useStatistics.ts  # Data hooks
├── e2e/                   # End-to-end tests
├── dist/                  # Built application
└── package.json          # Dependencies and scripts
```

## 🔧 Configuration

### Development Environment
The app automatically detects development mode and loads from `http://localhost:5123`.

### Production Build
Update `electron-builder.json` for custom build configurations:
```json
{
  "appId": "com.resourcemanager.app",
  "productName": "Resource Manager",
  "directories": {
    "output": "dist"
  }
}
```

## 📊 System Monitoring

The app monitors the following system resources:

**CPU Usage:**
- Real-time CPU utilization percentage
- Historical data visualization

**Memory (RAM):**
- Total and used memory
- Memory usage percentage

**Storage:**
- Disk space usage
- Available storage monitoring

**GPU Information:**
- GPU model detection
- Temperature monitoring (when available)

## 🧪 Testing

**Unit Tests:**
```bash
npm run test:unit
```

**E2E Tests:**
```bash
npm run test:e2e
```

**All Tests:**
```bash
npm run test:unit && npm run test:e2e
```

## 🚀 Deployment

**Development:**
```bash
npm run dev
```

**Production Build:**
```bash
# For Windows
npm run dist:win

# For macOS
npm run dist:mac

# For Linux
npm run dist:linux
```

The built applications will be available in the `dist` directory.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

MIT License - see [LICENSE](LICENSE) for details.

---

**Built with ❤️ using Electron and React**


