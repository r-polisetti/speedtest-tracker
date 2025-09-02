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

---

## 📁 Project Structure



speedtest-tracker/
├── docker-compose.yaml
└── config/
├── keys/            # SSL keys (optional)
└── data/            # App data and DB



---

## 🐳 Docker Compose Setup

Here’s the `docker-compose.yaml` used:

```yaml
services:
  speedtest-tracker:
    image: lscr.io/linuxserver/speedtest-tracker:latest
    restart: unless-stopped
    container_name: speedtest-tracker
    ports:
      - 8080:80          # Web UI
      - 8443:443         # HTTPS
    environment:
      - PUID=1000
      - PGID=1000
      - APP_NAME=Speedtest-tracker
      - APP_TIMEZONE=Asia/Kolkata
      - DISPLAY_TIMEZONE=Asia/Kolkata
      - TZ=Asia/Kolkata
      - SPEEDTEST_SCHEDULE=*/30 * * * *
      - APP_KEY=base64:your-generated-app-key
      - DB_CONNECTION=sqlite
    volumes:
      - ./config:/config
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

---

## 📸 Screenshots

<img width="1912" height="972" alt="image" src="https://github.com/user-attachments/assets/2c4899cd-1287-45a4-b035-68d99eae1627" />


---

## 🙋‍♂️ Why I Built This

This project helps monitor broadband performance over time and ensures my connection consistently meets ISP claims. It was also a great opportunity to explore Docker, cron scheduling, and real-time network diagnostics.

---

## 🙏 Credits

This setup is based on the open-source project:
➡️ [alexjustesen/speedtest-tracker](https://github.com/alexjustesen/speedtest-tracker)
Kudos to [@alexjustesen](https://github.com/alexjustesen) and contributors for building and maintaining this excellent tool.

Also thanks to [linuxserver.io](https://www.linuxserver.io/) for the Docker image.

---

## 📜 License

MIT

```

---

You're good to go — just paste this into your GitHub `README.md`, upload your screenshots, and commit. Let me know if you ever want to add badges (e.g., Docker, GitHub stars, etc.) later.
```
