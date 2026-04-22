
# 📡 Speedtest Tracker - Docker Setup

A simple self-hosted setup for [Speedtest Tracker](https://github.com/alexjustesen/speedtest-tracker) using Docker Compose on Ubuntu. This setup lets you monitor internet speed on a schedule, view historical results, and set performance thresholds with a beautiful web UI.

---

## 🔧 Features

- 📈 Automatic speed tests (every 30 mins)
- 🌍 Timezone adjusted to IST (Asia/Kolkata)
- 💾 Persistent storage using SQLite
- 🔐 SSL support (self-signed or custom keys)
- 📊 Clean and responsive web interface
- 🛠️ Easily configurable with Docker Compose
- Apprise (notification handler)  

---

## 📁 Project Structure

```

speedtest-tracker/
├── docker-compose.yaml
└── config/
├── keys/            # SSL keys (optional)
└── data/            # App data and DB

````

---

## 🐳 Docker Compose Setup

Here’s the `docker-compose.yaml` used:

```yaml
services:
  speedtest-tracker:
    image: lscr.io/linuxserver/speedtest-tracker:latest
    container_name: speedtest-tracker
    restart: unless-stopped
    ports:
      - 8080:80
      - 8443:443
    environment:
      - PUID=1000
      - PGID=1000
      - APP_TIMEZONE=Asia/Kolkata
      - DISPLAY_TIMEZONE=Asia/Kolkata
      - TZ=Asia/Kolkata

      - SPEEDTEST_SCHEDULE=*/10 * * * *
      - SPEEDTEST_SERVERS=50231

      - NOTIFICATION_DRIVER=apprise
      - APPRISE_URL=http://apprise:8000/notify
      - APPRISE_TAG=all

      - DB_CONNECTION=sqlite

    volumes:
      - ./config:/config

    depends_on:
      - apprise

  apprise:
    image: caronc/apprise
    container_name: apprise
    restart: unless-stopped
    ports:
      - 8000:8000
````

> 📌 **Note**: Replace `your-generated-app-key` with your own key from [Speedtest Tracker App Key Generator](https://speedtest-tracker.dev/)

---

## 🕒 Scheduling

This setup runs a speed test every **30 minutes**, using the cron expression:

```
*/30 * * * *
```

You can modify this via the `SPEEDTEST_SCHEDULE` environment variable.

---

## 🌐 Access

* **Web UI**: [http://localhost:8080](http://localhost:8080)
* **HTTPS UI**: [https://localhost:8443](https://localhost:8443) *(if using SSL)*

Key settings:
Speed test interval:
```
SPEEDTEST_SCHEDULE=*/10 * * * *
```
Fixed server (optional):
```
SPEEDTEST_SERVERS=50231
```
Apprise webhook:
```
APPRISE_URL=http://apprise:8000/notify
```
📲 Telegram Alerts

Apprise is used to forward notifications to Telegram.

Format used:
```
tgram://BOT_TOKEN/CHAT_ID
```
---

## 📸 Screenshots

<img width="1919" height="1000" alt="image" src="https://github.com/user-attachments/assets/2ae637e9-e5e4-4051-b444-ee0b71e9c0f1" />

<img width="1910" height="995" alt="image" src="https://github.com/user-attachments/assets/08a460cc-5daa-4a2f-ae59-bc3d879ab9a9" />

<img width="1220" height="2603" alt="Media (55)" src="https://github.com/user-attachments/assets/da2edeab-9a45-4add-99ee-fc6b66442c9d" />

---

## 🙋‍♂️ Why I Built This

> *"Is it just me, or is the internet slow again?"*  
> *— Everyone, all the time*

It all started when people kept complaining about internet speed.  
But every time I ran a speed test — boom 💥 — speeds looked perfect. Suspiciously perfect.

So I asked myself:

![giphy](https://github.com/user-attachments/assets/92cf6a43-33bc-4f95-942e-e67f2e845c26)


**Is my ISP really delivering what they promise all the time?**  
I started manually testing using:
- Speedtest.net  
- Fast.com  
- MyFibre app  

But then I thought, *how many times can I keep running these tests like a caveman?*

---

### 💡 The Idea

Let’s automate it!  
I set up `speedtest-cli` with `cron` jobs to run tests on a schedule. Great. ✅  
But then came the **next challenge**:  
📊 **Data analysis.**

My terminal started looking like Matrix logs — tons of numbers, no insight.  
This was spiraling out of hand.

---

### 🔍 Enter: **Speedtest Tracker**

I figured, *someone must’ve solved this already*.  
A quick search led me to the amazing open-source project: **Speedtest Tracker**.

And wow — what a tool:

✅ Beautiful UI  
📈 Real-time graphs & charts  
📥 Exportable Excel reports (perfect to send to the ISP)  
🚀 Quick Docker setup  
🔔 Smart notifications when speeds drop

I configured it to alert me if speeds fall below:
- **Download:** 120 Mbps  
- **Upload:** 120 Mbps  

Now, when things go sideways — I *know exactly when and how*.

---

### 🚀 TL;DR

Tired of arguing with your ISP about inconsistent speeds?  
Set up Speedtest Tracker with Docker, and let the graphs do the talking.  
I’ve even attached screenshots to show you how it looks.

Try it yourself — it’s a great tool!

---

## 🙏 Credits

This setup is based on the open-source project:
➡️ [alexjustesen/speedtest-tracker](https://github.com/alexjustesen/speedtest-tracker)
Kudos to [@alexjustesen](https://github.com/alexjustesen) and contributors for building and maintaining this excellent tool.

Also thanks to [linuxserver.io](https://www.linuxserver.io/) for the Docker image.

---

## 📜 License

MIT
