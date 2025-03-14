# DevSecOps Pipeline Implementation for simple game app

![pipeline](/images/Pipelineimg.png)

![game](/images/game.png)

## Features

- 🎮 Fully functional Tic Tac Toe game
- 📊 Score tracking for X, O, and draws
- 📜 Game history with timestamps
- 🏆 Highlights winning combinations
- 🔄 Reset game and statistics
- 📱 Responsive design for all devices

## Technologies Used

- React 18
- TypeScript
- Tailwind CSS
- Lucide React for icons

## Project Structure

```
src/
├── components/
│   ├── Board.tsx       # Game board component
│   ├── Square.tsx      # Individual square component
│   ├── ScoreBoard.tsx  # Score tracking component
│   └── GameHistory.tsx # Game history component
├── utils/
│   └── gameLogic.ts    # Game logic utilities
├── App.tsx             # Main application component
└── main.tsx           # Entry point
```

## Game Logic

The game implements the following rules:

1. X goes first, followed by O
2. The first player to get 3 of their marks in a row (horizontally, vertically, or diagonally) wins
3. If all 9 squares are filled and no player has 3 marks in a row, the game is a draw
4. Winning combinations are highlighted
5. Game statistics are tracked and displayed

## Getting Started

### Prerequisites

- Node.js (v14 or higher)
- npm or yarn

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/devsecops-demo.git
   cd devsecops-demo
   ```

2. Install dependencies:
   ```bash
   npm install
   # or
   yarn
   ```

3. Start the development server:
   ```bash
   npm run dev
   # or
   yarn dev
   ```

4. Open your browser and navigate to `http://localhost:5173`

## Building for Production

To create a production build:

```bash
npm run build
# or
yarn build
```

The build artifacts will be stored in the `dist/` directory.

---
### **Pipeline Breakdown:**


### **1. Code Commit & Trigger**

- The pipeline starts when a **push** or **pull request** is made to the `main` branch.
- Any changes to `kubernetes/deployment.yaml` are ignored to prevent recursive deployments.

---

### **2. Unit Testing**

- Runs on `ubuntu-latest`.
- Steps:
    1. Checkout code from GitHub.
    2. Setup Node.js (`v20`).
    3. Install dependencies (`npm ci`).
    4. Run unit tests (`npm test`).

---

### **3. Static Code Analysis (Linting)**

- Runs `ESLint` to check for syntax errors and enforce coding standards.
- Steps:
    1. Checkout code.
    2. Setup Node.js (`v20`).
    3. Install dependencies.
    4. Run `npm run lint`.

---

### **4. Build**

- The application is compiled.
- Steps:
    1. Checkout code.
    2. Setup Node.js.
    3. Install dependencies.
    4. Build the project (`npm run build`).
    5. Upload the build artifacts (`dist/` directory).

---

### **5. Docker Image Build & Push**

- Steps:
    1. Checkout code.
    2. Download build artifacts.
    3. Setup Docker Buildx.
    4. Login to GitHub Container Registry (`ghcr.io`).
    5. Extract Docker metadata for tagging.
    6. **Build the Docker image**.
    7. **Security Scan using Trivy** for vulnerabilities.
    8. If no critical issues are found, push the image to the container registry.
    9. Output the image tag.

---

### **6. Kubernetes Deployment Update**

- Runs only on `main` branch **after the Docker build**.
- Steps:
    1. Checkout the repository.
    2. Setup Git config.
    3. Update the Kubernetes `deployment.yaml` file with the new **Docker image tag**.
    4. Commit and push changes back to GitHub.

---

### **7. ArgoCD Deployment**

- **ArgoCD** monitors the `deployment.yaml` file in the repository.
- Once the file is updated, **ArgoCD automatically deploys the new version** of the application to Kubernetes.

---

### **Security Measures taken:**

✔ **Static Code Analysis** – Linting prevents code quality issues.

✔ **Unit Testing** – Ensures application logic is correct.

✔ **Container Security Scanning** – Trivy scans Docker images for vulnerabilities.

✔ **Automated Deployment** – ArgoCD ensures safe and controlled deployments.

This pipeline ensures **secure, automated, and efficient deployment** of applications in a Kubernetes environment.
