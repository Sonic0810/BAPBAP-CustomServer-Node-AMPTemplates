# BAPBAP Custom Server (Node) — AMP Templates

Private AMP **Generic Module** template for the **Node.js rewrite** lobby server + Wine match host.

**Not** CubeCoders „Node.js App Runner“.  
**Not** for a PR to [CubeCoders/AMPTemplates](https://github.com/CubeCoders/AMPTemplates) (uses shell start wrapper).

## Repo layout (flat — for AMP Configuration Repositories)

```text
manifest.json
bapcustomservernode.kvp
bapcustomservernodeconfig.json
bapcustomservernodemetaconfig.json
bapcustomservernodeports.json
bapcustomservernodeupdates.json
README.md
```

## 1) Create GitHub repo + push

Suggested name: **`BAPBAP-CustomServer-Node-AMPTemplates`**

```bash
cd BAPBAP-CustomServer-AMPTemplates-Node   # this folder

git init
git add .
git commit -m "Initial BAPBAP Custom Server Node AMP template"
git branch -M main
git remote add origin https://github.com/Sonic0810/BAPBAP-CustomServer-Node-AMPTemplates.git
git push -u origin main
```

If your GitHub user/repo differs: edit `manifest.json` (`origin`/`url`) and  
`bapcustomservernodeupdates.json` → `UpdateSourceArgs` (`OWNER/REPO`).

## 2) Add repo in AMP (ADS)

1. AMP → **Configuration** → **Instance Deployment** → **Configuration Repositories**
2. Remove the **old** broken source if it still points at the previous Node module path  
   (e.g. anything that created `Sonic0810-CustomBAPBAPServerModuleNode` with bad permissions)
3. Add:

```text
https://github.com/Sonic0810/BAPBAP-CustomServer-Node-AMPTemplates
```

Branch: `main`  
4. **Fetch Latest**  
5. Fix permissions once if needed (only if you ever copied files as root):

```bash
sudo chown -R amp:amp /home/amp/.ampdata/instances/ADS01/Plugins/ADSModule/DeploymentTemplates
```

6. **Create Instance** → **BAPBAP Custom Server (Node)**

## 3) Runtime package (server files)

Instance update downloads **GitHub Release** asset:

```text
bapcustomserver-node-amp.zip
```

from the same repo (see `bapcustomservernodeupdates.json`).

Build that ZIP on Windows:

```powershell
cd C:\Users\Administrator\Downloads\CustomServer\Rewrite
.\tools\Build-NodeAmpPackage.ps1 -BuildWebUi -DownloadNodeRuntime
# → deployment-node\out\bapcustomserver-node-amp.zip
```

Upload as a **Release asset** on the new GitHub repo (name must match).

### Manual alternative (no GitHub Release yet)

- SFTP contents of the ZIP into the instance `BapCustomServer/` folder  
- Then still run **Update** once for `chmod +x` stages (or chmod yourself)

## 4) Game + Wine (matches)

Not in the Node ZIP by default. Merge:

- `start-match.sh` (+ Wine helpers)
- `game/versions/<tag>/` with `bapbap.exe`, MelonLoader, `Mods/BapCustomServerMelon.dll`

Host needs Wine + Xvfb (template requests container packages).

## 5) After create

Set in instance config:

- **Public Base URL** = `http://YOUR_HOST:5055`
- **Public Game Host** = `YOUR_HOST`
- **Admin API Token** = panel login secret

Ports: 5055/tcp, 7777/tcp, 7778/udp, 7779/tcp, 7850/tcp

Start → console should show `Now listening on:`  
Health: `http://YOUR_HOST:5055/health`

## Local source of this folder

Generated/maintained from:

```text
Rewrite/deployment-node/amp-module/BAPBAP-CustomServer-AMPTemplates-Node/
```
