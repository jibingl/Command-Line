# Commands for Blue Screen of Death

## SFC (System File Checker)
SFC checks for missing important files of your Windows operating system and restores them from the cache.  
To run _CHKDSK_ on your **C: drive**
```
chkdsk c:
```
To fix problems scanned by _CHKDSK_ automatically, type.
```
chkdsk /f c:
```
To scan errors and bad sectors, type
```
chkdsk /r c:
```

## CHKDSK (Check Disk)
CHKDSK scans your drive to find bad sectors and tries to fix errors in the file system.  
For a full scan and automatic repair of corrupted files by SFC, type this command
```
sfc /scannow
```
To run SFC in Read-Only mode, then use this command
```
sfc /verifyonly command
```

## DISM (Deployment Image Servicing and Management)
DISM directly deals with faulty Windows images and repairs them by downloading actual replacement files from Windows’ online servers.  
DISM may require an active internet connection for its complete functionality.  
To check the status of your drive. This step gives us an idea of whether the core components of your drive are corrupted or not.
```
dism /Online /Cleanup-Image /CheckHealth
```
To check the health of your drive for an **advanced** _DISM_ scan
```
dism /Online /Cleanup-Image /ScanHealth
```
Once done, we need to apply these fixes to the operating system permanently.
```
Dism /Online /Cleanup-Image /RestoreHealth
```



