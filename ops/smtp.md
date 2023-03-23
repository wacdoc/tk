# Öz SMTP poçta iberiş serweriňizi guruň

## preamble

SMTP bulut satyjylaryndan hyzmatlary gönüden-göni satyn alyp biler, meselem:

* [Amazon SES SMTP](https://docs.aws.amazon.com/ses/latest/dg/send-email-smtp.html)
* [Ali bulut e-poçta basmagy](https://www.alibabacloud.com/help/directmail/latest/three-mail-sending-methods)

Şeýle hem öz poçta serweriňizi gurup bilersiňiz - çäklendirilmedik ibermek, umumy bahasy az.

Aşakda öz poçta serwerimizi nädip gurmalydygyny ädimme-ädim görkezýäris.

## Serwer saýlamak

Özbaşdak ýerleşdirilen SMTP serweri 25, 456 we 587 portlary açyk bolan umumy IP talap edýär.

Köplenç ulanylýan jemgyýetçilik bulutlary bu portlary deslapky görnüşde petikledi we iş buýrugy bilen açmak mümkin bolmagy mümkin, ýöne bu gaty kyn.

Bu portlary açyk we ters domen atlaryny gurmagy goldaýan öý eýesinden satyn almagy maslahat berýärin.

Bu ýerde men [Contabo-ny](https://contabo.com) maslahat berýärin.

Contabo, Germaniýanyň Mýunhen şäherinde ýerleşýän, 2003-nji ýylda gaty bäsdeşlik bahalary bilen esaslandyrylan hosting üpjünçisi.

Euroewrony satyn alyş walýutasy hökmünde saýlasaňyz, bahasy arzan bolar (8 Gb ýady we 4 protsessorly serwer ýylda 529 ýuana deňdir we başlangyç gurnama tölegi bir ýyl mugt).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/UoAQkwY.webp)

Sargyt ýerleşdirilende bellik `prefer AMD` we AMD CPU bilen serwer has gowy öndürijilik gazanar.

Aşakda, öz poçta serweriňizi nädip gurmalydygyny görkezmek üçin Contabo-nyň VPS-ni mysal hökmünde alaryn.

## Ubuntu ulgamynyň konfigurasiýasy

Bu ýerdäki operasiýa ulgamy Ubuntu 22.04

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/smpIu1F.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/m7Mwjwr.webp)

Ssh-de serwer görkezilse, `Welcome to TinyCore 13!` (aşakdaky suratda görkezilişi ýaly), ulgamyň entek gurulmandygyny aňladýar. Ssh-i aýyryň we gaýtadan girmek üçin birnäçe minut garaşyň.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/-qKACz9.webp)

`Welcome to Ubuntu 22.04.1 LTS` , başlangyç gutardy we aşakdaky ädimleri dowam etdirip bilersiňiz.

### [Meýletin] Ösüş gurşawyny başlaň

Bu ädim islege bagly däl.

Amatlylyk üçin ubuntu programma üpjünçiliginiň gurnama we ulgam konfigurasiýasyny [github.com/wactax/ops.os/tree/main/ubuntu](https://github.com/wactax/ops.os/tree/main/ubuntu) goýdum.

Bir gezek basmak bilen gurmak üçin aşakdaky buýrugy işlediň.

```
bash <(curl -s https://raw.githubusercontent.com/wactax/ops.os/main/ubuntu/boot.sh)
```

Hytaýly ulanyjylar, ýerine aşakdaky buýrugy ulanyň, dil, wagt guşaklygy we ş.m. awtomatiki usulda düzüler.

```
CN=1 bash <(curl -s https://ghproxy.com/https://raw.githubusercontent.com/wactax/ops.os/main/ubuntu/boot.sh)
```

### Contabo IPV6-y işledýär

SMTP IPV6 salgylary bilen e-poçta iberip biler ýaly IPV6-ny işlediň.

`/etc/sysctl.conf` redaktirläň

Aşakdaky setirleri üýtgediň ýa-da goşuň

```
net.ipv6.conf.all.disable_ipv6 = 0
net.ipv6.conf.default.disable_ipv6 = 0
net.ipv6.conf.lo.disable_ipv6 = 0
```

[“Contabo” gollanmasyny yzarlaň: Serweriňize IPv6 birikmesini goşmak](https://contabo.com/blog/adding-ipv6-connectivity-to-your-server/)

`/etc/netplan/01-netcfg.yaml` redaktirläň, aşakdaky suratda görkezilişi ýaly birnäçe setir goşuň (Contabo VPS deslapky konfigurasiýa faýlynda eýýäm bu setirler bar, diňe bir zat däl).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/5MEi41I.webp)

Soňra üýtgedilen konfigurasiýanyň güýje girmegi üçin `netplan apply` .

Sazlama üstünlikli bolansoň, daşarky ulgamyňyzyň ipv6 salgysyny görmek üçin `curl 6.ipw.cn` ulanyp bilersiňiz.

## Sazlama ammar opslaryny klonlaň

```
git clone https://github.com/wactax/ops.soft.git
```

## Domen adyňyz üçin mugt SSL şahadatnamasyny dörediň

Poçta ibermek, şifrlemek we gol çekmek üçin SSL şahadatnamasyny talap edýär.

Şahadatnamalar döretmek üçin [acme.sh](https://github.com/acmesh-official/acme.sh) ulanýarys.

acme.sh açyk çeşme awtomatiki şahadatnama gol çekiş guraly,

Ops.soft konfigurasiýa ammaryna giriň, `./ssl.sh` işlediň we **ýokarky bukjada** `conf` bukjasy dörediler.

DNS üpjün edijiňizi [acme.sh dnsapi](https://github.com/acmesh-official/acme.sh/wiki/dnsapi) -den tapyň, `conf/conf.sh` redaktirläň.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/Qjq1C1i.webp)

Soňra domen adyňyz üçin `123.com` we `*.123.com` şahadatnamalaryny döretmek üçin `./ssl.sh 123.com` işlediň.

Ilkinji iş awtomatiki usulda [acme.sh](https://github.com/acmesh-official/acme.sh) gurar we awtomatiki täzelenmek üçin meýilleşdirilen meseläni goşar. `crontab -l` görüp bilersiňiz, aşakdaky ýaly setir bar.

```
52 0 * * * "/mnt/www/.acme.sh"/acme.sh --cron --home "/mnt/www/.acme.sh" > /dev/null
```

Döredilen şahadatnamanyň ýoly `/mnt/www/.acme.sh/123.com_ecc。` like ýaly bir zat

Şahadatnamanyň täzelenmegi `conf/reload/123.com.sh` skriptine jaň eder, bu skripti redaktirlär, degişli programmalaryň sertifikat keşini täzelemek üçin `nginx -s reload` ýaly buýruklary goşup bilersiňiz.

## Çasquid bilen SMTP serwerini guruň

[chasquid](https://github.com/albertito/chasquid) Go dilinde ýazylan açyk çeşme SMTP serweridir.

“Postfix” we “Sendmail” ýaly gadymy poçta serwer programmalarynyň ornuny tutýan “chasquid” has ýönekeý we ulanmak aňsat, ikinji derejeli ösüş üçin hem aňsat.

Işlediň `./chasquid/init.sh 123.com` bir gezek basmak bilen awtomatiki usulda gurlar (123.com iberýän domen adyňyz bilen çalşyň).

## E-poçta goluny DKIM sazlaň

DKIM hatlara spam hökmünde garalmazlygy üçin e-poçta gollaryny ibermek üçin ulanylýar.

Buýruk üstünlikli işlänsoň, size DKIM ýazgysyny bellemek soralar (aşakda görkezilişi ýaly).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/LJWGsmI.webp)

DNS-ä diňe TXT ýazgysyny goşuň (aşakda görkezilişi ýaly).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/0szKWqV.webp)

## Hyzmatyň ýagdaýyny we gündeligini görüň

 `systemctl status chasquid` Hyzmat ýagdaýyny görmek.

Adaty işleýiş ýagdaýy aşakdaky suratda görkezilişi ýaly

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/CAwyY4E.webp)

 `grep chasquid /var/log/syslog` ýa-da `journalctl -xeu chasquid` säwlik gündeligini görüp biler.

## Domen adynyň konfigurasiýasyny tersine

Ters domen ady, IP adresi degişli domen adyna çözülmegine rugsat bermekdir.

Tersine domen adyny bellemek, e-poçtalaryň spam diýip kesgitlenmeginiň öňüni alyp biler.

Poçta alnanda, kabul ediji serwer iberiji serweriň ters domen adynyň bardygyny ýa-da ýokdugyny tassyklamak üçin iberilýän serweriň IP adresinde ters domen adynyň derňewini amala aşyrar.

Iberýän serweriň ters domen ady ýok bolsa ýa-da ters domen ady iberilýän serweriň IP adresine laýyk gelmeýän bolsa, kabul ediji serwer e-poçta spam diýip tanap biler ýa-da ret edip biler.

[Https://my.contabo.com/rdns](https://my.contabo.com/rdns) girip görüň we aşakda görkezilişi ýaly düzüň

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/IIPdBk_.webp)

Ters domen adyny belläniňizden soň, domen adynyň ipv4 we ipv6 domeniniň çözgüdini serwere düzmegi ýatdan çykarmaň.

## Chasquid.conf-iň baş adyny redaktirläň

`conf/chasquid/chasquid.conf` ters domen adynyň bahasyna üýtgediň.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/6Fw4SQi.webp)

Soňra hyzmaty täzeden başlamak üçin `systemctl restart chasquid` işlediň.

## Git ammaryna ätiýaçlyk nusgasy

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/Fier9uv.webp)

Mysal üçin, konflikt bukjasyny aşakdaky ýaly öz github prosessime ätiýaç edýärin

Ilki bilen hususy ammar dörediň

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/ZSCT1t1.webp)

Conf katalogyna giriň we ammara tabşyryň

```
git init
git add .
git commit -m "init"
git branch -M main
git remote add origin git@github.com:wac-tax-key/conf.git
git push -u origin main
```

## Iberiji goşuň

ylga

```
chasquid-util user-add i@wac.tax
```

Iberiji goşup biler

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/khHjLof.webp)

### Parolyň dogry gurnalandygyny barlaň

```
chasquid-util authenticate i@wac.tax --password=xxxxxxx
```

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/e92JHXq.webp)

Ulanyjy goşandan soň, `chasquid/domains/wac.tax/users` täzelener, ammara ibermegi ýatdan çykarmaň.

## DNS SPF ýazgysyny goşýar

SPF (Iberiji syýasaty çarçuwasy) e-poçta galplygynyň öňüni almak üçin ulanylýan e-poçta tassyklaýyş tehnologiýasydyr.

Poçta iberijiniň şahsyýetini, iberijiniň IP adresiniň domen adynyň DNS ýazgylaryna gabat gelýändigini barlap, kezzaplaryň galp e-poçta ibermeginiň öňüni alýar.

SPF ýazgylaryny goşmak, e-poçtalaryň mümkin boldugyça spam hökmünde kesgitlenmeginiň öňüni alyp biler.

Domen adyňyz serweri SPF görnüşini goldamaýan bolsa, diňe TXT görnüşli ýazgy goşuň.

Mysal üçin, `wac.tax` -nyň SPF-i aşakdaky ýaly

`v=spf1 a mx include:_spf.wac.tax include:_spf.google.com ~all`

`_spf.wac.tax` üçin SPF

`v=spf1 a:smtp.wac.tax ~all`

Bu ýerde `include:_spf.google.com` , munuň sebäbi `i@wac.tax` soň Google poçta gutusyna iberiş salgysy hökmünde düzerin.

## DNS konfigurasiýasy DMARC

DMARC gysgaltmasydyr (Domain esasly habary tassyklamak, hasabat bermek we ýerine ýetiriş).

SPF duralgalaryny ele almak üçin ulanylýar (belki konfigurasiýa säwliklerinden dörän bolmagy mümkin ýa-da başga biri sizi spam iberýän ýaly edip görkezýär).

TXT ýazgysyny goşuň `_dmarc` ,

Mazmuny aşakdaky ýaly

```
v=DMARC1; p=quarantine; fo=1; ruf=mailto:ruf@wac.tax; rua=mailto:rua@wac.tax
```

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/k44P7O3.webp)

Her parametriň manysy aşakdaky ýaly

### p (Syýasat)

SPF (Iberiji Syýasat Çarçuwasy) ýa-da DKIM (DomainKeys Identified Mail) tassyklamasy şowsuz e-poçtalary nädip dolandyrmalydygyny görkezýär. P parametrini üç bahanyň birine düzüp bolýar:

* hiç biri: Hiç hili çäre görülmeýär, diňe barlamak netijesi e-poçta hasabat mehanizmi arkaly iberijä yzyna iberilýär.
* Karantin: Barlagdan geçmedik haty spam bukjasyna salyň, ýöne haty göni ret etmeýär.
* ret etmek: Barlamagy başarmaýan e-poçtalary göni ret ediň.

### fo (Şowsuzlyk opsiýalary)

Hasabat mehanizmi tarapyndan yzyna gaýtarylýan maglumatlaryň mukdaryny kesgitleýär. Aşakdaky gymmatlyklaryň birine belläp bolar:

* 0: allhli habarlar üçin tassyklama netijelerini habar beriň
* 1: Diňe barlanylmaýan habarlary habar beriň
* d: Diňe domen adyny barlamakdaky näsazlyklary habar beriň
* s: diňe SPF barlagynyň näsazlyklaryny habar beriň
* l: Diňe DKIM barlamagyň näsazlyklaryny habar beriň

### rua & ruf

* rua (Jemi hasabatlar üçin URI hasabat bermek): Jemi hasabatlary almak üçin e-poçta salgysy
* ruf (Kazyýet hasabaty üçin URI hasabat bermek): jikme-jik hasabatlary almak üçin e-poçta salgysy

## Google Mail-e e-poçta ibermek üçin MX ýazgylaryny goşuň

Universalhliumumy salgylary goldaýan mugt korporatiw poçta gutusyny tapyp bilmedigim üçin (Catch-All, bu domen adyna iberilen e-poçtalary, prefiksleri çäklendirmezden alyp biler), ähli e-poçtalary Gmail poçta gutusyma ugratmak üçin chasquid ulandym.

**Öz tölegli iş poçta gutyňyz bar bolsa, MX-ni üýtgetmäň we bu ädimden geçmäň.**

`conf/chasquid/domains/wac.tax/aliases` redaktirläň, poçta gutusyny ugradyň

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/OBDl2gw.webp)

`*` ähli e-poçtalary görkezýär, `i` ýokarda döredilen iberiji ulanyjynyň e-poçta salgysynyň prefiksi. Poçta ugratmak üçin her bir ulanyjy setir goşmaly.

Soňra MX ýazgysyny goşuň (aşakdaky suratyň birinji setirinde görkezilişi ýaly göni ters domen adynyň salgysyna salgylanýaryn).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/7__KrU8.webp)

Sazlama tamamlanandan soň, Gmail-de e-poçta alyp biljekdigiňizi görmek üçin `i@wac.tax` we `any123@wac.tax` e-poçta ibermek üçin beýleki e-poçta salgylaryny ulanyp bilersiňiz.

Notok bolsa, chasquid gündeligini barlaň ( `grep chasquid /var/log/syslog` ).

## Google Mail bilen i@wac.tax e-poçta iberiň

Google Mail haty alandan soň, i.wac.tax@gmail.com ýerine derek `i@wac.tax` bilen jogap bermäge umyt etdim.

[Https://mail.google.com/mail/u/1/#settings/accounts](https://mail.google.com/mail/u/1/#settings/accounts) girip, "Başga e-poçta salgysyny goş" düwmesine basyň.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/PAvyE3C.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/_OgLsPT.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/XIUf6Dc.webp)

Soň bolsa, ugradylan e-poçta bilen alnan tassyklama koduny giriziň.

Netijede, deslapky iberiji salgysy hökmünde kesgitlenip bilner (şol bir salgy bilen jogap bermek mümkinçiligi bilen birlikde).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/a95dO60.webp)

Şeýlelik bilen, SMTP poçta serwerini döretmegi tamamladyk we şol bir wagtyň özünde e-poçta ibermek we almak üçin Google Mail-den peýdalanýarys.

## Sazlamanyň üstünlikli ýa-da ýokdugyny barlamak üçin synag e-poçta iberiň

`ops/chasquid` giriziň

Run `direnv allow` (direnv öňki bir açar başlangyç işinde guruldy we gabyga çeňňek goşuldy)

soň ylga

```
user=i@wac.tax pass=xxxx to=iuser.link@gmail.com ./sendmail.coffee
```

Parametrleriň manysy aşakdaky ýaly

* ulanyjy: SMTP ulanyjy ady
* pass: SMTP paroly
* : alyjy

Synag e-poçta iberip bilersiňiz.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/ae1iWyM.webp)

Sazlamalaryň üstünlikli ýa-da ýokdugyny barlamak üçin synag e-poçtalaryny almak üçin Gmail-den peýdalanmak maslahat berilýär.

### TLS standart şifrlemek

Aşakdaky suratda görkezilişi ýaly, SSL şahadatnamasynyň üstünlikli işledilendigini aňladýan bu kiçijik gulp bar.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/SrdbAwh.webp)

Soňra "Asyl e-poçta görkez" -e basyň

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/qQQsdxg.webp)

### DKIM

Aşakdaky suratda görkezilişi ýaly, Gmail asyl poçta sahypasynda DKIM görkezilýär, bu bolsa DKIM konfigurasiýasynyň üstünlikli bolandygyny aňladýar.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/an6aXK6.webp)

Asyl e-poçta sözbaşysynda alnanlary barlaň we iberijiniň adresiniň IPV6dygyny görüp bilersiňiz, bu bolsa IPV6-nyň hem üstünlikli düzülendigini aňladýar.
