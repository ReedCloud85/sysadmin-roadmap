# Day 2 Thursday 8/6/26

Objectives for today were to:
- [X] Create a Windows Server VM with the Windows Server 2022 ISO downloaded on Day 1 (Image Attached in Images folder)
  -VM Name: SRV-DC01
  -Operating System: Windows Server 2022 Evaluation
  -RAM: 6GB
  -Disk: 60GB
  -Network Mode: NAT
- [X] Configure server hostname (Image Attached in Images folder)
  -Renamed the server to SRV-DC01
- [X] Configure a static IP (Image Attached in Images folder)
  -IPv4: 192.168.150.200
  -Subnet: 255.255.255.0
  -Gateway: 192.168.150.2
  -DNS: 192.168.150.2
- [X] Test Network Connectivity
  -Verified connectivity by pinging 192.168.150.2 and 8.8.8.8
- [X] Install Windows Updates (Image Attached in Images folder)
  -Also ran 'DISM /Online /Cleanup-Image /RestoreHealth' to verify the Windows component store was healthy and repair any corruption that may have been present.
- [X] Create a clean VM snapshot (Image Attached in Images folder)
  -Snapshot named CLEAN-BASE.

<img width="1060" height="728" alt="Desktop" src="https://github.com/user-attachments/assets/dba3c868-34c9-46b9-af5a-b3a62fefdda5" />
<img width="677" height="401" alt="ipconfig all" src="https://github.com/user-attachments/assets/93349bac-c3e7-4881-ad6d-5255f23ab43e" />
<img width="311" height="91" alt="Hostname" src="https://github.com/user-attachments/assets/f7197c53-8e97-4b6f-90ea-c2e21e02fd38" />
<img width="368" height="166" alt="Windows Update" src="https://github.com/user-attachments/assets/ddcec557-1db0-450d-a545-ba46db6eaf96" />
<img width="238" height="177" alt="Snapshot" src="https://github.com/user-attachments/assets/5bb1228a-fff2-45ff-9574-3e6361bfd422" />
