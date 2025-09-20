# Agent OS Workspace

This workspace manages Agent OS installations and projects using Git submodules for easy updates.

## Directory Structure

```
D:\agent-os-workspace\
├── .git\                   # Workspace git repository
├── .gitmodules            # Submodule configuration
├── agent-os\              # Agent OS submodule (your fork)
├── .agent-os\             # Symlink pointing to agent-os\
├── project-1\             # Your projects (separate git repos)
├── project-2\
└── README.md              # This file
```

## Initial Setup

### 1. Add Your Fork as Submodule

```bash
cd D:\agent-os-workspace
git submodule add https://github.com/donshults/agent-os.git agent-os
git submodule init
git submodule update
```

### 2. Set Up Upstream Remote

```bash
cd agent-os
git remote add upstream https://github.com/buildermethods/agent-os.git
git remote -v  # Verify remotes
```

### 3. Create .agent-os Directory

**Option A: Junction (Run as Administrator)**
```cmd
cd D:\agent-os-workspace
mklink /J .agent-os agent-os
```

**Option B: Copy (Recommended for compatibility)**
```bash
cd D:\agent-os-workspace
cp -r agent-os .agent-os
```

**Note:** We use a copy for maximum compatibility. This requires updating `.agent-os` after pulling upstream changes.

## Maintaining and Updating

### Update from Original Agent OS Repository

1. **Fetch latest changes from upstream:**
```bash
cd D:\agent-os-workspace\agent-os
git fetch upstream
git checkout main
git merge upstream/main
```

2. **Push updates to your fork:**
```bash
git push origin main
```

3. **Update the submodule reference in workspace:**
```bash
cd D:\agent-os-workspace
git add agent-os
git commit -m "Update Agent OS to latest version"
git push
```

4. **Update .agent-os copy (if using copy instead of symlink):**
```bash
cd D:\agent-os-workspace
rm -rf .agent-os
cp -r agent-os .agent-os
```

### Update from Your Fork

If you've made changes to your fork elsewhere:

```bash
cd D:\agent-os-workspace\agent-os
git pull origin main
cd ..
git add agent-os
git commit -m "Update Agent OS from fork"
```

### Check Current Version

```bash
cd D:\agent-os-workspace\agent-os
git log --oneline -1
git describe --tags
```

## Creating New Projects

1. **Create project directory:**
```bash
cd D:\agent-os-workspace
mkdir my-new-project
cd my-new-project
```

2. **Initialize as separate git repository:**
```bash
git init
git remote add origin https://github.com/donshults/my-new-project.git
```

3. **Agent OS configuration will be available via the .agent-os symlink**

## Troubleshooting

### .agent-os Directory Issues

**If using junction/symlink:**
- Ensure you ran the command prompt/PowerShell as Administrator
- Try junction instead of symlink: `mklink /J .agent-os agent-os`

**If using copy method:**
- Remember to update after pulling changes: `rm -rf .agent-os && cp -r agent-os .agent-os`
- Alternative Windows copy: `xcopy /E /I /Y agent-os .agent-os`

### Submodule Out of Sync

```bash
cd D:\agent-os-workspace
git submodule update --remote --merge
```

### Reset to Clean State

```bash
cd D:\agent-os-workspace\agent-os
git reset --hard upstream/main
git clean -fd
```

## Important Notes

- **Never modify files directly in the `agent-os` directory** unless you intend to contribute back
- Keep your customizations in project directories or separate config files
- The `.agent-os` symlink allows all projects to use the same Agent OS installation
- Each project maintains its own git repository independent of Agent OS

## Backup Strategy

1. **Workspace repository:** Tracks which version of Agent OS you're using
2. **Your fork:** Preserves any customizations you've made
3. **Project repositories:** Each project has its own backup via GitHub

## Version Tracking

Record Agent OS version when updating:

```bash
cd D:\agent-os-workspace\agent-os
git describe --tags > ../AGENT_OS_VERSION.txt
cd ..
git add AGENT_OS_VERSION.txt
git commit -m "Record Agent OS version"
```