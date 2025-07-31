# 🏈 GameDay: Power Your Panel. Engage Your Fans.

**GameDay** is an open-source, customizable platform for hosting dynamic sports talk panels, trivia segments, and live interviews. Built with **Blazor**, **SignalR**, **RabbitMQ**, and **OBS Studio** integrations, it empowers podcasters and streamers to personalize their broadcast experience through modular overlays and live audience engagement.

> 💡 **Design Vision:** See [`WakeUp-Idea1-Trivia.png`](./WakeUp-Idea1-Trivia.png) for a visual concept of the GameDay interface layout and trivia functionality.

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

### Prerequisites

- **[.NET 9.0.302+ SDK](https://dotnet.microsoft.com/download)** (Required for .NET Aspire - Compatible with .NET 10 when available)
- **[Docker Desktop](https://www.docker.com/products/docker-desktop)** (Aspire will manage containers automatically)
- **[.NET Aspire workload](https://learn.microsoft.com/en-us/dotnet/aspire/fundamentals/setup-tooling)** (`dotnet workload install aspire`)
- **[OBS Studio](https://obsproject.com/)** *(Optional - only needed for streaming integration testing)*

### Quick Start

1. **Clone the repository:**
   ```cmd
   git clone https://github.com/jedelfraisse/gameday.git
   cd gameday
   ```

2. **Install Aspire workload** (if not already installed):
   ```cmd
   dotnet workload install aspire
   ```

3. **Run the application with Aspire:**
   ```cmd
   dotnet run --project GameDay/GameDay.AppHost
   ```
   
   Aspire will automatically:
   - Start SQL Server container
   - Start RabbitMQ container with management UI
   - Launch the GameDay web application
   - Launch the API service
   - Open the Aspire dashboard

4. **Access the platform:**
   - **Aspire Dashboard:** http://localhost:15888 (monitoring, logs, metrics)
   - **Panel Leader Dashboard:** http://localhost:5000/leader
   - **Panelist Interface:** http://localhost:5000/panel
   - **Audience View:** http://localhost:5000
   - **API Documentation:** http://localhost:5001/swagger
   - **RabbitMQ Management:** http://localhost:15672

---

## 🏗️ Project Structure

```
GameDay/                      # Solution folder
├── GameDay.AppHost/          # .NET Aspire orchestration project
├── GameDay.ServiceDefaults/  # Shared Aspire service configurations
├── GameDay.Web/              # Blazor Server UI (Panelist & Leader interfaces)
├── GameDay.ApiService/       # ASP.NET Core Web API (Session & data management)
├── GameDay.Core/             # Domain models and business logic
├── GameDay.Infrastructure/   # Data access, SignalR hubs, RabbitMQ
├── GameDay.Plugins/          # Plugin system and extensions
└── GameDay.Shared/           # Shared DTOs and contracts

tests/
├── GameDay.Core.Tests/       # Unit tests
├── GameDay.Integration.Tests/ # Integration tests
└── GameDay.Web.Tests/        # UI tests

obs/
├── overlays/                 # OBS Browser Source overlays
├── scenes/                   # Sample OBS scene configurations
└── plugins/                  # OBS WebSocket integration scripts
```

---

## 🎛️ Usage

### For Panel Leaders (Show Hosts)
1. Create a new session from the Leader Dashboard
2. Configure trivia questions and segments
3. Invite panelists via shared session codes
4. Control discussion flow, grant focus, and manage timing
5. *(Optional)* Connect OBS for live streaming overlays

### For Panelists (Guests)
1. Join session using the provided code
2. Use your control panel to:
   - Request speaking focus
   - Answer trivia questions
   - Trigger reaction effects
   - Participate in polls

### For Audience (Viewers)
1. View live session without login required
2. Participate in polls and chat
3. See real-time trivia scores and reactions

---

## 🔧 Configuration

### Aspire Configuration

Aspire automatically manages service discovery and configuration between services. Database connections, RabbitMQ messaging, and service-to-service communication are handled automatically.

### Environment Variables

For additional configuration, create an `appsettings.Development.json` in each project:

```json
{
  "OBS": {
    "WebSocketUrl": "ws://localhost:4455",
    "WebSocketPassword": "your-obs-password"
  },
  "SignalR": {
    "MaxConnectionsPerUser": 5
  }
}
```

### OBS Studio Setup (Optional)

1. **Install OBS WebSocket Plugin:** Download from [OBS WebSocket Releases](https://github.com/obsproject/obs-websocket/releases)
2. **Configure WebSocket:** Tools → WebSocket Server Settings
3. **Add Browser Sources:** 
   - Scoreboard: `http://localhost:5000/overlay/scoreboard`
   - Panel Focus: `http://localhost:5000/overlay/focus`
   - Chat Feed: `http://localhost:5000/overlay/chat`

---

## 🧪 Development

### Aspire Dashboard

The Aspire dashboard provides comprehensive monitoring:
- **Service Health:** Real-time status of all services
- **Logs:** Centralized logging from all containers and services
- **Metrics:** Performance monitoring and telemetry
- **Traces:** Distributed tracing across services

### Database Migrations

```cmd
# Add new migration
dotnet ef migrations add MigrationName --project GameDay/GameDay.Infrastructure

# Update database (Aspire handles connection strings)
dotnet ef database update --project GameDay/GameDay.Infrastructure
```

### Running Tests

```cmd
# Unit tests
dotnet test tests/GameDay.Core.Tests/

# Integration tests (uses Aspire test containers)
dotnet test tests/GameDay.Integration.Tests/
```

### Plugin Development

Create custom plugins by implementing `IGameDayPlugin`:

```csharp
public class CustomTriviaPlugin : IGameDayPlugin
{
    public string Name => "Custom Trivia Pack";
    public string Version => "1.0.0";
    
    public void Initialize(IServiceCollection services)
    {
        // Register your services
    }
}
```

---

## 🚀 Deployment

### Development with Aspire

```cmd
# Run with Aspire orchestration
dotnet run --project GameDay/GameDay.AppHost

# Run specific service independently
dotnet run --project GameDay/GameDay.Web
dotnet run --project GameDay/GameDay.ApiService
```

### Production Deployment

Aspire can generate deployment manifests for various platforms:

```cmd
# Generate Azure Container Apps manifest
dotnet run --project GameDay/GameDay.AppHost -- --publisher manifest --output-path ./aspire-manifest.json

# Deploy to Azure Container Apps
azd deploy
```

### Cloud Deployment Options

- **Azure Container Apps:** Native Aspire support with `azd` tooling
- **Kubernetes:** Generate Kubernetes manifests from Aspire
- **Docker Compose:** Export to docker-compose for traditional hosting

---

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guidelines](CONTRIBUTING.md) for details.

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Commit changes: `git commit -m 'Add amazing feature'`
4. Push to branch: `git push origin feature/amazing-feature`
5. Open a Pull Request

---

## 📋 Roadmap

- [ ] **v1.0** - Core panel and trivia functionality
- [ ] **v1.1** - Advanced OBS integration with scene automation
- [ ] **v1.2** - Mobile-responsive panelist interface
- [ ] **v1.3** - Twitch/YouTube chat integration
- [ ] **v1.4** - Advanced analytics and session replay
- [ ] **v1.5** - Plugin marketplace and community extensions

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🆘 Support

- **Documentation:** [Wiki](https://github.com/jedelfraisse/gameday/wiki)
- **Issues:** [GitHub Issues](https://github.com/jedelfraisse/gameday/issues)
- **Discussions:** [GitHub Discussions](https://github.com/jedelfraisse/gameday/discussions)
- **Discord:** [Join our community](https://discord.gg/gameday) *(Coming Soon)*

---

**Built with ❤️ for the sports podcasting and streaming community**
