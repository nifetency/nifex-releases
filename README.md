# NifeX Tunnel Agent

Publish a service running on your server, either a web app (HTTP) or any TCP service such as a database, through NifeX. No inbound port needs to be opened. The agent connects **out** to NifeX.

## 1. Create the tunnel

In the NifeX dashboard, open your service, go to **Overview → TCP Tunnel**, choose a URL and click **Create tunnel**. Copy the token, because it is shown only once.

## 2. Download the agent

Pick the file for your server from the [Releases](../../releases) page:

| OS | File |
|---|---|
| Linux x64 | `nifex-tunnel-agent-linux-x64` |
| Linux ARM64 | `nifex-tunnel-agent-linux-arm64` |
| macOS Intel | `nifex-tunnel-agent-macos-x64` |
| macOS Apple Silicon | `nifex-tunnel-agent-macos-arm64` |
| Windows x64 | `nifex-tunnel-agent-windows-x64.exe` |

Linux / macOS:
```bash
curl -fL -o nifex-tunnel-agent https://github.com/nifetency/nifex-releases/releases/download/v1.0.1/<file-name>
chmod +x nifex-tunnel-agent
```

Windows (PowerShell):
```powershell
Invoke-WebRequest -Uri "https://github.com/nifetency/nifex-releases/releases/download/v1.0.1/nifex-tunnel-agent-windows-x64.exe" -OutFile nifex-tunnel-agent.exe
```

## 3. Run it

Use the command the dashboard shows. Set `--local-port` to the port your app or database listens on **on this server**.

Linux / macOS:
```bash
./nifex-tunnel-agent \
  --server 'ws://on.nifetency.com:4444/tunnel/ws' \
  --token '<YOUR_TOKEN>' \
  --host 'myapp.on.nifetency.com' \
  --local-port 8080
```

Windows:
```powershell
.\nifex-tunnel-agent.exe --server "ws://on.nifetency.com:4444/tunnel/ws" --token "<YOUR_TOKEN>" --host "myapp.on.nifetency.com" --local-port 8080
```

The dashboard badge changes to **Connected** when the agent is up. Keep it running (`nohup`, `screen`, or a service).

## 4. Use it

Use the tunnel address saved in the dashboard, for example `myapp.on.nifetency.com:10070`.

- **Web app (HTTP):** open `http://myapp.on.nifetency.com:10070` in a browser.
- **Database or any TCP service:** point your client at the same host and port, for example `psql "host=myapp.on.nifetency.com port=10070 user=<user> dbname=<db>"`.

## Requirements

Outbound access from your server to `on.nifetency.com` on port **4444**.

## Troubleshooting

- **Stays on "Waiting for agent":** check the token, and test the connection with `nc -vz on.nifetency.com 4444`.
- **Connects but the app doesn't load:** check that your service is listening on the `--local-port` you passed.
- **Lost the token:** remove the tunnel in the dashboard and create it again.
