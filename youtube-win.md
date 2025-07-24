To download a **1080p MP4 YouTube video on Windows** using the **command line**, follow these steps:

---

## ✅ Step 1: Install `yt-dlp` and `ffmpeg`

### 📦 1. Download `yt-dlp.exe`:

* Go to: [https://github.com/yt-dlp/yt-dlp/releases/latest](https://github.com/yt-dlp/yt-dlp/releases/latest)
* Download `yt-dlp.exe` and save it in a folder like `C:\yt-dlp\`

### 🔧 2. Download and set up `ffmpeg`:

1. Go to: [https://ffmpeg.org/download.html](https://ffmpeg.org/download.html)
2. Choose Windows > Download a **static build** (from sites like `gyan.dev` or `BtbN`)
3. Extract the zip file
4. Add the `bin` folder (e.g., `C:\ffmpeg\bin`) to your **System PATH**:

   * Search **"Environment Variables"** in Start
   * Edit `Path` under System variables
   * Add: `C:\ffmpeg\bin`

---

## ✅ Step 2: Run Command in CMD or PowerShell

Open **Command Prompt** or **PowerShell**, then run:

```powershell
cd C:\yt-dlp
.\yt-dlp.exe -f "bv*[height=1080][ext=mp4]+ba[ext=m4a]" -o "%(title)s.%(ext)s" https://www.youtube.com/watch?v=5GhbkrMukmk
```

This will:

* Download the best 1080p MP4 video stream
* Download the best audio
* Merge them using ffmpeg
* Save it as an `.mp4` file using the video title

---

## 🧪 Test it

After the download completes, you’ll see an `.mp4` file in `C:\yt-dlp\` with the full 1080p quality and synced audio.

