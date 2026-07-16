# MikroTik-Backup-via-Email
Mikrotik Ros v.6 &amp; v.7


========================script V.7========================

#########################################
# AUTO BACKUP + EMAIL
# RouterOS 7
#########################################

:local email "email-penerima@gmail.com"

:local router [/system identity get name]
:local date [/system clock get date]
:local time [/system clock get time]

# Format nama file
:local filename ($router . "-" . [:pick $date 7 11] . [:pick $date 0 3] . [:pick $date 4 6] . "-" . [:pick $time 0 2] . [:pick $time 3 5] . [:pick $time 6 8])

:local subject ("Backup Router - " . $router)
:local body ("Backup otomatis " . $router . " | " . $date . " " . $time)
:local files ($filename . ".rsc," . $filename . ".backup")

:log warning "START BACKUP"

# Export
/export file=$filename

:delay 3s

# Binary Backup
/system backup save name=$filename

:delay 5s

# Kirim Email
/tool e-mail send \
to=$email \
subject=$subject \
body=$body \
file=$files

:log warning "EMAIL TERKIRIM"

:delay 15s

# Hapus file lokal
/file remove ($filename . ".rsc")
/file remove ($filename . ".backup")

:log warning "SELESAI"

========================script V.6========================

#### CONFIG ####
:local toemail "email-penerima@gmail.com"

#### AMBIL INFO ROUTER ####
:local sysname [/system identity get name]
:local time [/system clock get time]
:local date [/system clock get date]

#### FORMAT DATE (hapus /) ####
:local newdate ""
:for i from=0 to=([:len $date]-1) do={
    :local tmp [:pick $date $i]
    :if ($tmp != "/") do={
        :set newdate ($newdate . $tmp)
    }
}

#### HILANGKAN SPAASI DI NAMA ROUTER ####
:if ([:find $sysname " "] != -1) do={
    :local newname ""
    :for i from=0 to=([:len $sysname]-1) do={
        :local tmp [:pick $sysname $i]
        :if ($tmp = " ") do={
            :set newname ($newname . "_")
        } else={
            :set newname ($newname . $tmp)
        }
    }
    :set sysname $newname
}

#### NAMA FILE ####
:local textfilename ($newdate . "-" . $sysname . ".rsc")
:local backupfilename ($newdate . "-" . $sysname . ".backup")

#### BUAT BACKUP ####
:log info "Creating backup files..."
/export file=$textfilename
/system backup save name=$backupfilename

#### TUNGGU FILE BENAR-BENAR ADA ####
:log info "Waiting for files..."

:while ([:len [/file find name=$textfilename]] = 0) do={
    :delay 1s
}

:while ([:len [/file find name=$backupfilename]] = 0) do={
    :delay 1s
}

#### KIRIM EMAIL ####
:log info "Sending backup via email..."

# kirim export (.rsc)
/tool e-mail send to=$toemail \
subject=("Backup RSC " . $sysname . " " . $time) \
body=("Backup konfigurasi (RSC) dari " . $sysname . " pada " . $date . " " . $time) \
file=$textfilename

# kirim full backup (.backup)
/tool e-mail send to=$toemail \
subject=("Backup FULL " . $sysname . " " . $time) \
body=("Backup full router dari " . $sysname . " pada " . $date . " " . $time) \
file=$backupfilename

#### TUNGGU SEBENTAR BIAR EMAIL TERKIRIM ####
:delay 5s

#### HAPUS FILE ####
:log info "Cleaning up backup files..."
/file remove $textfilename
/file remove $backupfilename

:log info "Backup selesai & terkirim"
