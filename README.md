# 🎬 clip.studio

<p align="center">
  <img src="public/logo.png" alt="clip.studio logo" width="100" height="100" style="border-radius: 20px;" />
</p>

<p align="center">
  <strong>Open-Source AI Viral Shorts & Subtitle Studio</strong><br />
  Transform long-form YouTube videos into high-retention vertical Shorts (35–40s complete scenes) with frame-synchronized Alex Hormozi animated captions, intelligent scene boundary detection, and intelligent AI-powered curation for maximum virality across TikTok, Instagram Reels, and YouTube Shorts.
</p>

---

## 🌟 Key Features

- **🧠 Complete Scene & Narrative Extraction**: Extracts complete, standalone 35–40s conversational arcs (full jokes with setups & punchlines, complete educational lessons, and full story scenes).
- **💬 Frame-Synced Animated Subtitles**:
  - **Alex Hormozi Style**: Dynamic word-by-word active yellow highlights with thick outlines.
  - **Minimalist**: Clean, sleek subtitle lower-third box.
  - **Classic**: Traditional centered bordered captions.
  - **Clean Video Mode**: Option to render pure, cropped video with zero on-screen captions.
- **🌐 Multilingual & Hinglish Translation**: Automatically transcribes Hindi audio into Romanized **Hinglish** (e.g. *"Namaste Dosto"*) or preserves original Devanagari script.
- **⚡ Multi-Model AI Engine**: Support for **Groq** (ultra-fast LLaMA & GPT-OSS), **Mistral AI**, **Google Gemini**, and **OpenAI**.
- **🔒 Local-First Privacy**: All API keys, MongoDB connection strings, and binary paths are stored directly in your browser's `localStorage`. No credentials are leaked or saved on the server.
- **📐 Dual Aspect Ratio Export**: Generate **9:16 Vertical** Shorts/Reels/TikToks (with Left, Center, or Right framing focus) or **16:9 Horizontal** widescreen videos.
- **🚀 YouTube Shorts Direct Publishing & Scheduling**: Connect your YouTube channel via Google OAuth2 to publish or schedule viral Shorts directly from the platform with auto-generated titles, descriptions, and tags.

---

## 📋 Table of Contents

1. [Prerequisites](#-1-prerequisites)
2. [Installation](#-2-installation)
3. [Installing Video Binaries & Finding Paths (FFmpeg & yt-dlp)](#-3-installing-video-binaries-ffmpeg--yt-dlp)
4. [Setting Up MongoDB](#-4-setting-up-mongodb)
5. [Getting AI API Keys](#-5-getting-ai-api-keys)
6. [Configuring YouTube Shorts Uploads (Google OAuth)](#-6-configuring-youtube-shorts-uploads-google-oauth)
7. [Running the Application](#-7-running-the-application)
8. [In-App Settings & Diagnostic Testing](#-8-in-app-settings--diagnostic-testing)
9. [Troubleshooting & FAQ](#-9-troubleshooting--faq)

---

## 💻 1. Prerequisites

Before installing, make sure you have the following installed on your machine:

- **Node.js**: `v18.17.0` or later (Node.js 20+ recommended). [Download Node.js](https://nodejs.org/)
- **npm**, **yarn**, **pnpm**, or **bun** package manager.
- **Git**: [Download Git](https://git-scm.com/)

---

## 🚀 2. Installation

Clone the repository and install project dependencies:

```bash
# 1. Clone the repository
git clone https://github.com/nittinbhagaaat/shorts.git

# 2. Navigate into the project folder
cd shorts

# 3. Install dependencies
npm install
```

---

## 🛠️ 3. Installing Video Binaries (FFmpeg & yt-dlp)

`clip.studio` relies on two command-line tools:
1. **FFmpeg** (with `libass` enabled) to crop 9:16 videos and burn animated subtitle styles.
2. **yt-dlp** to download high-resolution YouTube video sections and transcripts.

### A. Install FFmpeg (with `libass` Subtitle Support)

> [!IMPORTANT]
> To burn animated subtitles onto videos, FFmpeg **must** be compiled with `libass` support.

#### macOS (via Homebrew):
```bash
# Install ffmpeg-full which includes libass and all subtitle filters
brew tap homebrew-ffmpeg/ffmpeg
brew install homebrew-ffmpeg/ffmpeg/ffmpeg-full

# Binary path will be:
# /opt/homebrew/opt/ffmpeg-full/bin/ffmpeg (Apple Silicon M1/M2/M3/M4)
# /usr/local/opt/ffmpeg-full/bin/ffmpeg (Intel Mac)
```

#### Ubuntu / Debian Linux:
```bash
sudo apt update
sudo apt install -y ffmpeg libass-dev
```

#### Windows:
- **Using Chocolatey**:
  ```powershell
  choco install ffmpeg-full
  ```
- **Using Scoop**:
  ```powershell
  scoop install ffmpeg
  ```
- **Manual Download**: Download the `full` build from [Gyan.dev FFmpeg Builds](https://www.gyan.dev/ffmpeg/builds/) and add its `bin` directory to your system `PATH`.

---

### B. Install yt-dlp

#### macOS (via Homebrew):
```bash
brew install yt-dlp
```

#### Ubuntu / Debian Linux:
```bash
sudo wget https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -O /usr/local/bin/yt-dlp
sudo chmod a+rx /usr/local/bin/yt-dlp
```

#### Windows:
```powershell
winget install yt-dlp
# or
choco install yt-dlp
```

#### Via Python (Cross-Platform):
```bash
pip install -U yt-dlp
```

---

## 🍃 4. Setting Up MongoDB

`clip.studio` uses MongoDB to store workspaces, video metadata, and extracted clip timestamps. You can use either a **Local MongoDB instance** or a **Free Cloud MongoDB Atlas cluster**.

### Option A: Local MongoDB (Recommended for Offline Development)

#### macOS (via Homebrew):
```bash
brew tap mongodb/brew
brew install mongodb-community
brew services start mongodb-community
```

#### Ubuntu / Debian:
```bash
sudo apt install -y mongodb-org
sudo systemctl start mongod
sudo systemctl enable mongod
```

#### Windows:
Download and run the MSI installer from [MongoDB Community Server](https://www.mongodb.com/try/download/community).

---

### Option B: MongoDB Atlas (Free Cloud Database)

If you prefer a hosted database without running MongoDB locally:

1. **Create an Account**: Sign up at [MongoDB Atlas](https://www.mongodb.com/cloud/atlas/register).
2. **Create a Free Cluster**: Choose the **M0 Free Tier** (Shared), select your region, and click **Create Deployment**.
3. **Set Up Security & Credentials**: Add a database user with password authentication and whitelist your IP (`0.0.0.0/0` for anywhere).
4. **Copy Connection String**: Get your MongoDB URI and use it in the app settings.

---

## 🔑 5. Getting AI API Keys

You only need **at least one** AI provider. You can configure multiple and switch between them.

| Provider | Best For | Free Tier | Get API Key |
| :--- | :--- | :--- | :--- |
| **Groq** | ⚡ Ultra-fast LLaMA & GPT-OSS | **Yes** | [Groq Console](https://console.groq.com/keys) |
| **Mistral AI** | 🎯 Balanced | **Yes** | [Mistral Console](https://console.mistral.ai/api-keys/) |
| **Google Gemini** | 🧠 Large context | **Yes** | [Google AI Studio](https://aistudio.google.com/app/apikey) |
| **OpenAI** | 🏆 GPT-4o | Paid | [OpenAI Platform](https://platform.openai.com/api-keys) |

---

## 📺 6. Configuring YouTube Shorts Uploads (Google OAuth)

To publish Shorts directly from the app:

1. **Set Up Google OAuth** in the Google Cloud Console (see **[GOOGLE_AUTH_SETUP.md](GOOGLE_AUTH_SETUP.md)** for detailed steps)
2. **Add Credentials** to `.env.local`:
   ```env
   GOOGLE_CLIENT_ID=your_client_id.apps.googleusercontent.com
   GOOGLE_CLIENT_SECRET=your_client_secret
   ```
3. **Connect Your Channel**: Open `/settings` > Click **Connect YouTube Channel** > Authorize.

---

## 🏃 7. Running the Application

```bash
npm run dev
```

Open your browser and navigate to:
```
http://localhost:3000
```

---

## 📄 License

This project is licensed under the **MIT License**.

---

<p align="center">
  Made with ❤️ by the <strong>clip.studio</strong> community.
</p>
