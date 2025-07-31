# 🏈 GameDay: Power Your Panel. Engage Your Fans.

**GameDay** is an open-source, customizable platform for hosting dynamic sports talk panels, trivia segments, and live interviews. Built with **Blazor**, **SignalR**, **RabbitMQ**, and **OBS Studio** integrations, it empowers podcasters and streamers to personalize their broadcast experience through modular overlays and live audience engagement.

---

## 🎯 Features

- 🎙️ **Live Panel Controls** — Each participant gets a control panel to answer questions, request focus, and trigger visual effects.
- 🧠 **Real-Time Trivia Engine** — Scoreboards, timers, and overlay reactions.
- 💬 **Interactive Chat & Polls** — Blazor UI with SignalR updates.
- 🎥 **OBS Studio Integration** — Scene switching and overlay control via RabbitMQ & WebSockets.
- 📡 **Remote Guests via VDO.Ninja** — Seamless video/audio for panelists and interviews.
- 🛠️ **Modular Plugin Architecture** — Drop-in support for trivia packs, sponsor tickers, and segment tools.
- 🎛️ **Panel Leader Dashboard** — Orchestrate discussion flow, grant focus, and manage guests.

---

## 🧩 Tech Stack

| Component        | Purpose                                |
|------------------|----------------------------------------|
| Blazor (WASM/Server) | Frontend UI for panelists and leader  |
| ASP.NET Core API | Backend for session, trivia & user data|
| SignalR          | Real-time updates and focus control     |
| RabbitMQ         | Event bus for overlays & panel sync     |
| SQL Server       | Persistence for sessions, questions, users|
| OBS + VDO.Ninja  | Stream integration with remote guests   |

---

## 🚀 Getting Started

1. **Clone the repository:**
   ```cmd
   git clone https://github.com/your-org/gameday.git
   cd gameday
