# Backup # Praktikum 13

Proyek ini mendemonstrasikan backup di Linux dengan berbagai metode dan monitoring.

Backup /etc dibuat dengan tar, kemudian direstore ke /tmp/etc_restore. Script backup.sh dibuat untuk backup /home ke /backup dan dijadwalkan lewat cron setiap jam pada menit 10.
Backup tambahan dilakukan ke flashdisk FAHAMI menggunakan rsync. Semua proses backup dipantau dengan Cronitor, mengirim status START / SUCCESS / FAIL ke dashboard dan email, dengan Grace Period 1 menit dan reminder alert 1 menit jika job gagal.

```bash
# Backup /etc
tar -czf etc_backup.tar.gz /etc

# Restore ke /tmp/etc_restore
mkdir -p /tmp/etc_restore
tar -xzf backup_etc.tar.gz -C /tmp/etc_restore

# Script backup.sh untuk /home/irham440 → /backup dan menyimpan output ke file log
#!/bin/bash
DATE=$(date +%Y%m%d)
tar -czf /backup/backup_home_$DATE.tar.gz /home/irham440 >> /backup/backup_home_$DATE.log

# Cron job tiap jam pada menit 10
10 * * * * /home/irham440/backup.sh

# Backup tambahan ke flashdisk FAHAMI
rsync -ah /home/irham440 /media/irham440/FAHAMI/Backup_home

# Monitoring backup dengan Cronitor
cronitor exec Backup -- /home/irham440/backup.sh
