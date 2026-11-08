# Windows B2 Backup

A Windows interface for **Backblaze B2 and rclone**. Set up scheduled backups, review results, exclude folders and restore files without working at a command prompt.

**[Download the latest Windows installer](https://github.com/mitchell-levesque/b2-backup-downloads/releases/latest/download/Win-B2-Backup-Setup.exe)** | **[Release notes](https://github.com/mitchell-levesque/b2-backup-downloads/releases/latest)**

## Get started

1. Download Win-B2-Backup-Setup.exe and open it on Windows 10 or 11.
2. Connect your own Backblaze B2 application key and choose a private bucket.
3. Select backup folders, choose a schedule and activate backups.

The app requires .NET Framework 4.8, Windows PowerShell 5.1 and administrator access. Setup obtains the verified rclone engine. Python, Git and a GitHub account are not required. Each person uses their own B2 account, bucket and Windows credentials.

## User guide

Read the **[full user guide](User-guide.md)** for folder selection, Copy versus Mirror, exclusions, optimization, restore and permanent cloud cleanup.

Quick tips:

- **Copy** keeps cloud copies when local files are deleted. **Mirror** can remove cloud files that are missing locally and requires a preview.
- Excluding a folder stops future uploads. Existing cloud copies stay unless you separately confirm removal.
- Restore from **Cloud files** to a new local destination; existing files are not overwritten.
- Use **Optimize backup** to review suggestions, then apply only the exclusions you want.
- **Needs attention** contains flagged problems; **Skipped files** contains informational skips.

## Updates

Opening the installed app checks for updates in the background. Choose **Install and restart** when offered, or **Later** to keep working. A failed or offline check does not prevent the app from opening. The installer waits for active backup work and keeps saved settings, exclusions, schedule, history and credentials. Windows may ask for administrator approval.

You can also update manually by closing the manager and opening a newer setup file.


## What it does

- Copy or guarded Mirror backups, with a read-only preview.
- Scheduled backups and separate views for uploads, attention items, skips and cloud deletions.
- Folder exclusions and optional, separately confirmed permanent cloud cleanup.
- Optimization suggestions for large, frequently changing or likely disposable folders.
- Cloud browsing and file/folder restore to a new local location.

rclone handles file comparison and transfers; B2 Backup supplies the Windows interface and workflow. It is an independent application. Backblaze B2 storage charges are managed through your Backblaze account.

## About this repository

This repository distributes installers, checksums and release notes. The development repository is private. The installer contains the application; no personal backup configuration or account credentials are included.

Share the installer link, rather than your installed ProgramData folder. Releases include SHA256SUMS.txt if you want to verify a download yourself.
