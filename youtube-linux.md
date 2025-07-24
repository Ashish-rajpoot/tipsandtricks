Yes, you **can download YouTube videos from the Linux command line** using tools like `yt-dlp` or `youtube-dl`. The recommended tool now is **`yt-dlp`**, which is a more up-to-date fork of `youtube-dl`.

---

### ✅ How to Install `yt-dlp` (Recommended)

#### On Debian/Ubuntu:

```bash
sudo apt update
sudo apt install yt-dlp
```

#### Or via Python (works on most Linux distros):

```bash
pip install -U yt-dlp
```

---

### 📥 How to Download a YouTube Video

Once installed, simply run:

```bash
yt-dlp 'https://www.youtube.com/watch?v=VIDEO_ID'
```

Replace `VIDEO_ID` with the actual ID or full URL of the video.

---

### 🔧 Useful Options

* Download as audio only (MP3):

  ```bash
  yt-dlp -x --audio-format mp3 'https://www.youtube.com/watch?v=VIDEO_ID'
  ```

* Choose format:

  ```bash
  yt-dlp -F 'https://www.youtube.com/watch?v=VIDEO_ID'
  ```

  Then download a specific format:

  ```bash
  yt-dlp -f FORMAT_CODE 'https://www.youtube.com/watch?v=VIDEO_ID'
  ```

---

To download the YouTube video in **1080p MP4**, use the `yt-dlp` tool with some additional options to ensure you get the best quality **video + audio**, and **merge them** into a single `.mp4` file.

---

### ✅ Command to Download in 1080p MP4

```bash
yt-dlp -f "bv*[height=1080][ext=mp4]+ba[ext=m4a]" -o "%(title)s.%(ext)s" https://www.youtube.com/watch?v=5GhbkrMukmk
```

### 🔍 What this does:

* `bv*[height=1080][ext=mp4]`: selects **best video** at 1080p in MP4 format.
* `+ba[ext=m4a]`: adds **best audio** in M4A format (which is standard YouTube audio).
* `-o "%(title)s.%(ext)s"`: saves the file using the video title as filename.
* `yt-dlp` will **merge** the video and audio automatically into a final `.mp4` file using `ffmpeg`.

---

### ⚠️ Requirements:

To merge video and audio, `yt-dlp` needs `ffmpeg` installed. Install it with:

```bash
sudo apt install ffmpeg
```

---

