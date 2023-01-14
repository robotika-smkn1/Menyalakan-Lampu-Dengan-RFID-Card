

# :pushpin: Menyalakan-Lambu-Dengan-RFID-Card



<p align="center">
  <img src="https://i.postimg.cc/tRZw0xQ4/logo-removebg-preview.png" alt="robotika smkn1 kotabekasi logo"/ style="height:350px;" "width: 350px;">
</p>


[![Version](https://img.shields.io/badge/VENOM-1.0.17-brightgreen.svg?maxAge=259200)]()
[![Stage](https://img.shields.io/badge/Release-Stable-brightgreen.svg)]()
![licence](https://img.shields.io/badge/license-GPLv3-brightgreen.svg)
[![Arduino Build Status](https://buildbot.aircrack-ng.org/badges/aircrack-ng-alpine.svg?left_text=Alpine%20Linux%20Build)](##Link##)
[![Kali Linux Build Status](https://buildbot.aircrack-ng.org/badges/aircrack-ng-kali.svg?left_text=Kali%20Linux%20Build)](##Link##)
[![Armel Kali Linux Build Status](https://buildbot.aircrack-ng.org/badges/aircrack-ng-armel.svg?left_text=Armel%20Kali%20Linux%20Build)](##Link##)
[![Armhf Kali Linux Build Status](https://buildbot.aircrack-ng.org/badges/aircrack-ng-armhf.svg?left_text=Armhf%20Kali%20Linux%20Build)](##Link##)
[![DragonFly BSD Build Status](https://buildbot.aircrack-ng.org/badges/aircrack-ng-dfly.svg?left_text=DragonFly%20Build)](##Link##)
[![FreeBSD 11 Build Status](https://buildbot.aircrack-ng.org/badges/aircrack-ng-fbsd-11.svg?left_text=FreeBSD%2011%20Build)](##Link##)
[![FreeBSD 12 Build Status](https://buildbot.aircrack-ng.org/badges/aircrack-ng-fbsd-12.svg?left_text=FreeBSD%2012%20Build)](##Link##)
[![OpenBSD 6 Build Status](https://buildbot.aircrack-ng.org/badges/aircrack-ng-obsd.svg?left_text=OpenBSD%20Build)](##Link##)
[![NetBSD 8.1 Build Status](https://buildbot.aircrack-ng.org/badges/aircrack-ng-netbsd81.svg?left_text=NetBSD%20Build)](##Link##)
[![Coverity Scan Build Status](https://scan.coverity.com/projects/aircrack-ng/badge.svg)](##Link##)



## :inbox_tray: Media Social Instagram

To keep this collection up-to-date need contributors who can add more Program Arduino scripts
||
|--------------|
|:mailbox_with_mail: [artha_sa_](https://www.instagram.com/artha_sa_/)
|:mailbox_with_mail: [dicky_asqaelani26](https://www.instagram.com/dicky_asqaelani26/)
|:mailbox_with_mail: [rahmatnurraya](https://www.instagram.com/rahmatnurraya990/)
|:mailbox_with_mail: [applepi_fthur](https://www.instagram.com/applepi_fthur/)
|:mailbox_with_mail: [robotika-smkn1](https://www.instagram.com/robotika.smkn1kotabekasi/)


# :moneybag: [Donate](https://saweria.co/arthasyarif)

# :briefcase: Barang & Bahan
- Arduino Uno
- Buzzer
- Rfid Card
- Rfid Reader
- Relay 1 Chanel ( Bisa Di Tambah Sesuai Kebutuhan )
- Kebel Male & Female
- Lampu Arus Ac


# :mag: Ilustrasi Arduino

<p align="center">
  <img src="https://i.postimg.cc/yd3YM8y3/Ilustrasi-Rfid.jpg" style="height:205px;" "width:205px;"/>
</p>


# :clipboard: Source Code

```bash

/*
 * 
 * All the resources for this project: smkn1kotabekasi.sch.id
 * Modified by Robotika smkn1 kotabekasi
 * 
 * Created by Robotika smkn1 kotabekasi
 * 
 */
 
 // menambah library program
#include <SPI.h>
#include <MFRC522.h>
#include <Wire.h>

// inialisasi pin RFID, buzzer, dan relay
#define SS_PIN 10
#define RST_PIN 9
MFRC522 mfrc522(SS_PIN, RST_PIN);
int pinBuzzer = 8;
int pinRelay = 7;

// inialisasi variabel kondisi
int kondisi;


// ===================== PROGRAM PENGATURAN AWAL ======================= //

void setup()
{
  // inialisasi baud rate serial monitor
  Serial.begin(9600); // Initiate a serial communication
  SPI.begin(); // Initiate SPI bus
  mfrc522.PCD_Init(); // Initiate MFRC522

  // inialisasi status I/O pin
  pinMode(pinBuzzer, OUTPUT);
  pinMode(pinRelay, OUTPUT);

  // mematkan buzzer dan relay di awal program
  digitalWrite(pinBuzzer, HIGH);
  digitalWrite(pinRelay, HIGH);

  // kondisi awal = 0
  kondisi = 0;
}

// ============================== PROGRAM UTAMA ============================== //

void loop()
{

  // cek kartu RFID baru
  if ( ! mfrc522.PICC_IsNewCardPresent())
  {
    return;
  }

  // memilih kartu RFID
  if ( ! mfrc522.PICC_ReadCardSerial())
  {
    return;
  }

  // menampilkan ID kartu RFID pada Serial Monitor
  Serial.print("UID tag :");
  String content = "";
  byte letter;
  for (byte i = 0; i < mfrc522.uid.size; i++)
  {
    Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " ");
    Serial.print(mfrc522.uid.uidByte[i], HEX);

    content.concat(String(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " "));
    content.concat(String(mfrc522.uid.uidByte[i], HEX));
  }

  content.toUpperCase();

  // *** PROGRAM JIKA KARTU RFID SESUAI DENGAN YANG TERDAFTAR *** //
  // ****** ubah ID katu RFID yang ingin didaftarkan di sini ****** //
  if (content.substring(1) == "67 DF 74 E3")
  {


    // PROGRAM "ON" alat

    // jika kondisi = 0
    if (kondisi == 0)
    {
      // relay dinyalakan
      // alat dalam kondisi "ON"
      // buzzer menyala
      digitalWrite(pinRelay, LOW);
      digitalWrite(pinBuzzer, LOW);
      delay(1000);
      // buzzer dimatikan
      digitalWrite(pinBuzzer, HIGH);
      delay(1000);
      // kondisi menjadi = 1
      kondisi = 1;
    }

    // PROGRAM "OFF" alat

    // jika kondisi = 1
    else if (kondisi == 1)
    {
      // relay dimatikan
      // alat dalam kondisi "ON"
      // buzzer menyala
      digitalWrite(pinRelay, HIGH);
      digitalWrite(pinBuzzer, LOW);
      delay(1000);
      // buzzer dimatikan
      digitalWrite(pinBuzzer, HIGH);
      delay(1000);
      // kondisi menjadi = 0
      kondisi = 0;
    }
  }

  // PROGRAM JIKA KARTU RFID YANG DIGUNAKAN SALAH ATAU TIDAK TERDAFTAR

  else {
    // buzzer berbunyi pendek 3 kali
    digitalWrite(pinBuzzer, LOW);
    delay(300);
    digitalWrite(pinBuzzer, HIGH);
    delay(300);
    digitalWrite(pinBuzzer, LOW);
    delay(300);
    digitalWrite(pinBuzzer, HIGH);
    delay(300);
    digitalWrite(pinBuzzer, LOW);
    delay(300);
    digitalWrite(pinBuzzer, HIGH);
    delay(300);
  }
}

 

```
