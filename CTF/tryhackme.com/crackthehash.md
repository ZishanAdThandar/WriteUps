# CrackTheHash

Room Link: https://tryhackme.com/room/crackthehash

Badges: https://tryhackme.com/ZishanAdThandar/badges/hash-cracker

## Tools 

1. hashcat
2. hashid
3. hash-identifier
4. rockyou.txt

## Level 1

1. I googled the first hash `48bb6e862e54f2a795ffc4e541caed4d`. Found this link https://md5.gromweb.com/?md5=48bb6e862e54f2a795ffc4e541caed4d So, It is nd5 of string `easy`. Also running hashcat with `hashcat -D 2 -m 0 hash.txt rockyou.txt` gives cracked string as `easy`.
2. Used `hashid` on second hash with `hashid CBFDAC6008F9CAB4083784CBD1874F76618D2A97` and it shows SHA1 as one of the probable hash. So, again runnning hashcat with `-m 100` gives us answer `password123`.
3. Again used hashid on hash `1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032`, it shows SHA256. So, used hashcat with `-m 1400` gives the answer `letmein`.
4. Again hash id method shows hash `$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom` is bcrypt blowfish. Similarly hashcat with `-m 3200` gives answer `bleh`.
5. hashid shows hash `279412f945939ba78ce0758d3fd83daa` could be md5, md2, md4 etc. After trying hashcat for different hash types, got the answer for md4 with `-m 900` is `Eternity22`.

## Level 2

1. 
