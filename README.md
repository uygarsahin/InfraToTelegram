# InfraToTelegram
Merhaba, 

Sizlerle Sisteminizdeki ESXi serverlarinizin CPU.Memory ve durumlarini kontrol eden bir python kodu paylasmak isterim. Zaman buldukca kodu gelistirmek istiyorum.

Bu scripti istediginiz gibi gelistirebilirsiniz. Boylece size uygun hale getirebilirsiniz.

**Gereksinimler:**
1. Linux sunucu
    Bu sunucu telegram adresine erisebilmeli.
2. Python 
3. Gerekli paketler:
   3.1 python telebot paketi
   3.2 python pyVim
   3.3 python pyVmomi

**Code Hakkinda kisa bilgi:**
1. log dosyasinin nereye kayit etmesini istiyorsaniz "log Parameterleri" kisminda bulunan "logFileName" path'ini deigistirmeniz yeterli.
2. Telegram'a mesaj gonderebilmek icin TOKEN ve Chat ID yenigtiyaciniz var. Bu bilgileri telegramdan elde edebilirsiniz.
3. Vcenter bilgilerini "vcs" listesine eklemelisiniz. Ben ornek olarak ESX1_FQDN seklinde yazdim. 
4. Python scriptinin calisacagi Linux sunucu ile Vcenterlar ve ESX ler arasinda rest api portlari acik olmalidir.
5. cpu_threshold  ve memory_threshold degerlerini degistirerek esik degerleri belirleye bilirisiniz.


--------------------------------------------------------------------------------
#**ENGLISH**
Hello, 

I would like to share with you a python code that checks the CPU.Memory and status of your ESXi servers in your system. I want to improve the code when I find time.

You can develop this script as you want. So you can make it suitable for you.

**Requirements:**
1. Linux server
    This server should be able to access the telegram address.
2. Python 
3. Required packages:
   3.1 python telebot package
   3.2 python pyVim
   3.3 python pyVmomi

**Information about Code:**
1. if you want the log file to save where you want it to save, just change the “logFileName” path in the “log Parameters” section.
2. You need a TOKEN and a Chat ID to send a message to Telegram. You can get this information from telegram.
3. You need to add the Vcenter information to the “vcs” list. I wrote ESX1_FQDN as an example. 
4. The rest api ports must be open between the Linux server where the Python script will run and the Vcenters and ESXs.
5. You can set threshold values by changing the cpu_threshold and memory_threshold values.
