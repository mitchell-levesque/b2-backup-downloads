# B2 Backup user guide

Current release: **3.2.25**. Download **Win-B2-Backup-Setup.exe** from the [downloads page](https://github.com/mitchell-levesque/b2-backup-downloads/releases/latest).

B2 Backup is a Windows interface for rclone and Backblaze B2. rclone compares and transfers files; the app manages setup, scheduling, exclusions, run results, optimization and cloud cleanup. Python and developer tools are not required to use the app.

## Requirements

Windows 10/11, .NET Framework 4.8, Windows PowerShell 5.1, administrator access, an internet connection and your own Backblaze B2 account are required. The installer downloads a pinned, checksum-verified rclone engine.

Use the Windows account that owns the installation. Elevation under another account is rejected. New installations are stored in `C:\ProgramData\Win-B2-Backup`; the manager is **B2 Backup.exe**. If an existing backup is found under `C:\ProgramData\PC-B2-Backup`, it is reused in place, including its saved schedule, credentials and history.

## Install or update

Open **Win-B2-Backup-Setup.exe**. A new installation opens guided setup. Opening a newer installer updates the existing application and worker, then opens Backup overview. Settings, exclusions, saved credentials, history and queued removals are retained. A paused schedule stays paused.

The installed app checks for a newer stable release in the background when opened. If one is available, choose **Install and restart** or **Later**. Later dismisses the offer for this opening; the app checks again next time. Offline or failed checks do not block normal use.

The update is downloaded from the public downloads repository. Its size, SHA256 digest and executable version are checked before it runs. Unsaved settings must be saved or cancelled first, and the button is disabled during local operations. The installer waits if an independent backup, preview or cleanup is still running. A failed download leaves the installed version unchanged and can be retried. Windows may ask for administrator approval.

Update checks send an HTTPS request to GitHub; they do not include B2 credentials, backup paths or logs. No GitHub login is required.

Version 3.2.24 and later detect saved installations from before the rename. If 3.2.23 displayed first-time setup on a previously configured PC, close that window and open the latest setup file. It will find the existing backup without asking you to configure it again.

Close older manager windows before updating. If a backup, preview or cleanup is running, the update waits; choose Later to return after it finishes. Follow the retry prompt if another manager window is still open. Failed updates restore the previous files. Older installers cannot downgrade a newer installation.

Check the installed version under **Options > Support and maintenance**.


## First-time setup

1. Enter your B2 application key ID and key. Bucket listing and file list/read/write/delete permissions are needed. Bucket-restricted keys are supported; prefix-restricted keys are not. Choose a private bucket dedicated to this PC. Creating a bucket also requires bucket-creation permission.
2. Choose your backup folders. Users includes every local Windows profile, including Public and Default; ProgramData includes shared application data. Add other folders or drives as needed. **+** marks included folders and **-** marks exclusions.
3. Choose Copy or Mirror and a schedule. Copy keeps cloud copies after local deletion. Mirror removes covered cloud files that are missing locally, with safeguards for unreadable and excluded paths.
4. For backups while signed out, enter your Windows account password, rather than a Windows Hello PIN. Task Scheduler retains the credentials. Signed-in backups can run while the screen is locked.
5. Review and activate. Preview is optional for Copy and required for Mirror. A preview uploads and deletes nothing. Comparison can take several minutes. Scheduled backups continue after the manager closes.

## Managing backups

The installed app has five sections:

| Section | What it contains |
| --- | --- |
| Backup overview | Latest result, next run, running activity, recent history, Back up now, Preview backup and pause/resume |
| Backup folders | Included folders, exclusions and Optimize backup |
| Backup settings | Schedule, Copy/Mirror, battery policy, signed-out credentials and B2 account |
| Cloud files | Cloud browsing, restore, folder-size calculation, queued removals and retention |
| Options | Transfer/check settings, attention preferences, optimization settings and support tools |

Pages are directly editable. **Save changes** and **Cancel** appear when a setting changes. Save or cancel before switching sections. Background refresh preserves the draft, and a conflicting save from another window is detected before committing it.

Backup and preview ask for confirmation before starting. Pause/resume displays progress while Windows applies the change; pausing the schedule does not stop an already-running backup.

### Folders and coverage

Add folder, Exclude folder, Remove folder and Include again update the draft. Selecting a row shows its local path and cloud destination. Adding a child already covered by a parent reports that it is already included. Previously saved nested mappings and include-only selections remain supported.

Exclusions stop future uploads and keep existing cloud copies by default. Choose **Also remove existing cloud copies** if you want to queue permanent cloud removal. The queue is prepared after the exclusion is saved; large inventories can take time to list.

Existing Copy coverage can usually be saved without a full scan or task change. New sources/destinations and Mirror coverage changes use the reviewed validation path. Excluding a folder from a current preview updates its displayed totals immediately; Mirror activation still requires a fresh preview. Historical runs retain their original records.

Application credentials, live state and logs are automatically protected from backup. Junctions, unreadable paths and unavailable cloud-only local files may be skipped. This application does not encrypt ordinary files before upload; encrypted vaults are copied as stored.

### Schedule and cloud account

Backup settings lets you change the schedule, battery policy and signed-out behavior. Compatible saved task credentials are retained. **Update password** explicitly replaces them; some signed-out schedule changes require your Windows password again.

Choose a private bucket from the B2 dropdown. Buckets load in the background and can be refreshed. The saved destination is not silently replaced during loading. To replace the application key, enter both its ID and key, then refresh the available buckets. Save validates the changed settings. Cancelling clears staged credentials.

### Run results

Preview results contain **Planned uploads**, **Needs attention**, **Skipped files** and **Planned deletions**. Backup results contain **Uploaded**, **Needs attention**, **Skipped files** and **Deleted from cloud**.

The tabs share folder navigation, keyword search and table layout. Individual-file pages contain at most 50 items. Viewing results reads retained logs; it does not rescan your folders or B2.

Sizes describe the recorded operations, not the entire local folder. Deletions include cloud files that may no longer exist locally. Unknown sizes show a dash. Permanent cleanup records file counts and stored-version counts separately: one file may have several removed versions. A processed approved version that was already absent is not recorded as a new deletion.

History sorts dates, file counts, uploaded bytes and durations by their values. Local history defaults to 14 days, with up to 500 retained runs available in History. File details are available while the corresponding journals remain retained. The app rebuilds missing large-cleanup indexes from those journals when results are opened.

### Skips and attention preferences

Options lets you choose which skipped-file categories need attention and which are informational. In-use, changing, unavailable, local access-denied and general permission-needed skips are informational by default. Cloud permissions, connection issues, storage full, read/compare failures and other issues are flagged by default. Read/compare failures have a separate category. Unclassified operation failures remain separate from routine skips.

All skips remain visible and are retried later. Failed operations, critical errors, unreadable whole sources and blocked Mirror deletion checks still need attention. Attention preferences also affect how old runs are displayed without changing their original records. Success with informational skips does not mean every file was uploaded. Preferences do not weaken deletion safeguards.

## Cloud browsing and restore

Cloud files starts at **All backup folders**, with Users, ProgramData and other configured sources shown as folders. Open a folder and use the path buttons to navigate. Ordinary browsing includes file sizes without calculating every folder's contents.

Select a file for **Restore file** or a folder for **Restore folder**. File restore lets you choose a new filename. Folder restore asks for a local parent and creates a folder with the selected name, including descendants. Choose a different parent if that folder already exists. Existing files and folders are not overwritten or merged. Downloads are staged and moved to the final destination only after success.

Right-click a selected folder and choose **Calculate folder size**. Hold Ctrl to select several folders and calculate their sizes sequentially. The calculation window counts files and bytes as metadata arrives; it does not download file contents. Closing it stops the active calculation and leaves later selections unprocessed.

Completed totals are cached for the manager session. Incomplete results are not cached. Refresh, a completed backup or a saved configuration change invalidates cached totals. Totals include current files below the selected folder, including subfolders, but not older/hidden versions or billed storage. Separate totals are not calculated for every descendant folder.

Tables grow with the available window height. Expanding explanatory notes keeps the main list height stable; longer page content can be scrolled.

## Permanent cloud cleanup

An exclusion with **Also remove existing cloud copies**, or **Remove cloud folder** in the cloud browser, prepares an approved inventory of exact B2 version IDs. Review the confirmation carefully: cleanup permanently removes current and older approved versions. Local files remain unchanged.

The next real backup processes queued cleanup before scanning normal sources. It runs until complete unless stopped or an error occurs; there is no default 1,000-version or two-minute cutoff. Large queues can take hours because deletion requests are sequential. **Stop cleanup and continue backup** stops after the current request, preserves the checkpoint and continues the normal backup. Remaining approved removals resume on a later backup.

New versions created after approval remain untouched. B2 Object Lock and retention protections are respected. Unrelated local read errors do not block explicitly approved removal. The saved inventory is bounded to one million versions per request.

Pending removals and completed removal history are separate. An exclusion-based removal requires its saved exclusion to remain in place. Cloud-only removals do not add an exclusion; included local files can upload again on a future backup. Removing an entire configured source or bucket through the folder-removal action is refused.

Cancel removal stops future work and keeps the exclusion. Already deleted versions cannot be recovered. Ordinary Mirror deletion may retain older B2 versions under lifecycle rules, so its recorded size does not promise reclaimed storage. Bucket retention settings are under **Cloud files > Cloud retention**.

## Optimize backup

**Backup folders > Optimize backup** runs a preview, then examines local folder metadata and recent matching backup results. Normal backups collect history without running this additional analysis. Optimization does not read file contents or contact an AI service.

Suggestions distinguish **Large**, **Frequently changing** and **Likely disposable** folders. A large photo or video folder can be suggested without being called a cache. General names and locations identify likely disposable data; there is no hardcoded catalog of installed apps and no automatic cache-exclusion checkbox.

Nothing starts selected. **Exclude** stages a normal exclusion. **Keep** hides the exact folder from future suggestions, while allowing its children to be suggested. **Apply choices** records these decisions; save any staged backup-folder exclusions afterward. Cloud copies stay unless removal is separately confirmed.

**Exclude all shown**, **Keep all shown** and **Uncheck all shown** respect the active filter. Choices survive sorting and filtering. No-match completion explicitly reports **No optimizations found** or **No new optimizations found** when matching folders were already reviewed.

Optimization settings are available from Options and the optimization confirmation. Defaults are:

| Category | Default threshold |
| --- | --- |
| Large | 1,024 MB |
| Frequently changing | 4 MB uploaded across at least two of the latest five matching retained backups |
| Likely disposable | Nonempty folders, with a 0 MB minimum |

Thresholds accept fractional MB. Detection names and required locations are shown for reference. **Folders not suggested again** contains the collapsed remembered list. Allow suggestions again reverses a Keep choice; Exclude selected folders opens an ordinary unsaved exclusion draft.

Readable folder sizes may be lower bounds if access fails. Parent and child sizes overlap. Repeated-upload suggestions need retained history; size and name/location suggestions work without it. Unreadable paths or the 100,000-folder tracking guard can make an inventory incomplete, and results report that limitation.

Optimization-origin dry runs appear as **Optimization** in history. Ordinary previews retain their Preview label. Both are read-only.

## Performance, logs and support

rclone compares included local and cloud inventories and transfers new or changed files. Scanning many small files can dominate a backup even when few bytes need uploading. Excluding unnecessary folders reduces inventory and transfer work. The default comparison concurrency remains 8; isolated 8/16/32 tests did not show a consistent benefit from higher values.

The interface reads large results in bounded pages and loads cloud listings asynchronously. Idle status normally refreshes every 30 seconds and every five seconds while a backup is running. Closing the manager does not stop scheduled backups.

**Open backup logs** shows retained rclone logs and run summaries. **Open app diagnostics** shows interface exception records under `%LOCALAPPDATA%\B2Backup\diagnostics`. The diagnostics folder is created when opened. GUI diagnostics record exception types and stack traces rather than raw credentials or backend responses, and their file size is bounded.

JSON stores settings and summaries; JSONL stores one operation record per line for streaming and searching. Both are plain text. Use the result tabs and readable report/export for everyday review. Retention applies to local history; B2 retention is configured separately.

## Sharing and removal

Share **Win-B2-Backup-Setup.exe**. Each person configures their own B2 key, bucket and Windows account. Installed credentials and configuration are local to that person; share the installer rather than the installed ProgramData directory.

Support and maintenance includes uninstall. Removing the application does not turn an exclusion into a cloud deletion; use the reviewed cloud-cleanup workflow for permanent remote removal.
